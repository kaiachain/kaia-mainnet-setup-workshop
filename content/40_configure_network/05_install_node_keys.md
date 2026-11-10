---
title: "Install Node Keys"
date: 2026-11-10T10:00:00+09:00
weight: 5
pre: "<b>A. </b>"
draft: false
---

{{< line_break >}}
##### 1. Install the nodekey and bls-nodekey into the data directory.
The nodekey is the private key of your node, and the bls-nodekey is the BLS secret key used for consensus. Put the ```nodekey``` and ```bls-nodekey``` files you generated with keytool into the data directory (```DATA_DIR=/kaia/kcnd/data```).

{{% notice warning %}}
Install both keys **before starting kcnd**. If kcn cannot find a nodekey in the data directory, it does not stop: it generates a new nodekey (and derives a new bls-nodekey from it) and runs with that identity instead of yours.
{{% /notice %}}

###### 1) Install the nodekey
{{< highlight bash >}}
$ sudo cp nodekey /kaia/kcnd/data/
{{< /highlight >}}

###### 2) Install the bls-nodekey
{{< highlight bash >}}
$ sudo cp bls-nodekey /kaia/kcnd/data/
{{< /highlight >}}

###### 3) Restrict the permissions and check the files
{{< highlight bash >}}
$ sudo chmod 600 /kaia/kcnd/data/nodekey /kaia/kcnd/data/bls-nodekey
$ sudo ls -l /kaia/kcnd/data/nodekey /kaia/kcnd/data/bls-nodekey
{{< /highlight >}}

_** These files are private keys. Keep a backup in a safe place and never share them._

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
