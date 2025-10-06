---
title: Ubuntu下 repo的安装
published: 2025-04-14
description: ""
image: ""
tags:
  - ubuntu
category: ""
draft: false
lang: ""
---

方法一：

根目录下创建bin文件夹，并且配置环境变量；

```text
mkdir ~/bin
PATH=~/bin:$PATH   //export PATH="$HOME/bin:$PATH"
```

下载repo脚本，并且给与权限：

 ```text
curl https://mirrors.tuna.tsinghua.edu.cn/git/git-repo > ~/bin/repo
chmod a+x ~/bin/repo
```