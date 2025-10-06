---
title: ubuntu 切换到超级用户失败的解决办法（su 认证失败）
published: 2025-04-14
description: ""
image: ""
tags:
  - ubuntu
  - root
category: ""
draft: false
lang: ""
---

```text
yg@ubuntu:~$ su

密码： 

su：认证失败

yg@ubuntu:~$ su passwd root

没有用户“passwd”的密码项

yg@ubuntu:~$ sudo passwd root

[sudo] yg 的密码： 

输入新的 UNIX 密码： 

重新输入新的 UNIX 密码： 

passwd：已成功更新密码

yg@ubuntu:~$ su

密码： 

root@ubuntu:/home/yg# 
```

```text
linux su认证失败

Ubuntu安装后，root用户默认是被锁定了的，不允许登录，也不允许 su 到 root ，可以通过更改root的密码来登录root。

su 切换root不行所以就用手动形式进行改密码切换。
```

如果确实需要使用root账户，可以通过设置root密码来启用它。这可以通过以下步骤完成：

打开终端。

输入以下命令：

```text
sudo passwd root
[sudo] six-my 的密码： 
新的密码： 
无效的密码： 密码少于 8 个字符
重新输入新的密码： 
passwd：已成功更新密码
```