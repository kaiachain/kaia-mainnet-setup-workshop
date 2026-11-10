---
title: "Firewall Configuration"
date: 2022-07-11T19:18:45+09:00
weight: 10
pre: "<b>A. </b>"
draft: false
---
{{< line_break >}}

##### 1. CN Firewall configuration

###### 1) For communication and multichannel across CNs and Bootnodes in the Mainnet network, allow TCP ```32323-32324``` from any source to your firewall ```ingress``` rule.
```vim
0.0.0.0/0
```

###### 2) Additionally, allow UDP ``` 32323 ``` from any source to your firewall ```ingress``` rule.
```vim
0.0.0.0/0
```

###### 3) For communication, allow ```all traffic``` to your firewall ```egress``` rule.
```vim
IP version    |    Type           |    Protocol    |    Port range    |    Destination
---------------------------------------------------------------------------------------
IPv4          |    All traffic    |    All         |    All           |    0.0.0.0/0
```

_** The monitoring endpoints (```metrics.node.kaia.io:45560```, ```logs.node.kaia.io:45560```) only accept traffic from allowlisted IP addresses. Please share the public IP address of your CN with Kaia DevOps by Slack._

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
