---
title: "Log Shipper Configuration"
date: 2026-11-10T10:00:00+09:00
weight: 40
pre: "<b>D. </b>"
draft: false
---

{{< line_break >}}
##### 4. Configure the log shipper (Fluent Bit)
The kcnd log is shipped by Fluent Bit, a separate process from Telegraf. Whatever happens to log shipping, Telegraf metrics and the kcn process are not affected.

###### 1) Install Fluent Bit
Fluent Bit's official installer detects your distribution and sets up the repository itself.
The package's own ```fluent-bit.service``` runs a demo configuration, so disable it. The ```kaia-log-shipper.service``` below is used instead.
{{< highlight bash >}}
$ curl -fsSL https://raw.githubusercontent.com/fluent/fluent-bit/master/install.sh | sh
$ sudo systemctl disable --now fluent-bit
$ /opt/fluent-bit/bin/fluent-bit --version
{{< /highlight >}}

{{< line_break >}}

###### 2) Create the ```fluent-bit``` user
The log shipper runs as an unprivileged user.
{{< highlight bash >}}
$ sudo groupadd -r fluent-bit
$ sudo useradd -r -g fluent-bit -d /home/fluent-bit -s /sbin/nologin -c "Fluent Bit" fluent-bit
{{< /highlight >}}

{{< line_break >}}

###### 3) Create ```/etc/fluent-bit/kaia-logs.conf```
Replace ```<hostname>``` with the same value as ```hostname``` in ```/etc/telegraf/telegraf.d/kaia.conf``` (e.g. ```abc-cn```).

```vim
# kaia-logs.conf
#
# Ships this node's kcnd log to VictoriaLogs via Fluent Bit rather than
# telegraf: telegraf's own InfluxDB output would exit entirely if a second,
# log-shipping output (e.g. Elasticsearch) failed its connection handshake,
# which would silently stop this node's METRICS too. A separate process
# can't do that — whatever happens to log shipping, telegraf and the chain
# process are untouched.

[SERVICE]
    Flush                     5
    Log_Level                 info
    # Node-side spill buffer. Only holds data while the upstream is
    # unreachable, and empties itself once delivery resumes. A queue, not a
    # log store.
    storage.path              /var/lib/fluent-bit/chunks
    # Write chunks through to disk rather than relying on the page cache —
    # the setting that makes a hard kill survivable.
    storage.sync              normal
    storage.checksum          on
    storage.max_chunks_up     128

[INPUT]
    Name                      tail
    Path                      /kaia/kcnd/logs/kcnd.out
    # Read position, so an agent restart neither re-ships nor skips. Must
    # live on persistent storage, not /tmp.
    DB                        /var/lib/fluent-bit/tail.db
    DB.sync                   normal
    storage.type              filesystem
    # Start at the end of the file on first install — shipping the node's
    # entire existing log history on day one would be hours of backlog for
    # no benefit.
    Read_from_Head            false
    # A Kaia log line is ~211 bytes; anything past 32k is not a log line.
    Skip_Long_Lines           on
    Buffer_Chunk_Size         32k
    Buffer_Max_Size           64k
    Tag                       kaia

[FILTER]
    Name                      record_modifier
    Match                     kaia
    # These three become the VictoriaLogs stream, which is what the store
    # partitions and compresses by. Identity only — nothing with open
    # cardinality, and deliberately not the file path (which drifts across
    # the fleet) so each node is its own stream regardless of path shape.
    Record                    instance <hostname>
    Record                    nodetype cn
    Record                    network  mainnet

[OUTPUT]
    Name                      http
    Match                     kaia
    host                      logs.node.kaia.io
    port                      45560
    # No TLS — a deliberate carry-over, same as the metrics path beside it
    # (metrics.node.kaia.io:45560): plain HTTP reachable only from an IP
    # allowlist. Node logs are block numbers/hashes/peer counts, not
    # secrets, but raising this whole ingest surface to TLS is tracked as a
    # separate, not-yet-done item.
    uri                       /insert/jsonline?_msg_field=log&_time_field=date&_stream_fields=instance,nodetype,network
    format                    json_lines
    json_date_format          iso8601
    json_date_key             date
    compress                  gzip
    # Retry forever — the filesystem buffer above holds the backlog, and
    # giving up would turn a cluster restart into permanent loss.
    Retry_Limit               false
    # Cap what this node may hold in the local spill buffer.
    storage.total_limit_size  1G
```

{{< highlight bash >}}
$ sudo chmod 644 /etc/fluent-bit/kaia-logs.conf
{{< /highlight >}}

{{< line_break >}}

###### 4) Create ```/usr/local/bin/kaia-log-shipper-preflight.sh```
A startup guard that stops the unit before Fluent Bit starts if the log file is not readable or the buffer directory is not writable. Without it, a wrong configuration makes the unit look healthy while shipping nothing.

