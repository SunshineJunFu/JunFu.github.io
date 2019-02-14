---
title: linuxweek7
date: 2019-02-14 14:54:38
tags: linux
---

# shell

## 种类

|种类|描述|
|---|---|
|B-shell|/bin/sh Stephen R.Bourne 于贝尔实验室开发|
|C-shell|/bin/csh, Bill Joy |
|K-shell|/bin/csh, korn shell, David Korn |
|Bash|/bin/bash, Bourne Again shell|

## 功能

+ shell是命令解释器

+ 文件名替换，命令替换，变量替换

+ 历史替换，别名替换

+ 流程控制的内部命令(内部命令和外部命令)

+ 主要用途：批处理，执行效率比算法语言低

`学习bash的目的`: 

+ 交互方式下：熟习shell的替换机制、转义机制，掌握循环等流程控制，
可以编写复合命令

+ 非交互方式：编写shell脚本程序，把一系列的操作，编纂成一个脚本文
件，批量处理

## 具体功能

1. 重定向与管道
2. 方便交互使用的功能：历史替换与别名替换
3. shell变量
4. shell的变量替换，命令替换，文件名替换
5. 元字符，如：单引号，双引号
6. 流程控制
7. 子程序

### 启动交互式shell

注册shell 和 交互式shell的区别？

启动方法

+ 注册shell： 注册shell

+ 键入bash 命令： 交互式shell

+ 脚本解释器

自动执行一批命令(用户偏好)：

+ 注册shell时：自动执行用户主目录下的.bash_profile文件中的命令

+ bash作为注册shell退出时： 自动执行$HOME/.bash_logout

+ bash作为注册shell启动时： 自动执行$HOME/.bashrc

自动执行一批命令(系统级)：

+ 当bashbash bash作为注册shell被启动时:自动执行 /etc/profile 文件中命令

+ 当bash 作为交互式shell启动时： 自动执行 /etc/bash.bashrc

+ 当bash作为注册shell退出时：自动执行/etc/bash.bash.bash_logout

## 历史与别名

### 历史表

查看历史表

history

设置历史表的大小：

修改变量HISTSIZE

历史替换：

+ 人机交互时，直接使用上下箭头

+ ！！ 引用上一条命令

+ ！str 以str开头的最近用过的命令

### 别名 

创建别名：alias h="alias"， 注意等号两端无空格

查看别名表： alias

取消别名： unalias h

### Tab键补全

+ 每行的首个单词： 全搜索$PATH下的命令

+ 行中的其他单词： 当前目录下的文件名


## 重定向

### 输入

|命令|描述|
|---|---|
|<filename| 从filename中获取stdin|
|<<word| 从shell脚本文件中获取数据直到再次遇到定界符word ？？？？|
|<<<word| 从命令行获取信息作为标准输入|

### 输出

|fd|描述|
|---|---|
|0|标准输入， stdin|
|1| 标准输出，stdout|
|2|标准错误输出， stderr|

||命令|描述|
|---|---|
|>filename|将stdout重定向到文件 filename,文件已存在则先清空（ 覆盖方式 |
|>>filename|将stdout重定向 追加 到文件 filename尾|
|2> filename|将文件句柄2重定向到文件 filename|
|2>&1|将文件句柄 2重定向到文件描述符 1|
|cmd1 | cmd2|将cmd1的标准输出作为cmd2的标准输入|
