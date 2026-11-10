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

Download the latest version of kcn from the **[Download page](https://docs.kaia.io/nodes/downloads/)**, and uncompress it where you want to install the package. The commands below look up the latest release version from the [Kaia GitHub releases](https://github.com/kaiachain/kaia/releases) and download it.

_** In this workshop, ```<DOWNLOADED_PATH>``` means the directory where you download and uncompress the archive. The ```kcn-linux-amd64``` directory is created under it._
{{< highlight bash >}}
$ cd <DOWNLOADED_PATH>
$ VERSION=$(curl -s https://api.github.com/repos/kaiachain/kaia/releases/latest | grep '"tag_name"' | cut -d'"' -f4)
$ echo $VERSION
$ curl -fLO https://packages.kaia.io/kaia/${VERSION}/kcn-${VERSION}-0-linux-amd64.tar.gz
$ tar zxf kcn-${VERSION}-0-linux-amd64.tar.gz
$ export PATH=$PATH:<DOWNLOADED_PATH>/kcn-linux-amd64/bin
$ kcn version
{{< /highlight >}}

_** It is recommended to add ```<DOWNLOADED_PATH>/kcn-linux-amd64/bin``` to the environment variable ```$PATH``` (e.g. in ```~/.bashrc```) to run ```kcn``` and ```kcnd``` globally. The other pages assume that the path is added to the variable._

| File Name | Location |
|---|---|
| kcn | ```<DOWNLOADED_PATH>/kcn-linux-amd64/bin/kcn``` |
| kcnd.conf | ```<DOWNLOADED_PATH>/kcn-linux-amd64/conf/kcnd.conf``` |

###### (Optional) Register kcnd as a systemd service
With the archive distribution, kcnd is not registered as a service, so it does not start automatically after reboot. If you want to manage kcnd with ```systemctl``` like Option 1, create ```/etc/systemd/system/kcnd.service``` as follows.

```vim
[Unit]
Description=kcnd Service

[Service]
Type=forking
ExecStart=<DOWNLOADED_PATH>/kcn-linux-amd64/bin/kcnd start
ExecStop=<DOWNLOADED_PATH>/kcn-linux-amd64/bin/kcnd stop
# kcnd writes the kcn process ID to $DATA_DIR/kcnd.pid
PIDFile=/kaia/kcnd/data/kcnd.pid

[Install]
WantedBy=multi-user.target
```

_** Replace ```<DOWNLOADED_PATH>``` with your actual path. ```PIDFile``` must match ```DATA_DIR``` in ```kcnd.conf``` (see [3-A. Node Configuration](/40_configure_network/10_node_configuration/))._

Enable the service.
{{< highlight bash >}}
$ sudo systemctl daemon-reload
$ sudo systemctl enable kcnd.service
{{< /highlight >}}

_** Do not start kcnd yet. It will be started in [4-A. Start the Kaia service](/50_start_services/10_start_klaytn_service/) after the chaindata and ```kcnd.conf``` are ready._

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
