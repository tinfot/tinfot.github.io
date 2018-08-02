---
layout: post
title:  "CentOS crontab"
date:   2017-07-07 15:00:24 +0800
categories: Linux Centos
tags: Linux CentOS
---

### 基本命令


列出所有计划任务
```shell
crontab -l
```

编辑计划任务
```shell
crontab -e
```
查看计划任务日志
```shell
tail -f /var/log/cron
```

### 基本语法

**Minute** **Hour** **Day** **Month** **DayOfWeek** **Command**

**分** **时** **日** **月** **星期几** **命令**
```shell
* * * * * root /usr/bin dosomething.sh
```

### 例子

每晚的21:30重启nginx
```shell
30 21 * * * root /usr/local/bin/nginx -s restart
```

每月1、10、22日的04:45重启nginx
```shell
45 4 1,10,22 * * root /usr/local/bin/nginx -s restart
```

每周六、周日的01:10重启nginx
```shell
10 1 * * 6,0 root /usr/local/bin/nginx -s restart
```

每天18 : 00至23 : 00之间每隔30分钟重启nginx
```shell
0,30 18-23 * * * root /usr/local/bin/nginx -s restart
```

每星期六的11 : 00 pm重启nginx
```shell
0 23 * * 6 root /usr/local/bin/nginx -s restart
```

晚上11点到早上7点之间，每隔一小时重启nginx
```shell
* 23-7/1 * * * root /usr/local/bin/nginx -s restart
```

每一小时重启nginx
```shell
* */1 * * * root /usr/local/bin/nginx -s restart
```

每月的4号与每周一到周三的11点重启nginx
```shell
0 11 4 * mon-wed root /usr/local/bin/nginx -s restart
```

一月一号的4点重启nginx
```shell
0 4 1 jan * root /usr/local/bin/nginx -s restart
```

每半小时同步一下时间
```shell
*/30 * * * * root /usr/sbin/ntpdate 192.168.1.2
```

以用户www的身份每两小时就运行某个程序
```shell
0 */2 * * * www /usr/bin/somecommand  >>  /dev/null 2>&1
```

每分钟执行一次脚本
```shell
*/1 * * * *  /home/somedir/scripts.sh
```

每分钟sleep3秒后执行脚本
```shell
*/1 * * * * sleep 3 &&  /home/somedir/scripts.sh
```

每分钟sleep6秒后执行脚本
```shell
*/1 * * * * sleep 6 &&  /home/somedir/scripts.sh
```