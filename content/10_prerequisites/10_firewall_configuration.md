---
title: "Firewall Configuration"
date: 2022-07-11T19:18:45+09:00
weight: 10
pre: "<b>A. </b>"
draft: false
---
{{< line_break >}}

##### 1. *(Only for CN)* Firewall configuration

###### 1) If Kairos validation is completed, remove below Kaia CN's IP addresses of Kairos from your firewall configuration.
```34.64.63.34```   
```34.64.209.150```   
```34.64.229.190```   
```34.64.63.183```   
```147.92.173.41```
```34.64.155.25```   
{{< line_break >}}
{{< line_break >}}

###### 2) For communication and multichannel across CCO CNs in the Mainnet network, TCP ```32323-32324``` and UDP ``` 32323 ``` from existing CCO's CNs should be allowed in your CN firewall configuration.
_** The existing CCO's CNs IP addresses will be provided by Slack DM._

{{< line_break >}}
###### 3) For communication and multichannel across Bootnode in the Mainnet network, UDP ``` 32323 ``` from existing CCO's CNs should be allowed in your CN firewall configuration.
_** The existing Bootnode IP addresses will be provided by Slack DM._

{{< line_break >}}

### 2. *(Only for PN)* Firewall configuration

###### 1) For communication and multichannel between Kaia PN, allow TCP ```32323-32324``` with below IP addresses to your firewall ```ingress``` rule.
```vim
0.0.0.0/0
```

###### 2) Additionally, it is required to allow UDP ``` 32323 ``` with the same IP addresses to your firewall ```ingress``` rule.
```vim
0.0.0.0/0
``` 

###### 3) For communication, allow ```all traffic``` to your firewall ```egress``` rule.
```vim
IP version    |    Type           |    Protocol    |    Port range    |    Destination
---------------------------------------------------------------------------------------
IPv4          |    All traffic    |    All         |    All           |    0.0.0.0/0
``` 

{{< line_break >}}
{{< line_break >}}

---
{{< line_break >}}

*Please note that the above four IP addresses are attached to the Kaia CNs.*

{{< line_break >}}
{{< line_break >}}
{{< line_break >}}

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
