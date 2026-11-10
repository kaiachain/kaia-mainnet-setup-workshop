---
title: "Start the Kaia service"
date: 2022-07-11T18:37:31+09:00
weight: 10
pre: "<b>A. </b>"
draft: false
---

{{< line_break >}}
##### 1. Start the Kaia service.
###### 1) Option 1 (Yum Repo), or Option 2 (Archive) with ```kcnd.service``` registered
{{< highlight bash >}}
$ sudo systemctl status kcnd
$ sudo systemctl start kcnd
$ sudo systemctl status kcnd
{{< /highlight >}}

###### 2) Option 2 (Archive) without ```kcnd.service```
_** If you registered ```kcnd.service```, use 1) instead. Starting with the script directly while the service is enabled makes systemd lose track of the process._
{{< highlight bash >}}
$ sudo <DOWNLOADED_PATH>/kcn-linux-amd64/bin/kcnd status
$ sudo <DOWNLOADED_PATH>/kcn-linux-amd64/bin/kcnd start
$ sudo <DOWNLOADED_PATH>/kcn-linux-amd64/bin/kcnd status
{{< /highlight >}}

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
