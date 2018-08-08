---
layout: post
title:  "Vagrant 设置固定IP"
date:   2017-08-07 15:01:24 +0800
categories: Vagrant
tags: Vagrant
---

首先在`Vagrantfile`文件中设置IP配置

```bash
# 打开配置
vim Vagrantfile

# 修改 xxx 为你所要的实际值
config.vm.network "public_network", ip: "192.168.1.xxx"
```

启动`vagrant`:
```bash
vagrant up
```

进入`vagrant`:
```bash
# 进入虚拟机
vagrant ssh

# 切换到超管用户
sudo su -

# 重启网卡(可能会有错误提醒)
systemctl restart network.service
```

查看当前网卡设置:
```bash

cd /etc/sysconfig/network-scripts/



ip addr

en0: 以太网

systemctl enable NetworkManager-wait-online.service
```

如果能看到你设置的网卡IP地址则成功了
```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 08:00:27:de:0e:0e brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic enp0s3
       valid_lft 85726sec preferred_lft 85726sec
    inet6 fe80::a00:27ff:fede:e0e/64 scope link
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1492 qdisc pfifo_fast state UP qlen 1000
    link/ether 08:00:27:8b:cd:92 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.xxx/24 brd 192.168.1.255 scope global enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe8b:cd92/64 scope link
       valid_lft forever preferred_lft forever
```