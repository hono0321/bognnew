---
title: Shell编程学习笔记（二）
author: hono
date: 2017-1-1 15:29:15
updated: 2017-1-3 22:13:42
tags: [linux,shell,编程]
---

### Empty Variables

```bash
a=1
b=
```
b是空变量。变量值是nothing，有点类似声明变量吧<!--more-->

```bash
#!/bin/bash -x
```
在shebang中加-x，可以打印出整个脚本的执行过程。类似编程语言的tracing。
使用set -x命令开启tracing，set +x关闭tracing。用于脚本中部分执行过程的追踪。
```bash
#!/bin/bash
number=1

set -x
if [ $number = "1" ]; then
    echo "Number equals 1"
else
    echo "Number does not equals 1"
fi
set +x
```

### read command
交互式脚本中使用read请求输入
```bash
#!/bin/bash
echo -n "Enter sonme text > "
read text
echo "You entered: $text"
```
read命令后的text是变量名，read将输入的值赋给这个变量，如果read后未跟任何变量名称，那么read会将输入的值付给系统的环境变量REPLY。
read的两个可选参数-t，-s，-t表示等待时间后跟梳子，以秒为单位，-s表示不显示输入内容。
```bash
#!/bin/bash
echo -m "Please Input Your Password > "
read -t 10 -s pswd
```

## 算术运算
运算表达式加双括号((1+1))
```bash
echo $((1+1)) \\输出4
```

加减乘除取余

## 流控制2

case语句
```bash
case word in
    patterns ) commands ;;
esac
```
```bash
#!/bin/bash

echo -n "Type a digit or a letter > "
read character
case $character in
                                # Check for letters
    [[:lower:]] | [[:upper:]] ) echo "You typed the letter $character"
                                ;;

                                # Check for digits
    [0-9] )                     echo "You typed the digit $character"
                                ;;

                                # Check for anything else
    * )                         echo "You did not type a letter or a digit"
esac
```
*标识匹配任何模式。

## 循环
* while、until、for

### while command
```bash
#!/bin/bash

number=0
while [ "$number" -lt 10 ]; do
    echo "Number = $number"
    number=$((number + 1))
done
```

### until command
```bash
#!/bin/bash
number=0
until [ "$number" -ge 10 ]; do
    echo "Number = $number"
    number=$((number + 1))
done
```

### 编写一个菜单程序

```bash
#!/bin/bash
press_enter()
{
    echo -en "\nPress Enter to continue"
    read
    clear
}
selection=
until [ "$selection" = "0" ]; do
    echo "
    PROGRAM MENU
    1 - display free disk space
    2 - display free memory
    0 - exit program
"
    echo -n "Enter selection: "
    read selection
    echo ""
    case $selection in
        1 ) df ; press_enter ;;
        2 ) free ; press_enter ;;
        0 ) exit ;;
        * ) echo "Please enter 1, 2, or 0"; press_enter
    esac
done
```

计算机挂起通常是某部分程序陷入死循环了。

## 位置参数
$0
$1
...

shift命令

## 流控制 for
```bash
for variable in words; do
    commands
done
```







