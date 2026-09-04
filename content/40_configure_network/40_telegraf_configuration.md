---
title: "Telegraf Configuration"
date: 2022-07-14T16:44:50+09:00
weight: 50
pre: "<b>E. </b>"
draft: false
---

{{< line_break >}}
##### 1. Enable monitoring in kcnd/kpnd

###### 1) For both CN and PN,
Check the if below two options enabled.

>*`/etc/kcnd/conf/kcnd.conf`*
```vim
...
METRICS=1
PROMETHEUS=1
...
```

If two options enabled, You may check port 61001 is open.

{{< highlight html >}}
$ netstat -ntap | grep 61001
tcp        0      0 :::61001        :::*       LISTEN      8989/kcn
{{< /highlight >}}

{{< line_break >}}
### 2. Configure Telegraf service
Create new telegraf configuration file as kaia.conf under below path and add the following
the configuration.

>*`/etc/telegraf/telegraf.d/`*

Edit 'nodetype', 'instance', and 'hostname' appropriately for each node.

```vim
[global_tags]
# Change "cn" to "pn" for PN installation
nodetype = "cn"
# The CN/PN name (e.g. `klaytn-cn`, `klaytn-pn-01`)
instance = "<hostname>"

[agent]
# The CN/PN name (e.g. `klaytn-cn`, `klaytn-pn-01`)
hostname = "<hostname>"

[[inputs.procstat]]
# The process name (e.g. `kcn`, `kpn`)
  exe = "<k*n>"

[[outputs.influxdb]]
urls = [ "http://node.kaia.io:45560" ]
database = "klaytn_cypress"

namedrop = ["kaia_log"]
[inputs.prometheus]
urls = [ "http://localhost:61001/metrics" ]
```

###### 2) Only for CN,
Create new telegraf configuration file as kaia_log.conf under below path and add the following
the configuration.

>*`/etc/telegraf/telegraf.d/`*

```vim   
[[inputs.tail]]
  files = ["/var/kcnd/logs/kcnd.out"]
  from_beginning = false
  watch_method = "inotify"
  name_override = "kaia_log"
  data_format = "value"
  data_type = "string"

[[outputs.influxdb]]
  urls = ["http://node.kaia.io:45560"]
  database = "kaia_logs"
  namepass = ["kaia_log"]
  ```

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.

### 1. Enable monitoring in kcnd/kpnd


{{% notice note %}}
Please find the Private Network Dashboard as below URL. The Credentials will be provided separately.
{{% /notice %}}

* http://node.kaia.io:3000

{{< line_break >}}
{{< line_break >}}

---
{{< line_break >}}

You can check the more details requirements on the page below.
* https://docs.kaia.io/nodes/core-cell/monitoring-setup/

{{< line_break >}}
{{< line_break >}}
{{< line_break >}}

If you finish this step, please click the next button ```>``` on the right side of this page.