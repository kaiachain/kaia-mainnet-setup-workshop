---
title: "Node Configuration"
date: 2022-07-11T17:55:05+09:00
weight: 20
pre: "<b>B. </b>"
draft: false
---

{{< line_break >}}
##### 1. Modify network, discovery and boot node information.
_** Note that CN and PN have some different information._

###### 1) For CN, your configuration should be modified as shown below.
{{< highlight html >}}
$ egrep "^NETWORK|NO_DISCOVER|BOOTNODES|ADDITIONAL" /etc/kcnd/conf/kcnd.conf
NETWORK="cypress"
NETWORK_ID=
NO_DISCOVER=0 # setting 1 to disable discovery
BOOTNODES="kni://b286e4140aea469992146c299f8915e34d59a014c29a045de41e578014761114176f42dece34a54ce1e37f92ab981fe1bb14e54fe23576e1182778c40e6272a2@ston146.node.kaia.io:32323?ntype=bn,kni://a6b61ee786952e3f8b681867d2535485622e80d0b9b7b89f26b2c631e59a4246b5f879487fbde7c324c3308ece0cdb1d7738430bdffce4f7f8c4f5a09eef80a3@ston738.node.kaia.io:32323?ntype=bn,kni://b25838727eb6b4631c8f8910b4f6376fe28041f251ee21129078a61d18d62d0dc2601be5a97eab8bdb5772309f861fddb7192935483813ef20e5716a34266f16@ston903.node.kaia.io:32323?ntype=bn"
ADDITIONAL="--state.trie-cache-limit 5000 --state.live-pruning"
{{< /highlight >}}

###### 2) For PN
{{< highlight html >}}
$ egrep "^NETWORK|NO_DISCOVER|BOOTNODES|ADDITIONAL" /etc/kpnd/conf/kpnd.conf
NETWORK="cypress"
NETWORK_ID=
NO_DISCOVER=1 # setting 1 to disable discovery
BOOTNODES=""
ADDITIONAL="--state.trie-cache-limit 5000 --state.live-pruning"
{{< /highlight >}}
{{< line_break >}}

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
