---
layout: post
title:  "CentOS screen 命令"
date:   2017-12-01 15:01:24 +0800
categories: Linux Centos
tags: Linux CentOS
---

## 功能描述

使用`telnet`或`SSH`远程登录`Linux`时，如果连接非正常中断（远程机器关闭），重新连接时，系统将开一个新的`SESSION`，无法恢复原来的`SESSION`。

`screen`命令可以解决这个问题。Screen工具是一个终端多路转接器，在本质上，这意味着你能够使用一个单一的终端窗口运行多终端的应用。

## 语法

```bash
screen [-opts] [cmd [args]]
```
或者
```bash
screen -r [host.tty]
```

## 案例

#### 开启
开启并且进入这个窗口
```bash
screen -S test
```

#### 退出
退出这个窗口
```bash
Ctrl+A D
```

#### 查看
查看所有窗口
```bash
screen -ls
```

#### 重连
重新进入这个窗口
```bash
screen -r test
```

#### 关闭
关闭这个窗口
```bash
exit
```

