---
title: "Data Disk and Directories"
date: 2026-11-10T10:00:00+09:00
weight: 30
pre: "<b>C. </b>"
draft: false
---

{{< line_break >}}

##### 3. Mount the data disk on ```/kaia``` and create the kcnd directories.
_** The size of Kaia blockchain data always increases. Please attach a data disk big enough to store the Mainnet chaindata. You can refer to the chaindata size via **[Mainnet snapshot archive](https://snapshots.node.kaia.io/#mainnet-pruning)**._

###### 1) Find the data disk.
{{< highlight bash >}}
$ lsblk
{{< /highlight >}}

###### 2) Format the data disk and mount it on ```/kaia```.
{{% notice warning %}}
```mkfs``` erases everything on the disk. Please make sure ```<data_disk>``` is the new, empty data disk.
{{% /notice %}}
{{< highlight bash >}}
$ sudo mkfs.xfs /dev/<data_disk>
$ sudo mkdir -p /kaia
$ sudo mount /dev/<data_disk> /kaia
$ df -h /kaia
{{< /highlight >}}

###### 3) Add the data disk to ```/etc/fstab``` so that it is mounted again after reboot.
{{< highlight bash >}}
$ echo "UUID=$(sudo blkid -s UUID -o value /dev/<data_disk>) /kaia xfs defaults,nofail 0 2" | sudo tee -a /etc/fstab
$ sudo umount /kaia && sudo mount -a && df -h /kaia
{{< /highlight >}}

###### 4) Create the ```kcnd``` directory, and the ```data``` and ```logs``` directories under it.
{{< highlight bash >}}
$ sudo mkdir -p /kaia/kcnd/data
$ sudo mkdir -p /kaia/kcnd/logs
$ ls -l /kaia/kcnd
{{< /highlight >}}

| Path | Usage |
|---|---|
| ```/kaia/kcnd/data``` | ```DATA_DIR``` - chaindata, nodekey |
| ```/kaia/kcnd/logs``` | ```LOG_DIR``` - ```kcnd.out``` |

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
