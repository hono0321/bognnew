---
title: robotframework安装与配置
author: hono
date: 2017-05-27
updated: 2017-05-27
tags: [robotframework,setup,config]
---
### 安装配置

1. 安装python，配置好python环境变量
2. 安装WxPython
3. 安装PyCrypto
4. 安装Robot Framework
5. 安装robotframework-ride
6. 安装一些库

```bash
pip install robotframework-selenium2library
pip install robotframework-archivelibrary
pip install robotframework-SSHLibrary
pip install robotframework-ftplibrary
```
安装完成后运行
```bash
robot --version
rebot --version
```
## 测试语法

### 文件和目录
* 测试用例在测试用例文件中创建
* 测试用例文件自动创建包含该用例文件的测试套件。
* 包含测试用例文件的目录形成一个更高级别的测试套件
* 

* 测试库，包含`最低级别`的关键字
* 资源文件，包含`变量`和`高级别`的`用户关键字`
* 变量文件，提供比资源文件更灵活的方式创建`变量`

### RF支持的文件格式
* html
* tsv
* txt,推荐
* .robot，推荐
* .rst .rest

### 测试数据表
* settings
1. 导入测试库，资源文件和变量文件
2. 定义测试用例和测试套件的元数据
* variables
1. 定义测试数据中其他地方使用的测试数据
* test cases
1. 从可用的关键字中创建测试用例
* keywords
1. 从现有的较低级别的关键字中创建用户关键字

mark

### 解析数据的规则



