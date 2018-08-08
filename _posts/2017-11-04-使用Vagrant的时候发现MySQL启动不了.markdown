---
layout: post
title:  "使用Vagrant的时候发现MySQL启动不了"
date:   2017-11-04 15:01:24 +0800
categories: Vagrant
tags: Vagrant MySQL
---

有一次在使用`Vagrant`的时候发现不能用了，`MySQL`打不开
然后我看一下是发生了什么事情。

### 检查MySQL进程
```bash
# 列出MySQL进程情况
ps -ef | grep mysql

# 返回空的话，代表进程没有启动
```
发现返回空值，意味着我的进程没有启动。

### 我们试着启动MySQL
```bash
# 启动MySQL服务
systemctl start mysqld.service

# 输出如下：
Job for mysqld.service failed because the control process exited with error code. See "systemctl status mysqld.service" and "journalctl -xe" for details.
```
我们发现没有启动成功，报错了

### 查看硬盘空间
```bash
# 列出磁盘空间情况
df -hl

# 输入以下信息
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root  8.4G  8.4G  12G  100% /
devtmpfs                 488M     0  488M   0% /dev
tmpfs                    497M     0  497M   0% /dev/shm
tmpfs                    497M  6.6M  491M   2% /run
tmpfs                    497M     0  497M   0% /sys/fs/cgroup
/dev/sda1                497M  120M  377M  25% /boot
none                     952G  188G  764G  20% /vagrant
none                     952G  188G  764G  20% /apps/web
tmpfs                    100M     0  100M   0% /run/user/1000
```

发现我们的磁盘空间使用率已经100%了，难怪进程启动不起来了。
我们现在看一下文件都有什么。

### 查看文件占用情况
```bash
# 进入磁盘根目录
cd /

# 列出文件/目录空间占用情况
du -m --max-depth=1

# 输出如下
95	./boot
0	./dev
0	./proc
7	./run
0	./sys
34	./etc
1701	./root
1	./tmp
979	./var
5008	./usr
3	./home
0	./media
0	./mnt
16	./opt
0	./srv
1352	./vagrant
3074	./apps
713	./data
0	./download
12977	.
```
我们发现在`./root`和`./usr`目录中存在较大的使用率，因为`./usr`里面一般装的是应用程序，所以我不去动它，我选择进入`./root`中继续操作

```bash
# 进入/root目录
cd /root

# 列出目录内容（包含隐藏文件）
ls -l

# 输出如下
-rw-------. 1 root root 1.4K Jul 16  2015 anaconda-ks.cfg
-rw-r--r--  1 root root   86 Apr 27  2017 yarn.lock
drwxr-xr-x  1 root root  748 Mar  7  2017 test
```

发现里面含有3个项目，我们再来看看这个目录的空间占用情况
```bash
# 列出文件/目录空间占用情况
du -m --max-depth=1

# 输出如下
1039	./test
```
我们发现`./test`目录占用了我们1G的空间，我们可以选择把它删除或者移到其他的挂载盘去。我们这里直接删除。

```bash
# 删除目录
rm -rf ./test
```

### 重新启动MySQL
```bash
# 启动MySQL服务
systemctl start mysqld.service

# 列出MySQL进程情况
ps -ef | grep mysql

# 输出如下
root     10158     1  0 18:22 ?        00:00:00 /bin/sh /usr/local/mysql/bin/mysqld_safe --datadir=/data/mysql --pid-file=/data/mysql/mysql.pid
mysql    11005 10158  2 18:22 ?        00:00:00 /usr/local/mysql/bin/mysqld --basedir=/usr/local/mysql --datadir=/data/mysql --plugin-dir=/usr/local/mysql/lib/plugin --user=mysql --log-error=/data/mysql/mysql-error.log --open-files-limit=65535 --pid-file=/data/mysql/mysql.pid --socket=/tmp/mysql.sock --port=3306
```
好了，我们的`MySQL`已经启动了，这只是一种无法启动的情况，这次是由于磁盘空间不足导致的。