---
title: VS Code配置C语言开发环境
published: 2025-04-16
description: ""
image: ""
tags:
  - vscode
category: ""
draft: false
lang: ""
---

## 为Windows安装C编译器（MinGW-W64 GCC）

C编译器（MinGW-W64 GCC）的下载地址为：[https://sourceforge.net/project](https://link.zhihu.com/?target=https%3A//sourceforge.net/projects/mingw-w64/)

Home/Toolchains targetting Win64/Personal Builds/mingw-builds/8.1.0/threads-posix/seh/

[x86_64-8.1.0-release-posix-seh-rt_v6-rev0.7z](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/8.1.0/threads-posix/seh/x86_64-8.1.0-release-posix-seh-rt_v6-rev0.7z/download "Click to download x86_64-8.1.0-release-posix-seh-rt_v6-rev0.7z")

然后右键点击“此电脑”，选择最下面的子菜单“属性”

调出“系统”对话框，然后点击“高级系统设置”按钮，调出“系统属性对话框”。

在“系统属性”对话框中的“高级”选项卡下点击“环境变量”按钮，调出“环境变量”对话框。

在“环境变量”对话框中，选中“Path”，点击“编辑”按钮，调出“编辑环境变量”对话框。

在“编辑环境变量”对话框中点击“新建”按钮，在下方粘贴复制的mingw64/bin的地址。

依次点击“确定”按钮，完成C编译器的安装和环境变量配置。如果配置成功，同时按下键盘上的“win+r“键，在出现的”运行“对话框中输入”cmd“回车。

在随后出现的”cmd.exe"窗口中，输入“gcc -v"命令，会出现gcc的版本号，说明安装配置成功。

### vscode

打开后，如下图。左侧边栏是几个快速的按钮，点击最下面的“Extensions”（扩展）按钮。

安装扩展

C/C++ Extension Pack

[ctrl]+[shift]+[p]

>c/c++

c/c++:编辑配置(UI)

编译器路径 选择....

IntelliSense 模式  windows-gcc-x64

### 编译

查看 终端

gcc text.c -o text

./text