---
title: Ubuntu环境下MediaWiki搭建及配置
author: hono
date: 2016-11-27 21:22:21
updated: 2017-1-15 21:56:54
tags: [mediawiki,环境搭建]
---

mediawiki官方有手动搭建文档，但比较繁琐且中文翻译质量一般，容易绕晕。官方提供可使用一个预集成的mediawiki软件包跳过手动搭建，过程还算简单，这里简单介绍下。

## 准备工作

### Linux环境准备

用的是ubuntu14.04 

### 安装文件下载

XAMPP [下载链接](https://www.apachefriends.org/zh_cn/index.html)

bitnami预集成mediawiki软件包 [下载链接](https://bitnami.com/stack/xampp#mediawiki)

**XAMPP**
XAMPP代表Apache、MYSQL、PHP,是一个PHP和Perl的集成开发环境，X表示支持Linux、Windows和MacOS

**Bitnami** 
Bitnami提供了各种在XAMPP上快速部署PHP应用的软件包，恰好提供了mediawiki这个应用，安装并经过简单配置后就可以使用了。

## 开始安装

### 安装XAMPP
将下载的安装文件拷贝至Ubuntu，追加可执行权限后直接运行，默认安装至/opt/lampp目录下即可。安装过程一路yes即可。

```bash
root@pad-vbox-ubuntu:~/tfcard# ./xampp-linux-x64-5.6.28-1-installer.run 
----------------------------------------------------------------------------
Welcome to the XAMPP Setup Wizard.

----------------------------------------------------------------------------
Select the components you want to install; clear the components you do not want 
to install. Click Next when you are ready to continue.

XAMPP Core Files : Y (Cannot be edited)

XAMPP Developer Files [Y/n] :y

Is the selection above correct? [Y/n]: y

----------------------------------------------------------------------------
Installation Directory

XAMPP will be installed to /opt/lampp
Press [Enter] to continue:

----------------------------------------------------------------------------
Setup is now ready to begin installing XAMPP on your computer.

Do you want to continue? [Y/n]: y

----------------------------------------------------------------------------
Please wait while Setup installs XAMPP on your computer.

 Installing
 0% ______________ 50% ______________ 100%
 #########################################

----------------------------------------------------------------------------
Setup has finished installing XAMPP on your computer.
```
安装完之后需要启动相关服务，就是apache、mysql和PHP。
```bash
root@pad-vbox-ubuntu:~/tfcard# /opt/lampp/lampp start
```
这里只要确保apache和mysql正常启动就可以了，启动之后可以访问主机IP看下主页是否正常显示，正常的话会显示xampp默认页面，表示安装成功。
之后继续安装mediawiki

### 安装mediawiki
给安装包可执行权限，直接运行安装，语言包选中文，后面根据情况配置即可，用户名是登录mediawiki用的，密码需要大于等于8位。
```bash
root@pad-vbox-ubuntu:~/tfcard# ./bitnami-mediawiki-1.28.0-0-module-linux-x64-installer.run 
Language Selection

Please select the installation language
[1] English - English
[2] Spanish - Español
[3] Simplified Chinese - 简体中文
Please choose an option [1] : 3
----------------------------------------------------------------------------
欢迎来到 Bitnami MediaWiki Module 安装程序。

----------------------------------------------------------------------------
安装文件夹

请选择Bitnami or XAMPP的安装目录

选择一个文件夹 [/opt/lampp]: 

Note: This module requires a pre-existing installation of Bitnami or a 
Bitnami-compatible stack like XAMPP. Please select the previous platform 
installation. For example: /opt/bitnami or /opt/lampp

----------------------------------------------------------------------------
创建管理员帐户

登录 [user]: admin

您的真实姓名 [User Name]: admin

Email地址 [user@example.com]: 

Enter the application password :
Retype password :
----------------------------------------------------------------------------
MediaWiki

请配置MediaWiki安装

Wiki名称 [admin's Wiki!]: hello world

----------------------------------------------------------------------------
安装程序已经准备好将 Bitnami MediaWiki Module 安装到您的电脑。

您确定要继续吗？ [Y/n]: y

----------------------------------------------------------------------------
正在安装 Bitnami MediaWiki Module 至您的电脑中，请稍候。

 正在安装
 0% ______________ 50% ______________ 100%
 #########################################

----------------------------------------------------------------------------
安装程序已经将 Bitnami MediaWiki Module 安装于您的电脑中。

调用 Bitnami MediaWiki Module [Y/n]: y
```
安装完成后在访问主机IP，在右上applications中可以看到mediawiki，点击access访问wiki首页。















