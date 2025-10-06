---
title: Linux 常用命令集合
published: 2025-04-18
description: ""
image: ""
tags:
  - Linux
category: ""
draft: false
lang: ""
---

### 清屏

ctrl+l 清屏，往上翻可以查看历史操作

clear 这个命令将会刷新屏幕

### 切换用户

su yao  切换为用户yao

exit 退出当前用户

### 关机 (系统的关机、重启以及登出 )

shutdown -h now 关机

init 0 关机

reboot 重启

### 文件和目录

cd / 进入/目录

cd - 切换到上次访问的目录

pwd 显示工作路径

ls 查看目录中的文件

mkdir dir1 创建一个目录

touch a.txt 创建一个文件

rm -f file1 删除一个文件

rm -rf dir1 删除一个目录

mv dir1 new_dir 重命名/移动 一个目录

cp file1 file2 复制一个文件

### 文件搜索

find /bin -name 'a*' 查找/bin目录下的所有以a开头的文件或者目录

### 文件权限

chmod 777 a.txt 授予所有权限

### 链接

ln -s /opt/a.txt /opt/git/ 对文件创建软链接（快捷方式不改名还是a.txt）

ln /opt/a.txt /opt/git/ 对文件创建硬链接