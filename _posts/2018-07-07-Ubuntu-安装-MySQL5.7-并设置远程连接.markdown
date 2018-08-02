---
layout: post
title:  "Ubuntu 安装MySQL5.7 并 设置远程连接"
date:   2018-07-07 14:00:24 +0800
categories: Linux Ubuntu
tags: Linux Ubuntu
---

### 更新依赖
```bash
apt update
```

### 安装...会提示输入`root`账号的密码，需要输入2次
```bash
apt-get install mysql-server
```

### 进入`MySQL`
```bash
mysql -u root -p
```

### 首先我们授权一个叫remote(自定义)的账户，并允许远程连接
```bash
# 添加用户
GRANT ALL PRIVILEGES ON *.* TO 'remote'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;

# 刷新数据库
FLUSH PRIVILEGES;

# 退出数据库
exit;
```

### 修改`MySQL`的配置
这个是绑定可以连接的IP
```bash
# 打开配置文件
vim /etc/mysql/mysql.conf.d/mysqld.cnf

# 讲下面这行给注释(前面加个#)
bind-address = 127.0.0.1 ==> # bind-address = 127.0.0.1
```

### 重启`MySQL`以加载配置
```bash
service mysql restart
```