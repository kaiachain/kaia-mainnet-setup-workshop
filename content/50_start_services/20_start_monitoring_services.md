---
title: "Start the monitoring services"
date: 2022-07-11T18:39:03+09:00
weight: 20
pre: "<b>B. </b>"
draft: false
---

{{< line_break >}}
##### 2. Start monitoring services.
###### 1) Start the Telegraf service.
{{< highlight bash >}}
$ sudo systemctl status telegraf
$ sudo systemctl enable --now telegraf
$ sudo systemctl status telegraf
{{< /highlight >}}

###### 2) Start the log shipper service.
_** Check that kcnd has created the log file first._
{{< highlight bash >}}
$ ls -l /kaia/kcnd/logs/kcnd.out
$ sudo systemctl enable --now kaia-log-shipper
$ sudo systemctl status kaia-log-shipper
{{< /highlight >}}

_** If the service fails to start, check the preflight message with ```sudo journalctl -u kaia-log-shipper -n 50```. A ```preflight: ok``` line means the log file and the buffer directory are ready._

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
