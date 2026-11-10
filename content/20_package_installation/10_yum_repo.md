---
title: "Option 1 - Kaia Yum Repo"
date: 2022-07-11T19:18:57+09:00
weight: 10
pre: "<b>A. </b>"
draft: false
---

{{< line_break >}}

##### 1. Install from Kaia Yum Repo (RHEL recommended)
_** Please refer to the document page for more details: <https://docs.kaia.io/nodes/core-cell/install/install-consensus-nodes/>_

{{< highlight bash >}}
$ sudo curl -o /etc/yum.repos.d/kaia.repo https://packages.kaia.io/config/rhel/9-stream/kaia.repo
$ sudo yum install kcnd -y
$ kcn version
{{< /highlight >}}

The installed files are located as follows.

| File Name | Location |
|---|---|
| kcn | ```/usr/bin/kcn``` |
| kcnd.conf | ```/etc/kcnd/conf/kcnd.conf``` |

_** If you install with this option, skip [2-B. Option 2 - Linux Archive](/20_package_installation/20_linux_archive/)._

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
