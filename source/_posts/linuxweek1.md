---
title: linuxweek1
date: 2019-01-12 00:25:58
tags: week1
categories: linux
---

# Linux 

## 基本的Linux命令
+ man
    * man name or man section name or man -k regexp
    * section: 1 ->命令,  2 -> 系统调用 3 -> 库函数
+ date
    * 读取系统的时间和日期
    * 定制输出格式： data ”+%Y-%m-%d %H:%M:%S Day %j" or data "+%s"
    * ntpdate 0.pool.ntp.org 设置时间（only root)
    * ntpdate -q 0.pool.ntp.org 查询时间
+ cal
    * cal  当前月份日历
    * cal year 
    * cal month year
+ bc
    * bc 缺省精度为数点后0位
    * bc -l 缺省精度为数点后20位
+ passwd
    * passwd 普通用户修改登陆密码
    * passwd 用户名 root 可重置用户的密码

## 了解系统状态的命令
+ who
    * 确定谁在系统中
    * who 列出已登录系统的用户
    * who am i 
    * whoami
+ uptime
    * 已开机时间， CPU负载， 登陆用户数
+ top
+ free
+ vmstat
+ ps
    * e 所有进程
    * f full格式
    * l long格式

## 文本处理命令
+ more/less
    * more filename
    * 满屏后，空格翻页，换行，向下滚动一行， q 退出， /pattern， /
    * 不支持回退
    * less is more， 可利用上下箭头，支持回退
+ cat/od
    * 文本格式打印 cat filename or cat -n filename 带行号
    * od 逐字节打印
    * od -c/-t c(字符), -t x1(16进制), -t d1(10进制), -t u1(8进制)
+ head/tail
    * head -n number filename / tail -n number filename
    * head -n -number filename 除末尾number行，均为头
    * tail -n +number filename  除首number行，均为尾
    * tail -f filename 实时打印文件尾部信息
+ tee
    * tee filename 将stdin的数据送到stdout，并存入文件filename
+ wc
    * 列出文件一共多行，多少单词，多少字符
    * 文件大于1，会有个合计
    * -l 只输出行
+ sort
    * sort -n 按数值大小排顺序，不按字符比较规则
+ tr
    * 翻译字符
    * tr string1 string2 翻译string1至string2
+ uniq 
    * uniq options inputfile 
    * -u 只保留没有重复的行（只打印一次）
    * -d 只保留重复的行（只打印一次）
    * -c 计数同样的行出现的次数
