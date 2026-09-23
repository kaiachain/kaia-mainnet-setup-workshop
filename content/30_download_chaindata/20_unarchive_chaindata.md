---
title: "Extract the chaindata"
date: 2022-07-11T15:43:06+09:00
weight: 20
pre: "<b>B. </b>"
draft: false
---

{{< line_break >}}
##### 2. Extract the chaindata downloaded to the DATA_DIR.

###### 1) For CN,
{{< highlight html >}}
$ tar --zstd -C <your_kaia_home_path>/kcnd/data -xvf kaia-mainnet-pruning-chaindata-20260918010012.tar.zst --exclude klay/chaindata/receipts
{{< /highlight >}}

###### 2) For PN,
{{< highlight html >}}
$ tar --zstd -C <your_kaia_home_path>/kpnd/data -xvf kaia-mainnet-pruning-chaindata-20260918010012.tar.zst --exclude klay/chaindata/receipts
{{< /highlight >}}

_** Snapshots are compressed with [zstd](https://github.com/facebook/zstd), so `tar` needs `--zstd`. If your `tar` was built without it, run `sudo yum install zstd` and extract in two steps: `zstd -d <file>.tar.zst -o out.tar && tar -C <your_kaia_home_path>/k*nd/data -xf out.tar --exclude klay/chaindata/receipts`._

_** If the disk has no room for both the archive and the directory it expands into, download and extract in one pass instead of doing step A first._
{{< highlight html >}}
$ URL=`curl -s https://snapshots.node.kaia.io/mainnet/pruning-chaindata/latest.txt`
$ curl -s $URL | tar --zstd -C <your_kaia_home_path>/k*nd/data -xf - --exclude klay/chaindata/receipts
{{< /highlight >}}

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
