---
title: "Extract the chaindata"
date: 2022-07-11T15:43:06+09:00
weight: 20
pre: "<b>B. </b>"
draft: false
---

{{< line_break >}}
##### 2. Extract the downloaded chaindata to the DATA_DIR (```/kaia/kcnd/data```).

{{< highlight bash >}}
$ sudo tar --zstd -C /kaia/kcnd/data -xvf kaia-mainnet-pruning-chaindata-20260918010012.tar.zst --exclude klay/chaindata/receipts
{{< /highlight >}}

_** Snapshots are compressed with [zstd](https://github.com/facebook/zstd), so `tar` needs `--zstd`. If your `tar` was built without it, run `sudo yum install zstd` and extract in two steps: `zstd -d <file>.tar.zst -o out.tar && sudo tar -C /kaia/kcnd/data -xf out.tar --exclude klay/chaindata/receipts`._

_** If the disk has no room for both the archive and the directory it expands into, download and extract in one pass instead of doing step A first._
{{< highlight bash >}}
$ URL=`curl -s https://snapshots.node.kaia.io/mainnet/pruning-chaindata/latest.txt`
$ curl -s $URL | sudo tar --zstd -C /kaia/kcnd/data -xf - --exclude klay/chaindata/receipts
{{< /highlight >}}

{{% notice note %}}
Keep the downloaded archive (```kaia-mainnet-pruning-chaindata-*.tar.zst```) until the node setup is complete. After your CN is running normally at the end of this workshop, remove it to free up the disk space: ```rm kaia-mainnet-pruning-chaindata-20260918010012.tar.zst``` (and ```out.tar``` if you used the two-step extraction).
{{% /notice %}}

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
