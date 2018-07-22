---
title: sed命令学习
author: hono
date: 2017-1-21 21:58:16
updated: 2017-1-21 21:58:35
tags: [linux,shell]
---

今天处理文档中一个词的替换，数量不多本来要手动一个个替换的--!(被自己蠢哭了有木有sigh~)
经老司机指点可以用sed，马上学习了下，记录备忘。<!--more-->

sed是流编辑器。

上命令。。

```bash
sed -i 's/^update:/updated:/g' ./*
```
这条命令的意思是将当前目录中所有以"update:"开头的字符串替换成"updated:"
关键字：
s表示字符串
^应该是第一行的第一个字符串
g全部替换
./*是当前目录
-i表示直接修改文件，如果不加则在终端输出结果。

爬了下网看耗子叔有讲这个命令的教程，今天没空学了，留个链接吧。


## 参考链接
http://coolshell.cn/articles/9104.html/comment-page-1#comments
http://sed.sourceforge.net/sed1line_zh-CN.html
