---
title: 禁用Win10驱动程序强制签名验证
published: 2025-04-14
description: ""
image: ""
tags:
  - windows
  - 右键
category: ""
draft: false
lang: ""
---

执行以下命令（复制后，在命令提示符中单击鼠标右键即可完成粘贴，然后按回车键执行）：

```text
bcdedit.exe /set nointegritychecks on
```

命令瞬间执行完毕，若想恢复默认验证，执行如下命令即可：
```text
bcdedit.exe /set nointegritychecks off
```
