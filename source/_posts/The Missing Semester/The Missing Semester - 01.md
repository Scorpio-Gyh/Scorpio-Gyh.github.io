---
title: Shell
date: 2024-05-14 15:20:08
tags:
categories: The Missing Semester
---
# 中文讲义：[计算机教育中缺失的一课 · the missing semester of your cs education (missing-semester-cn.github.io)](https://missing-semester-cn.github.io/)

# 常用命令

* cd 进入目录
* cd - 上一目录
* pwd 显示当前路径
* ls 列出当前目录下的文件
* mv 移动
* cp 复制
* rm 删除
* mkdir 创建文件夹
* man 参考手册

# 输入输出

* cat打印命令
* '<' 输入重定向
* '>' 输出重定向
* '>>' 输出追加

```
echo hello > hello.txt 输出hello到文件hello.txt
cat < hello.txt        输入hello.txt内容作为cat的
cat  < hello.txt >hello2.txt 将hello.txt的内容作为cat的参数输出到hello2.txt
```

# 管道 |

```
ls -l / | tail -n1 将前一个命令的输出作为后一个命令的输入，实现不同程序的通信
```

* sudo echo 500 > brightness 如果在系统sys内核中改变文件内容，会提示被deny，这是因为内核只是获取sudo echo 500得到的输出结果，它并不关注这个输出结果是以root权限运行，要执行该命令，需要实现通过root权限执行命令
* echo 1060 | sudo tee brightness 也能改变亮度 tee表示输出到文件同时输出到shell
* sudo su 以超级用户获取shell
* \# 超级用户权限执行命令

# Linux文件权限

r 读 w写 x执行
