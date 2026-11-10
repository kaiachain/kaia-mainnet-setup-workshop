---
title: "Start the Kaia service"
date: 2022-07-11T18:37:31+09:00
weight: 10
pre: "<b>A. </b>"
draft: false
---

{{< line_break >}}
##### 1. Start the Kaia service.
###### 1) Option 1 (Yum Repo)
{{< highlight bash >}}
$ sudo systemctl status kcnd
$ sudo systemctl start kcnd
$ sudo systemctl status kcnd
{{< /highlight >}}

###### 2) Option 2 (Archive)
{{< highlight bash >}}
$ sudo /opt/kcn-linux-amd64/bin/kcnd status
$ sudo /opt/kcn-linux-amd64/bin/kcnd start
$ sudo /opt/kcn-linux-amd64/bin/kcnd status
{{< /highlight >}}

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
