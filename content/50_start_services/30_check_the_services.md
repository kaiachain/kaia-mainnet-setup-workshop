---
title: "Check the services"
date: 2022-07-11T18:42:35+09:00
weight: 30
pre: "<b>C. </b>"
draft: false
---

{{< line_break >}}
##### 3. Watch the service log to confirm if your node is correctly joined.

_** If the node is not a proposer at that block, and the consensus is successful, the node have executed(==validates) the block. In other words, a block is inserted._
_** You can refer [this document](https://docs.kaia.io/nodes/debugging/node-log/#info-logs) for more details about node log._

{{< highlight bash >}}
$ tail /kaia/kcnd/logs/kcnd.out
INFO Inserted a new block number=14 hash=13cbfc…f007fc txs=0 gas=0 elapsed=793.458µs processTxs=167ns finalize=157.708µs validateState=7.542µs totalWrite=443.417µs trieWrite=256.667µs
{{< /highlight >}}

###### Please note that the above message will be found when Kaia operators add your node as a peer. So you may face the below messages instead. You can ignore error messages as below. It will disappear after some follow-up tasks from Kaia operators.
{{< highlight bash >}}
err="Genesis block mismatch"
err="unauthorized address"
{{< /highlight >}}


{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
