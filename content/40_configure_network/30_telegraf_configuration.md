---
title: "Telegraf Configuration"
date: 2022-07-14T16:44:50+09:00
weight: 50
pre: "<b>E. </b>"
draft: false
---

{{< line_break >}}
##### 1. To monitor your nodes in Mainnet Dashboard, please change the influxdb configuration as below:

###### 1) For both CN and PN,
{{< highlight html >}}
$ grep -A2 "influxdb" /etc/telegraf/telegraf.d/kaia.conf
[[outputs.influxdb]]
urls = [ "http://node.kaia.io:45560" ]
database = "klaytn_cypress"
{{< /highlight >}}

###### 2) Only for CN,
1. Change the interval time of Telegraf agent from 10s to 1s.   
{{< highlight html >}}
$ cat /etc/telegraf/telegraf.conf | grep -v '^\s*$\|^\s*\#' | grep interval
interval = "1s"
round_interval = true
flush_interval = "1s"
{{< /highlight >}}

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
