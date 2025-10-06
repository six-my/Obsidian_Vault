---
title: Windows带文件时间戳和属性复制文件
published: 2025-04-14
description: ""
image: ""
tags:
  - windows
  - 文件复制
category: ""
draft: false
lang: ""
---

### 启动命令行界面

在使用 Robocopy 之前，首先要打开命令行界面。常用方法如下：

- 使用 Windows + R​ 快捷键打开「运行」对话框，输入cmd​或powershell​ ，然后按Ctrl + Shift + Enter​以管理员权限打开「命令提示符」或 Windows PowerShell。
- 在 Windows 11 中，可以右键点击「开始」菜单，选择「终端管理员」以管理员权限打开「Windows 终端」。

大多数情况下，Robocopy 不需要管理员权限。但如果要复制系统文件或访问受保护的目录，可能需要以管理员身份运行。

### 基本命令结构

​​![](../assets/windows/Windows带文件时间戳和属性复制文件/Windows带文件时间戳和属性复制文件-20251006214803322.avif)

Robocopy 是一个没有图形界面的纯命令行工具，它的基本命令格式如下：

复制复制复制复制复制复制复制复制复制

```
robocopy <源目录> <目标目录> [<文件>[ ...]] [<选项>]
```

如果不指定具体的文件，Robocopy 将会复制源目录中的所有文件到目标目录。

### 常用选项

下面列出了一些常用的 Robocopy 参数：

|选项|描述|
|---|---|
|​/S​|复制子目录，不包括空子目录。|
|​/E​|复制子目录，并包括空子目录。|
|​/MIR​|镜像源目录，会删除目标中不在源中的文件和目录。|
|​/MOVE​|移动文件和目录（复制后删除源文件）。|
|​/XO​|排除较旧文件（不复制源中比目标旧的文件）。|
|​/XF​|排除指定文件。|
|​/XD​|排除指定目录。|
|​/Z​|可重启模式，支持断点续传。|
|​/ZB​|结合可重启模式和备份模式。|
|​/B​|备份模式，忽略文件权限限制。|
|​/R:<n>​|失败重试次数（默认一百万次）。|
|​/W:<n>​|重试间隔时间（默认 30 秒）。|
|​/TBD​|在处理网络路径时等待共享定义。|
|​/MT:<n>​|多线程复制（默认 8 线程，最大 128 线程）。|
|​/L​|仅列出文件，实际不执行复制或移动（用于预览）。|
|​/FP​|显示完整文件路径。|
|​/NP​|不显示进度百分比。|
|​/V​|生成详细输出，包括被跳过的文件。|

通过灵活组合这些选项，Robocopy 可以应对各种复杂的文件复制场景，成为 Windows 文件管理不可或缺的工具。

更多详细参数请参考[微软官方文档](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/robocopy)。

## Robocopy 使用实操

Robocopy 执行操作时会直接移动、删除或覆盖文件，不会弹出任何确认提示。因此，如果路径或选项设置有误，可能导致不可恢复的数据丢失。在运行命令前，请务必仔细检查参数。建议先使用/L​选项进行测试，预览操作结果。

### 多线程复制

Robocopy 是理想的备份工具。以下是将 D:\ 盘的所有文件备份到 E:\Backup\ 的示例命令：

```
robocopy D:\ E:\Backup\ /E /XO /XD "$RECYCLE.BIN" "System Volume Information" /R:3 /MT:32 /V
```

​![](../assets/windows/Windows带文件时间戳和属性复制文件/Windows带文件时间戳和属性复制文件-20251006214803340.avif)​

关键参数解析：

- ​/XO​：只复制新增或修改过的文件，提升后续备份效率。
    
- ​/XD​：排除特定目录：
    
    - ​$RECYCLE.BIN​：回收站目录。
    - ​System Volume Information​：系统卷信息目录。
- ​/MT:32​：使用 32 个线程进行多线程复制。
    

线程选择建议：

- SSD 到 SSD：16 到 32 线程，最多可尝试 64 线程。
- HDD 到 HDD：8 到 16 线程，最多 24 线程。
- SSD 与 HDD 之间：12 到 24 线程较为合适。

使用/MT​参数时，需根据文件大小选择线程数。大文件不需要太多线程；小文件可增加线程数。但超过 64 线程通常无法带来显著的性能提升，反而可能增加系统负担。

### 网络共享备份

将本地文件夹备份到网络共享的命令示例如下：

```
# 使用指定凭据，先建立网络连接：
net use \\NetworkServer\Backup /user:domain\username password
# 将文件备份到网络共享
robocopy C:\ImportantData \\NetworkServer\Backup\ImportantData /MIR /TBD /ZB /W:5 /R:3 /MT:16
```

​​![](../assets/windows/Windows带文件时间戳和属性复制文件/Windows带文件时间戳和属性复制文件-20251006214803364.avif)

关键参数解析：

- ​/MIR​：镜像复制，保持源和目标目录一致。
- ​/TBD​：在目标路径中的共享名称尚未可用时进行等待。
- ​/ZB​：结合可重启模式和备份模式，适用于不稳定的网络环境（断点续传），同时可以绕过某些权限限制。
- ​/W:5 /R:2​：失败后等待 5 秒，最多重试 3 次。

通过这些设置，Robocopy 可以高效地将重要文件备份到网络共享，并在出现网络中断或权限问题时继续执行，保障数据传输的稳定性。

### 大文件传输

优化大文件（如视频文件等）的传输：

```
robocopy E:\LargeFolder F:\FolderBackup /E /J
```

​​![](../assets/windows/Windows带文件时间戳和属性复制文件/Windows带文件时间戳和属性复制文件-20251006214803385.avif)

关键参数解析：

- ​/J​: 正常情况下，Windows 会使用系统缓存来优化文件读写操作。该选项会绕过系统缓存，直接进行磁盘 I/O 操作。主要用于超过 8MB 以上大文件的无缓存 I/O 传输，但不适用于多线程。

### 文件复制（排除特定文件类型）

复制文件夹时排除特定文件类型和目录：

```
robocopy C:\SourceFolder D:\DestinationFolder /E /XF *.tmp *.log /XD node_modules /MT:16 /V
```

关键参数解析：

- ​/XF​：排除.tmp​和.log​ 文件。
- ​/XD​：排除node_modules​目录。

### 移动文件

将文件从一个位置移动到另一个位置：

```
robocopy C:\OldFolder D:\NewFolder /MOVE /E /MT:16 /V
```

关键参数解析：

- ​/MOVE​：移动并删除源文件。

### 带权限复制文件和文件夹

在复制过程中保留原始权限结构，这对于系统迁移、备份和权限敏感的环境特别有用：

```
robocopy C:\SourceFolder D:\DestinationFolder /E /COPY:DATSOU /DCOPY:DAT /SECFIX /TIMFIX
```

关键参数解析：

- ​/COPY:DATSOU​: 复制所有文件信息（数据、属性、时间戳、安全性、所有者、审计信息）。
- ​/DCOPY:DAT​: 复制目录信息（数据、属性、时间戳）。
- ​/SECFIX​: 修复文件的 NTFS 权限。
- ​/TIMFIX​: 修复文件的时间戳。
