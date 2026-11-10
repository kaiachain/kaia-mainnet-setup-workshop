---
title: "Telegraf Installation"
date: 2022-07-11T15:42:53+09:00
weight: 20
pre: "<b>B. </b>"
draft: false
---

{{< line_break >}}

##### 2. Telegraf Installation
>Telegraf Installation Guide:

* https://docs.influxdata.com/telegraf/v1/install/

{{< line_break >}}

###### 1) Add the InfluxData yum repository
```vim
cat <<EOF | sudo tee /etc/yum.repos.d/influxdata.repo
[influxdata]
name = InfluxData Repository - Stable
baseurl = https://repos.influxdata.com/stable/\$basearch/main
enabled = 1
gpgcheck = 1
gpgkey = https://repos.influxdata.com/influxdata-archive.key
EOF
```

{{% notice warning %}}
Use ```influxdata-archive.key```, not ```influxdata-archive_compat.key```. The ```_compat``` key expired on 2026-01-18 and does not include the subkey (```DA61C26A0585BD3B```) that signs the current Telegraf packages, so ```yum install``` fails with a GPG check error. Do not work around it with ```--nogpgcheck```; fix the ```gpgkey``` instead. If an old ```/etc/yum.repos.d/influxdb.repo``` exists, remove it.
{{% /notice %}}

###### 2) Install the latest Telegraf
{{< highlight bash >}}
$ sudo yum install telegraf -y
$ telegraf --version
{{< /highlight >}}

{{< line_break >}}

If you finish this step, please click the next button ```>``` on the right side of this page.
