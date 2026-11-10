---
title: "Change log level"
date: 2024-08-13T10:43:05+09:00
weight: 50
pre: "<b>E. </b>"
draft: false
---
{{< line_break >}}
##### 5. Change log level
{{< highlight bash >}}
$ sudo kcn attach --exec 'debug.verbosity(1)' /kaia/kcnd/data/klay.ipc
$ sudo kcn attach --exec 'debug.vmodule("blockchain=3")' /kaia/kcnd/data/klay.ipc
{{< /highlight >}}
{{< line_break >}}
If you finish this step, please click the next button ```>``` on the right side of this page.