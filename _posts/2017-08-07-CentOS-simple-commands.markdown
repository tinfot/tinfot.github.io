---
layout: post
title:  "CentOS 简单常用命令"
date:   2017-08-07 15:00:24 +0800
categories: Linux Centos
tags: Linux CentOS
---

### 注释
- [*path] 可替换路径。eg. home || dev

#### 系统

##### 清空面板

```bash
clear
```

##### 查看当前时间

```bash
date
```

---

#### 目录

##### 输出当前完整目录

```bash
pwd
```

##### 进入目录

```bash
cd /[*path]
```

##### 返回上一级目录

```bash
cd ..
```

---

#### 文件

##### 解压tar.gz 文件

```bash
tar -zxvf xxxx.tar.gz [*path]
```


##### 列出当前目录下文件详细信息(包括隐藏文件)

```bash
ls -al
```

---

#### 用户

##### 查看活跃用户

```bash
w
```

---

#### 进程

##### 实时显示进程状态

```bash
top
```

##### 查看进程

```bash
#查看nginx进程
ps -ef | grep nginx
```

---

#### 磁盘

##### 列出各分区使用情况

```bash
df -h
```

##### 查看所有分区

```bash
fdisk -l
```

---

#### 网络

##### 查看IP

```bash
ip addr
```

##### 查看网卡配置

```bash
ifconfig
```

##### 查看所有监听端口

```bash
netstat -lntp
````

##### 查看所有已经建立的连接

```bash
netstat -antp
```