---
title: "Remove chaindata and log"
date: 2022-07-11T15:17:04+09:00
weight: 30
pre: "<b>C. </b>"
draft: false
---

{{< line_break >}}
#### 3. Remove existing chaindata and log for pre-cypress.

##### 0) During pre-cypress, you did set <klaytn_home_path> for Ansible deployment. But you can check this out if you can't remember where your data directory is located.
{{< highlight html >}}
$ grep "DATA_DIR" /etc/kpnd/conf/kpnd.conf
{{< /highlight >}}

##### 1) For CN,
{{< highlight html >}}
$ sudo rm -Rf <your_klaytn_home_path>/kcnd/data/klay/chaindata
$ sudo rm -Rf <your_klaytn_home_path>/kcnd/data/klay/lightchaindata
$ sudo rm -Rf <your_klaytn_home_path>/kcnd/log/*
{{< /highlight >}}

##### 2) For PN,
{{< highlight html >}}
$ sudo rm -Rf <your_klaytn_home_path>/kpnd/data/klay/chaindata
$ sudo rm -Rf <your_klaytn_home_path>/kpnd/data/klay/lightchaindata
$ sudo rm -Rf <your_klaytn_home_path>/kpnd/log/*
{{< /highlight >}}

{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.
