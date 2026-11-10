---
title: "Log Shipper Configuration"
date: 2026-11-10T10:00:00+09:00
weight: 40
pre: "<b>E. </b>"
draft: false
---

{{< line_break >}}
##### 5. Configure the log shipper (Fluent Bit)
The kcnd log is shipped by Fluent Bit, a separate process from Telegraf. Whatever happens to log shipping, Telegraf metrics and the kcn process are not affected.

###### 1) Install Fluent Bit
Fluent Bit's official installer detects your distribution and sets up the repository itself.
{{< highlight bash >}}
$ curl -fsSL https://raw.githubusercontent.com/fluent/fluent-bit/master/install.sh | sh
$ /opt/fluent-bit/bin/fluent-bit --version
{{< /highlight >}}

{{< line_break >}}

###### 2) Create ```/etc/fluent-bit/kaia-logs.conf```
This file holds the log shipping configuration. Replace ```<hostname>``` with the same value as ```hostname``` in ```/etc/telegraf/telegraf.d/kaia.conf``` (e.g. ```abc-cn```).

{{< highlight bash >}}
$ sudo vi /etc/fluent-bit/kaia-logs.conf
{{< /highlight >}}

```vim
# /etc/fluent-bit/kaia-logs.conf
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

{{< line_break >}}

###### 3) Make ```/etc/fluent-bit/fluent-bit.conf``` include ```kaia-logs.conf```
The package's ```fluent-bit.service``` always reads ```/etc/fluent-bit/fluent-bit.conf```, which is a demo configuration by default. Back it up and replace the **whole file** with the single ```@INCLUDE``` line below.

{{< highlight bash >}}
$ sudo cp /etc/fluent-bit/fluent-bit.conf /etc/fluent-bit/fluent-bit.conf.orig
$ echo "@INCLUDE /etc/fluent-bit/kaia-logs.conf" | sudo tee /etc/fluent-bit/fluent-bit.conf
{{< /highlight >}}

{{% notice warning %}}
Do not keep the demo ```[INPUT]``` / ```[OUTPUT]``` sections in ```fluent-bit.conf```. They would run together with ```kaia-logs.conf```.
{{% /notice %}}

{{< line_break >}}

###### 4) Create the buffer directory and check the configuration
{{< highlight bash >}}
$ sudo mkdir -p /var/lib/fluent-bit
$ sudo /opt/fluent-bit/bin/fluent-bit --dry-run -c /etc/fluent-bit/fluent-bit.conf
{{< /highlight >}}
_** ```--dry-run``` checks ```fluent-bit.conf``` together with the included ```kaia-logs.conf```. If there is no error, the configuration is valid._

_** Do not start ```fluent-bit``` yet. It tails ```/kaia/kcnd/logs/kcnd.out```, which is created after kcnd starts. It will be started in [5-B. Start the monitoring services](/50_start_services/20_start_monitoring_services/)._

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
