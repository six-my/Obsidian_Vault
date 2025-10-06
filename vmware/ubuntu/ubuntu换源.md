---
title: ubuntu换源
published: 2025-04-14
description: ""
image: ""
tags:
  - ubuntu
  - 镜像源
category: ""
draft: false
lang: ""
---

### 查询版本

先查询自己系统的版本号：

```text
lsb_release -a
```

```text
ma@cw:~$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 20.04.6 LTS
Release:        20.04
Codename:       focal
ma@cw:~$ 
```

可以看出我系统版本是 Ubuntu 20.04.6 LTS，注意这个开发代号Codename，Ubuntu每一个版本都有一个代号，这个一定要跟国内源对应，否则会出问题。

### 清华大学开源软件镜像站

[ubuntu | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

### 更换方法

```text
sudo vim /etc/apt/sources.list
```


或

```text
sudo nano /etc/apt/sources.list
```

### 执行更新源命令

sudo apt update