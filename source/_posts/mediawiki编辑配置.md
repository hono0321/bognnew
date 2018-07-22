---
title: MediaWiki编辑配置
author: hono
date: 2017-1-11 20:56:35
updated: 2017-1-11 20:56:44
tags: [mediawiki,配置,编辑]
---
mediawiki刚搭建好后默认WikiEditor扩展插件默认不启用。编辑功能比较简单。
这里配置一下这个功能。<!--more-->

* 首先确认下wiki安装目录的extentions路径下是否有WikiEditor这个文件夹。
```bash
root@hi:/opt/lampp/apps/mediawiki/htdocs/extensions# ls
Cite          Gadgets   Interwiki           ParserFunctions  README         SyntaxHighlight_GeSHi
CiteThisPage  ImageMap  LocalisationUpdate  PdfHandler       Renameuser     TitleBlacklist
ConfirmEdit   InputBox  Nuke                Poem             SpamBlacklist  WikiEditor
root@hi:/opt/lampp/apps/mediawiki/htdocs/extensions# cd WikiEditor/
root@hi:/opt/lampp/apps/mediawiki/htdocs/extensions/WikiEditor# ls
composer.json  extension.json  i18n     phpcs.xml  tests                 WikiEditor.php
COPYING        Gruntfile.js    modules  README     WikiEditor.hooks.php
root@hi:/opt/lampp/apps/mediawiki/htdocs/extensions/WikiEditor#
```
比较新的版本应该都是自带WikiEditor的，如果安装目录下没有需下载。
[下载链接](https://www.mediawiki.org/wiki/Special:ExtensionDistributor/WikiEditor)
文件下载后放置extensions/文件夹中的WikiEditor目录内。

* 确认WikiEditor目录后，将以下配置写入LocalSettings.php文件。
```bash
wfLoadExtension( 'WikiEditor' );

# Enables use of WikiEditor by default but still allows users to disable it in preferences
$wgDefaultUserOptions['usebetatoolbar'] = 1;

# Enables link and table wizards by default but still allows users to disable them in preferences
$wgDefaultUserOptions['usebetatoolbar-cgd'] = 1;

# Displays the Preview and Changes tabs
$wgDefaultUserOptions['wikieditor-preview'] = 1;

# Displays the Publish and Cancel buttons on the top right side
$wgDefaultUserOptions['wikieditor-publish'] = 1;
```
至此，配置完成。

配置完成后可能出现图片显示出错的情况，提示如下错误信息。
```bash
生成缩略图出错：/bin/bash: /bin/convert: No such file or directory Error code:127
```
首先确认是否配置如下配置
```bash
$wgUseImageMagick = true;  
$wgImageMagickConvertCommand = "/usr/bin/convert"; 
```
确认系统中是否有convert程序，主要是/usr/bin/目录/bin目录下是否有这个文件，如果没有就执行命令安装：
```bash
apt-get install ImageMagic
```
安装完成后，确认convert程序路径是否为配置路径，然后刷新页面即可。

[参考链接](https://www.mediawiki.org/wiki/Extension:WikiEditor/zh)
[参考链接](http://blog.csdn.net/loveaborn/article/details/44080043)

## 上传目录
如果未启用LocalSettings.php文件中的$wgHashedUploadDirectory这个参数，那么上传的图片将被分发到$wgUploadDirectory目录的子目录中，并基于文件的MD5
值的前两个字符作为目录名来存储，例如$IP/images/a/ab/foo.jpg这种形式。这些子目录都是自动创建的，采用这种方式可以避免过多的图片存储在一个目录下，从而
影响性能。如果wiki环境比较简单，存储的图片较少，则可以直接将$wgHashedUploadDirectory这个参数值设置为false，这样图片将全部会存储在$wgUploadDirectory
指定的目录下，默认情况下这个目录为images/目录。
但需要注意的是，如果从后台将图片上传到images/目录下并在前台使用名称引用，这样是不能正常显示图片的，应该是因为mediawiki是通过MD5值来唯一标识图片。




