---
title: Shell编程学习笔记（一）
author: hono
date: 2016-12-25 11:40:21
updated: 2016-12-26 21:35:12
tags: [linux,shell,编程]
---

Shell编程学习笔记和备忘，通过学习和记录了解Linux Shell和Shell编程相关知识。

使用文本编辑器编写shell脚本时，需要在第一行添加如下行：
```bash
#!/bin/bash
```

这行称为shebang(#she ！bang)，指引系统使用符号后面所跟的shell程序运行文件。
一般情况下也可写作如下形式：
```bash
#!/usr/bin/evn python
```
这种写法以为使用环境变量中的pyhton程序来执行脚本文件，是一种更广泛的写法。<!--more-->

shebang也可以引用任何系统中的程序来执行文件
```bash
#!/bin/cat
Hello World!
```

编辑完并保存后，需要修改文件权限为可执行：
```bash
chmod u+x filename
```
## 变量
shell变量默认都是字符串。
变量的引用只需在变量名前加一个$
变量名和等号之间不能有空格
变量名不能使用bash中的关键字
```bash
a=1
echo $a \\打印出1
a=$a+1
echo $a \\打印出1+1
```
bash里可以用(())来执行C风格的算术表达式，比如
```bash
var=0
(( var += 1)) \\var现在是1
(( var++ ))  \\var现在是2
((var = var * var)) \\var现在是4
let 'var = var/3'  \\var现在是1
echo $((var +=2 ))  \\var现在是3
```
转成C之后空格可以被忽略了

只读变量readonly
```bash
a=12
readonly a
a=22 \\执行这句会报错，因为a是只读变量
```

变量类型
* 局部变量 
脚本或命令中定义，仅在当前shell实例中有效，其他shell程序不能访问

* 环境变量
所有程序都能访问环境变量，有些程序需要环境变量来保证正常运行

* shell变量 
shell程序自带的变量


## Shell的流程控制
### if语句
if表达式如果条件命令组为真，则执行then后的部分，如下：
```bash
if
    判断命令，可有多个，真假取最后的返回值
then
    如果上述if为真做什么
elif
    如果上述if为假，继续来做判断
then
    elif为真做什么
else
    如果全都为假做什么
fi \\结束
```

### Exit Status
每条shell命令执行完之后都有一个返回值，这是一个介于0~255之间的值，0表示命令执行成功，非0表示命令执行失败，通过下面例子可以观察一下。

```bash
root@ubuntu:~# ls 
home  test.db  tfcard
root@ubuntu:~# echo $?
0
root@ubuntu:~# cd /bin/usr
-bash: cd: /bin/usr: No such file or directory
root@ubuntu:~# echo $?
1 
```
true和false命令除了以0和一个非零值退出之外什么也不做。
```bash
root@pad-vbox-ubuntu:~# true
root@pad-vbox-ubuntu:~# echo $?
0
root@pad-vbox-ubuntu:~# false
root@pad-vbox-ubuntu:~# echo $?
1
```
true以0值退出，false以非零值退出。

### test
test配合if语句来完成true/false判断，test有两种语法格式：

```bash

test expression \\First form

[ expression ]  \\Second form
```
如果表达式为真，test以0值退出，否则以值1退出。

常用条件测试语句
```bash
-d "filename"
    判断filename是否是一个目录
-e "filename"
    判断filename是否存在
-f "filename"
    判断是否是一个文件
-L "filename"
    判断filename是否是一个符号链接
-r "filename"
    判断filename是否对当前用户可读
-w "filename"
    判断filename是否对当前用户可写
-x "/bin/ls"
    判断/bin/ls是否存并对当前用户可执行
-n "$var"
    判断$var变量是否为空，如果不为空则真
-z "$var"
    判断$var变量是否为空，如果空则真
"$a" == "$b"
    判断$a和$b是否相等
file1 -nt file2
    如果file1的修改时间比file2的新，则为真(newer than)
file1 -ot file2
    older than
```
使用help [[ 、help [和man test查看如何使用

```bash
if [ "${SHELL}" == "/bin/bash" ]; then
    echo "your login shell is the bash"
else
    echo "your login shell is not bash"
fi
```
这里面的空格比较重要，漏掉空格会报语法错误。分号为命令分隔符，使用分号后可以在一行中输入多条命令。
[符号在shell中表示测试即test。

### Exit
脚本结束时设置一个退出状态值是一个好的编程习惯。
```bash
exit 0
```
退出脚本并设置退出状态值为0（表示成功）。

```bash
exit 1
```
退出脚本并设置退出状态值为1（表示失败）。

### test for root

测试当前用户是否为超级用户（root）
```bash
[me@linuxbox me]$ id -u
501
[me@linuxbox me]$ su
Password:
[root@linuxbox me]# id -u
0
```
id -u 命令将显示数字化的用户id（当前用户），当输出为0时表示当前用户为root用户。

```bash
if [ $(id -u) = "0" ]; then
    echo "superuser"
fi

if [ $(id -u) != "0" ]; then
    echo "You must be the superuser to run this script" >&2
    exit 1
fi
```
第一个if测试当前用户是否为superuser，如果是则打印"superuser".
第二个id测试当前用户是否为superuser，如果不是则打印出详细信息并将脚本退出状态置为1.
">&2"表示将输出重定向到标准错误中。&2是为了区分2这个文件。







