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

###### 2) Start the log shipper (Fluent Bit) service.
_** Check that kcnd has created the log file first._
{{< highlight bash >}}
$ ls -l /kaia/kcnd/logs/kcnd.out
$ sudo systemctl enable --now fluent-bit
$ sudo systemctl status fluent-bit
{{< /highlight >}}

_** If the log path is wrong, Fluent Bit keeps running without any error and ships nothing, so ```active (running)``` alone is not enough. Check that the log file is being watched:_
{{< highlight bash >}}
$ sudo journalctl -u fluent-bit -n 50 | grep inotify_fs_add
... [input:tail:tail.0] inotify_fs_add(): inode=... watch_fd=1 name=/kaia/kcnd/logs/kcnd.out
{{< /highlight >}}
_** If there is no such line, check the ```Path``` in ```/etc/fluent-bit/fluent-bit.conf```._

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
