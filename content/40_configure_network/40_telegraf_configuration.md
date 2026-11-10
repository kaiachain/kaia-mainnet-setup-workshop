---
title: "Telegraf Configuration"
date: 2022-07-14T16:44:50+09:00
weight: 30
pre: "<b>C. </b>"
draft: false
---

{{< line_break >}}
##### 3. Configure Telegraf for metrics

###### 1) Enable metrics in kcnd
Check if the two options below are enabled in ```kcnd.conf```.

```vim
...
METRICS=1
PROMETHEUS=1
...
```

After kcnd is started, you can check that port 61001 is open.

{{< highlight bash >}}
$ netstat -ntap | grep 61001
tcp        0      0 :::61001        :::*       LISTEN      8989/kcn
{{< /highlight >}}

{{< line_break >}}

###### 2) Set the collection intervals in ```/etc/telegraf/telegraf.conf```
Set the three values below under the ```[agent]``` section. If a line is commented out (```#```), uncomment it.

```vim
[agent]
  ...
  interval = "10s"
  round_interval = true
  ...
  flush_interval = "10s"
  ...
```

{{< line_break >}}

###### 3) Create ```/etc/telegraf/telegraf.d/kaia.conf```
Replace ```<hostname>``` with the name of your CN in the ```<org name>-cn``` format (e.g. ```abc-cn```).

{{% notice note %}}
Use the same ```<hostname>``` value for the ```instance``` of the log shipper in the next step. Otherwise, your node will be split into two identities in the metric and log dashboards.
{{% /notice %}}

```vim
[global_tags]
nodetype = "cn"
network = "mainnet"
[agent]
# The CN name: <org name>-cn (e.g. `abc-cn`)
hostname = "<hostname>"

[[inputs.procstat]]
  exe = "kcn"

[inputs.prometheus]
urls = [ "http://localhost:61001/metrics" ]

[[outputs.influxdb]]
  urls = ["http://metrics.node.kaia.io:45560"]
  database = "kaia-network-monitoring"

  namedrop = ["kaia_log"]
  skip_database_creation = true
```

{{< line_break >}}

---
You can check more details on the page below.
* https://docs.kaia.io/nodes/core-cell/monitoring-setup/

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
