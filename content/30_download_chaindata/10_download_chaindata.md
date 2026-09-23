---
title: "Download the latest Mainnet chaindata"
date: 2022-07-11T15:42:53+09:00
weight: 10
pre: "<b>A. </b>"
draft: false
---

{{< line_break >}}
##### 1. Download the latest chaindata snapshot from the Mainnet snapshot archive.
###### 0) Before proceeding, please check if your disk space is enough to store and extract the Mainnet chaindata.
_** You can refer to the chaindata size via **[Mainnet snapshot archive](https://snapshots.node.kaia.io/#mainnet-pruning)**, which lists every published snapshot with its compressed size, its extracted size and its checksum._   
{{< line_break >}}

###### 1) For CN
_** Please note that this step will take a lot of time to download: the snapshot is over 2.2 TB compressed and expands to about 2.8 TB, so we recommend using a temporary disk for downloading. If you want to reduce the time, please refer the next step._   
_** The latest chaindata name can be different with this example due to the date information._
{{< highlight html >}}
$ URL=`curl -s https://snapshots.node.kaia.io/mainnet/pruning-chaindata/latest.txt`
$ echo $URL
https://kaia-chaindata-r2-logging.kaia-foundation.workers.dev/mainnet/pruning/kaia-mainnet-pruning-chaindata-20260918010012.tar.zst
$ wget $URL
{{< /highlight >}}
###### 2) For PN
{{< highlight html >}}
$ URL=`curl -s https://snapshots.node.kaia.io/mainnet/pruning-chaindata/latest.txt`
$ echo $URL
https://kaia-chaindata-r2-logging.kaia-foundation.workers.dev/mainnet/pruning/kaia-mainnet-pruning-chaindata-20260918010012.tar.zst
$ wget $URL
{{< /highlight >}}

###### 2) Optional - If you want to save the time for downloading, you can consider using ```axel``` command.   
_**[Axel](https://github.com/axel-download-accelerator/axel) tries to accelerate the download process by using multiple connections per file._
###### 1) For CN
{{< highlight html >}}
(Amazon Linux 2) $ sudo amazon-linux-extras install epel
(CentOS) $ sudo yum install epel-release -y
$ sudo yum install axel -y
$ URL=`curl -s https://snapshots.node.kaia.io/mainnet/pruning-chaindata/latest.txt`
$ echo $URL
https://kaia-chaindata-r2-logging.kaia-foundation.workers.dev/mainnet/pruning/kaia-mainnet-pruning-chaindata-20260918010012.tar.zst
$ axel -n8 $URL
{{< /highlight >}}
###### 2) For PN
{{< highlight html >}}
(Amazon Linux 2) $ sudo amazon-linux-extras install epel
(CentOS) $ sudo yum install epel-release -y
$ sudo yum install axel -y
$ URL=`curl -s https://snapshots.node.kaia.io/mainnet/pruning-chaindata/latest.txt`
$ echo $URL
https://kaia-chaindata-r2-logging.kaia-foundation.workers.dev/mainnet/pruning/kaia-mainnet-pruning-chaindata-20260918010012.tar.zst
$ axel -n8 $URL
{{< /highlight >}}

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
