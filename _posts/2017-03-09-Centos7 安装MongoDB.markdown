---
layout: post
title:  "Centos7 安装MongoDB"
date:   2017-03-09 15:01:24 +0800
categories: CentOS
tags: Linux CentOS MongoDB
---

## 安装MongoDB

### 1.从官网下载MongDB

[官方网址](https://www.mongodb.com)下载对应的版本到你需要安装的电脑(服务器)上

### 2.解压MongoDB
``` shell
tar -zxvf mongodb-linux-x86_64-3.4.2.tgz
```

### 3.把MongoDB移动到程序目录/usr/local/mongdb
``` shell
mv mongodb-linux-x86_64-3.4.2.tgz /usr/local/mongodb
```

### 4.加入环境变量
``` shell
export PATH=/usr/local/mongodb/bin:$PATH
```

### 5.新建数据储存目录
``` shell
mkdir /data/mongodb
```

### 6.启动MongoDB
``` shell
mongod --fork --logpath /var/log/mongodb.log --logappend --dbpath /data/mongodb
```

### 7.查看程序进程
``` shell
ps -ef | grep mongod
```