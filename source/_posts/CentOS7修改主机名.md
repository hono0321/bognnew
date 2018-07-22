---
title: CentOS 7修改主机名
author: hono
date: 2016-12-04 
updated: 2016-12-04
tags: [Linux,配置]
---

CentOS 7修改主机名不在是直接修改hostname文件，而是使用如下命令

``` bash
$ hostnamectl set-hostname xxxx
```
其中xxxx为修改的主机名

修改完成后，使用如下命令查看是否修改成功

``` bash
$ hostnamectl status
```

应该会打印一些信息

修改主机名后不会马上显示出来，此时只需退出再登录即可，无需重启系统。