```vim
#!/bin/sh
# Startup guard for the Kaia node log shipper. Run by ExecStartPre; a
# non-zero exit stops the unit before Fluent Bit starts.
#
# It exists because every way this config can be wrong is SILENT: a wrong
# Path makes tail simply wait forever for a file that never appears, and the
# unit sits there Running and healthy, shipping nothing.
#
# Usage: kaia-log-shipper-preflight.sh /etc/fluent-bit/kaia-logs.conf
set -eu

CONF="${1:-/etc/fluent-bit/kaia-logs.conf}"

if [ ! -r "$CONF" ]; then
  echo "preflight: config not readable: $CONF" >&2
  exit 1
fi

# The file we intend to tail is there and readable AS THIS UNIT'S USER.
# Reading it as root would pass here and fail at runtime.
LOGPATH=$(sed -n 's/^[[:space:]]*Path[[:space:]]\{1,\}//p' "$CONF" | head -1)
if [ -z "$LOGPATH" ]; then
  echo "preflight: no Path found in $CONF" >&2
  exit 1
fi
if [ ! -r "$LOGPATH" ]; then
  echo "preflight: log file is not readable by $(id -un): $LOGPATH" >&2
  echo "preflight: add this user to the group that owns it, or fix the path" >&2
  exit 1
fi

# The buffer directory must be writable, or Fluent Bit silently degrades to
# memory-only buffering — the property that made us choose it in the first
# place.
BUFDIR=$(sed -n 's/^[[:space:]]*storage.path[[:space:]]\{1,\}//p' "$CONF" | head -1)
if [ -n "$BUFDIR" ]; then
  mkdir -p "$BUFDIR" 2>/dev/null || true
  if [ ! -w "$BUFDIR" ]; then
    echo "preflight: buffer dir is not writable by $(id -un): $BUFDIR" >&2
    exit 1
  fi
fi

echo "preflight: ok — tailing $LOGPATH, buffering in ${BUFDIR:-<none>}"
```

{{< highlight bash >}}
$ sudo chmod 755 /usr/local/bin/kaia-log-shipper-preflight.sh
{{< /highlight >}}

{{< line_break >}}

###### 5) Create ```/etc/systemd/system/kaia-log-shipper.service```

```vim
# /etc/systemd/system/kaia-log-shipper.service
# Installed alongside the node's telegraf unit; it replaces nothing and
# depends on nothing. Removing log shipping is
# `systemctl disable --now kaia-log-shipper` — the node's metrics path is
# not involved either way.

[Unit]
Description=Kaia node log shipper (Fluent Bit -> VictoriaLogs)
Documentation=https://docs.victoriametrics.com/victorialogs/data-ingestion/fluentbit/
After=network-online.target
Wants=network-online.target
# A node that stopped shipping because systemd rate-limited it would look
# identical to a node that is simply quiet. This belongs in [Unit], not
# [Service] — systemd moved it in 229 and the old placement is silently
# ignored on current releases.
StartLimitIntervalSec=0

[Service]
Type=simple

ExecStartPre=/usr/local/bin/kaia-log-shipper-preflight.sh /etc/fluent-bit/kaia-logs.conf
# Fluent Bit's own parser, so a malformed config fails here rather than in a
# restart loop.
ExecStartPre=/opt/fluent-bit/bin/fluent-bit --dry-run -c /etc/fluent-bit/kaia-logs.conf

ExecStart=/opt/fluent-bit/bin/fluent-bit -c /etc/fluent-bit/kaia-logs.conf

KillSignal=SIGTERM
TimeoutStopSec=60

Restart=always
RestartSec=10

# Unprivileged. It reads one log file and writes one buffer directory.
User=fluent-bit
Group=fluent-bit
StateDirectory=fluent-bit
NoNewPrivileges=true
PrivateTmp=true
ProtectSystem=strict
ProtectHome=true
# ProtectSystem=strict makes /var read-only; this is the one exception, for
# the chunk buffer and the tail offset DB.
ReadWritePaths=/var/lib/fluent-bit
ProtectKernelTunables=true
ProtectKernelModules=true
ProtectControlGroups=true
RestrictSUIDSGID=true
LockPersonality=true

# Bounded, so a runaway shipper cannot compete with the chain process.
MemoryMax=256M
CPUQuota=50%

[Install]
WantedBy=multi-user.target
```

{{< highlight bash >}}
$ sudo chmod 644 /etc/systemd/system/kaia-log-shipper.service
$ sudo systemctl daemon-reload
{{< /highlight >}}

_** Do not start ```kaia-log-shipper``` yet. The preflight check needs ```/kaia/kcnd/logs/kcnd.out```, which is created after kcnd starts. It will be started in [4-B. Start the monitoring services](/50_start_services/20_start_monitoring_services/)._

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
