---
title: "Package Installation"
date: 2022-07-11T19:18:57+09:00
weight: 20
pre: "<b>B. </b>"
draft: false
---

{{< line_break >}}

##### 2. Installation of Kaia package for CN
_** Please refer to the document page for more details: <https://docs.kaia.io/nodes/core-cell/install/install-consensus-nodes/>_

{{< line_break >}}

###### Option 1) Install from Kaia Yum Repo (RHEL recommended)
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

{{< line_break >}}

###### Option 2) Linux Archive Distribution
The archive file consists of the executable binary and the configuration file structured as follows.

```vim
kcn-linux-amd64
├── bin
│   ├── kcn      # CN executable file
│   └── kcnd     # CN start/termination script file
└── conf
    └── kcnd.conf  # CN configuration file
```

{{% notice warning %}}
Do NOT alter the file structure or file name. If you change it, the node may not function correctly.
{{% /notice %}}

Download the latest version of kcn from the **[Download page](https://docs.kaia.io/nodes/downloads/)**, and uncompress it where you want to install the package. The example below installs ```v2.2.2``` under ```/opt```.
{{< highlight bash >}}
$ VERSION=v2.2.2
$ curl -fLO https://packages.kaia.io/kaia/${VERSION}/kcn-${VERSION}-0-linux-amd64.tar.gz
$ sudo tar zxf kcn-${VERSION}-0-linux-amd64.tar.gz -C /opt
$ export PATH=$PATH:/opt/kcn-linux-amd64/bin
$ kcn version
{{< /highlight >}}

_** It is recommended to add ```/opt/kcn-linux-amd64/bin``` to the environment variable ```$PATH``` (e.g. in ```~/.bashrc```) to run ```kcn``` and ```kcnd``` globally. The other pages assume that the path is added to the variable._

| File Name | Location |
|---|---|
| kcn | ```/opt/kcn-linux-amd64/bin/kcn``` |
| kcnd.conf | ```/opt/kcn-linux-amd64/conf/kcnd.conf``` |

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
