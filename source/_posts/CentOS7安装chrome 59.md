---
title: CentOS 7安装Chrome 59
author: hono
date: 2017-6-25 10:34:37
updated: 2017-6-25 10:34:46
tags: [Linux,browser,Chrome]
---

**2016年3月后Chrome不在支持32位的Linux发行版**

## 启用Google YUM源
创建文件/etc/yum.repos.d/google-chrome.repo并将以下内容拷贝至该文件

``` bash
[google-chrome]
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub

```
## 安装Chrome最新稳定版
``` bash
$ yum info google-chrome-stable
$ yum install google-chrome-stable

```
OK,Done!
