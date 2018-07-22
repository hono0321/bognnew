---
title: mediawiki之word转换
author: hono
date: 2017-1-15 21:58:42
updated: 2017-1-21 13:29:01
tags: [mediawiki,使用指南]
---

## word2wiki

需求是将现有word文档迁移到wiki网站中。
word转wiki方法有几种但效果都一般，对样式特别复杂的word来说基本都不能保持一致，且工作量较大并不推荐，学习了几种方法记录以备忘。

### VisualEditor
VE是个编辑器，可以直接将word中的内容复制粘贴至wiki中，大部分格式都能保存完整，包括表格，但图片和高级的样式不支持。

### word2mediawikiPlus
这个组件现在已经不维护了，但可能还能正常工作。也是一个扩展组件。

### Microsoft office word add-in for mediawiki
Microsoft的一款插件，支持 office word 2007及以上版本。使用时将word文档保存为mediawiki类型的文件，是一个txt格式的文本。
然后将txt拷贝至wiki中。

#### 存在的问题：
* 不能处理图片。但在图片位置会生成占位符。
* 一些脚注之类的格式不能转换，包含这些格式转换时将报错。
* 部分文本将包含<nowiki>的标签。
* 最后，这个插件主要适用于不熟悉mediawiki标记语言的场景，比如初学者，适用于简单格式的word文本。

### LibreOffice
LibreOffice可以直接将word文档发布到mediawiki网站上，不过复杂格式和图片都不支持。
表格不支持，图片不支持 。

### word->html->mediawiki
* 将word文档另存为html文件
* 使用文本编辑器打开html，复制粘贴源码到[这个链接](https://tools.wmflabs.org/magnustools/html2wiki.php)中转换成wiki标记。
* 将处理好的wiki标记文本粘贴到wiki网站中。

特点：比较麻烦，效果不理想。

## 总结
这几种方法都先后试了下，但效果都非常不理想，主要是原word文档中文本样式复杂，如果转换处理的话，工作量可能比直接复制粘贴处理成wiki格式还要大，
并且当前较多的样式都基本不能恢复，而且目前重点也不在样式上，所以就学习了下wiki的标记语言，其实也不复杂。



## 参考链接
https://en.wikipedia.org/wiki/Help:WordToWiki






