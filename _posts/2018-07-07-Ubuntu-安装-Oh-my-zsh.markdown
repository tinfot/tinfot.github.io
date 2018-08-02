---
layout: post
title:  "Ubuntu 安装 Oh my zsh"
date:   2018-07-07 13:00:24 +0800
categories: Linux Ubuntu
tags: Linux Ubuntu
---

### 更新依赖
```bash
apt update
```

### 安装`zsh`
```bash
apt-get install zsh
```

### 安装`git`
```bash
apt-get install git
```

### 获取`oh my zsh`
```bash
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

常常在使用命令行的时候会忘记有些命令，或者在使用一些重复的命令的时候，有这个东西就可以节省时间了。

### 从Github上获取`zsh-autosuggestions`插件

```bash
git clone git://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
```

### 将环境变量加入`.zshrc`中

```bash
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh
```

### 重载环境变量

```bash
source ~/.zshrc
```