---
title: 用户友好的shell程序 —— Fish shell的安装和配置
published: 2025-04-14
description: ""
image: ""
tags:
  - Fish_shell
category: ""
draft: false
lang: ""
---

### 安装Fish

```text
sudo apt install fish
```

### 配置Fish

由于大部分的终端默认 shell 使用的是 Bash，所以我们还需要修改当前使用的 shell 为 Fish。

我们首先通过命令“echo $SHELL”查看当前 shell：

```text
$ echo $SHELL
/usr/bash
```

2、输入命令“cat /etc/shells”查看操作系统中存在的 shell 有那些：

```text
$ cat /etc/shells
/usr/bin/fish  <--  我们发现Fish已经安装
```


3、切换 shell，在这里通过chsh命令实现

```text
$ chsh -s /usr/bin/fish
```


假如，同时希望root模式下也可以使用该 shell，则可以在该命令后面加上root关键字

```text
$ chsh -s /usr/bin/fish root
```


4、重启电脑，这个时候，Fish 应该已经安装完毕！