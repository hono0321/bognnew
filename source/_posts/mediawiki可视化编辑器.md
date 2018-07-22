---
title: MediaWiki可视化编辑器配置
author: hono
date: 2017-1-21 19:15:17 
updated: 2017-1-21 19:15:22
tags: [VisualEditor,MediaWiki,Parsoid,nodejs]
---

VE这个组件点比较多，不像其他组件配起来还是有点复杂的，这里记录下配置过程以备忘。

* 系统版本：ubuntu16.04
* mediawiki版本：1.28

## 下载VE组件安装包
[下载链接](https://www.mediawiki.org/wiki/Special:ExtensionDistributor/VisualEditor)
下载完成后将tar.gz压缩包解压至mediawiki的extensions/目录下<!--more-->

```bash
tar xzvf VisualEditor-REL1_28-93528b7.tar.gz -C /opt/lampp/apps/mediawiki/htdocs/extensions
```

## 配置VisualEditor
### 配置Parsoid服务
如果不配置Parsoid Node.js服务，将不能使用VisualEditor编辑和保存已创建的页面。也就是说只能新建一个页面编辑，但不能保存也不能编辑已有页面。

Parsoid服务貌似是用来转换HTML和wiki文本的，以便用VE编辑显示。

### 安装Parsoid
配置VE前需要先安装Parsoid服务
导入gpg key

```bash
root@pad-vbox-ubuntu:~# apt-key advanced --keyserver pgp.mit.edu --recv-keys 90E9F83F22250DD7
Executing: /tmp/tmp.qTyLJ4Gtgg/gpg.1.sh --keyserver
pgp.mit.edu
--recv-keys
90E9F83F22250DD7
gpg: requesting key 22250DD7 from hkp server pgp.mit.edu
gpg: key 22250DD7: public key "MediaWiki releases repository <wikitech-l@lists.wikimedia.org>" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
```

增加wikimedia库(不造啥意思，按教程来的)

```bash
root@pad-vbox-ubuntu:~# apt-add-repository "deb https://releases.wikimedia.org/debian jessie-mediawiki main"
```

安装Parsoid

```bash
root@pad-vbox-ubuntu:~# apt-get install apt-transport-httpsReading package lists... Done
Building dependency tree       
Reading state information... Done
apt-transport-https is already the newest version (1.2.19).
0 upgraded, 0 newly installed, 0 to remove and 4 not upgraded.
root@pad-vbox-ubuntu:~# apt-get update && sudo apt-get install parsoid
```
更新安装打印信息太多了不贴了

修改parsoid配置文件/etc/mediawiki/parsoid/config.yaml
将uri: 'http://localhost/mediawiki/api.php'修改为指向wiki的api。

```bash
        # Configure Parsoid to point to your MediaWiki instances.
        mwApis:
        - # This is the only required parameter,
          # the URL of you MediaWiki API endpoint.
          uri: 'http://localhost/mediawiki/api.php'
          # The "domain" is used for communication with Visual Editor
          # and RESTBase.  It defaults to the hostname portion of
          # the `uri` property below, but you can manually set it
          # to an arbitrary string.
          domain: 'localhost'  # optional
          # To specify a proxy (or proxy headers) specific to this prefix
          # (which overrides defaultAPIProxyURI). Alternatively, set `proxy`
          # to `null` to override and force no proxying when a default proxy
...
```
修改配置文件/etc/meddiawiki/parsoid/settings.js,加入以下配置。这个文件如果不存在请手动在该路径下创建并添加以下内容。

```bash
parsoidConfig.setMwApi({ uri: 'https://localhost/mediawiki/api.php', prefix: 'localhost', domain: 'localhost' });
```

以上配置完成之后重启parsoid服务
```bash
root@pad-vbox-ubuntu:/etc/mediawiki/parsoid# service parsoid restart 
root@pad-vbox-ubuntu:/etc/mediawiki/parsoid# 
```

## 配置VE 
这个就比较简单了，将以下内容追加至配置文件LocalSettings.php的末尾。

* 配置启用VE

```bash
wfLoadExtension( 'VisualEditor' );

// Enable by default for everybody
$wgDefaultUserOptions['visualeditor-enable'] = 1;

// Optional: Set VisualEditor as the default for anonymous users
// otherwise they will have to switch to VE
// $wgDefaultUserOptions['visualeditor-editor'] = "visualeditor";

// Don't allow users to disable it
$wgHiddenPrefs[] = 'visualeditor-enable';

// OPTIONAL: Enable VisualEditor's experimental code features
#$wgDefaultUserOptions['visualeditor-enable-experimental'] = 1;

```

* 配置VE连接parsoid

```bash
$wgVirtualRestConfig['modules']['parsoid'] = array(
        // URL to the Parsoid instance
        // Use port 8142 if you use the Debian package
        'url' => 'http://localhost:8142',
        // Parsoid "domain", see below (optional)
        'domain' => 'localhost',
        // Parsoid "prefix", see below (optional)
        'prefix' => 'localhost'
);
```
ok了，应该可以正常使用了。

## 参考链接
https://www.mediawiki.org/wiki/Extension:VisualEditor
https://www.mediawiki.org/wiki/Parsoid/Setup
https://www.mediawiki.org/wiki/Help:VisualEditor/User_guide/zh