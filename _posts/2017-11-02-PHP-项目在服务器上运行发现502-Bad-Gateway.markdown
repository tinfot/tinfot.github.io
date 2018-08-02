---
layout: post
title:  "PHP 项目在服务器上运行发现502 Bad Gateway"
date:   2017-11-02 15:00:24 +0800
categories: Linux Centos
tags: Linux CentOS
---

## 产生错误的原因

连接超时 我们向服务器器发送请求 由于服务器当前链接太多，导致服务器方面无法给于正常的响应,产生此类报错。


## 怎么解决

### 普通访客
一般情况下稍候访问或者按下快捷键 CTRL+F5强制刷新一下，这样会重新向服务器发送请求。

或者清理一下电脑的缓冲文件（浏览器）。

### 管理员

#### 1.检查是否是PHP FastCGI的进程数不够用了
```bash
# 查看PHP是否启动
ps -ef | grep php

# 查看当前的PHP FastCGI进程数是否够用
netstat -anpo | grep "php-cgi" | wc -l

# 输出如下(x是实际的数值)
x
```

如果发现实际使用的"FastCGI进程数"接近预设的"FastCGI进程数"，那么，说明"FastCGI进程数"不够用，我们需要增大预设进程数。

#### 2.检查是否是Nginx的超时时间太短

```bash
# 查看Nginx是否启动
ps -ef | grep nginx

# 进入Nginx的配置文件(path 需要换成实际路径)
vim /path/conf/nginx.conf
```

下面的超时时间是300，如果过短可以设置长一些
```
http
    {
        ......
        fastcgi_connect_timeout 300;
        fastcgi_send_timeout 300;
        fastcgi_read_timeout 300;
        ......
    }
```

#### 3.检查PHP配置的内存是否过小

```bash
# 进入PHP的配置文件(path 需要换成实际路径)
vim /path/etc/php.ini
```
找到`memory_limit = xxxM`

`php.ini`中`memory_limit`设低了会出错，
修改了`php.ini`的`memory_limit`为128M

我这里是设置成以下保存:
```
memory_limit = 128M
```

### 注意
如修改配置后需要重启`Nginx`，`PHP-FPM`
命令如下：

```bash
# 重启Nginx方法一
nginx -s reload

# 重启Nginx方法二
service nginx restart

# 重启PHP-FPM
service php-fpm restart
```