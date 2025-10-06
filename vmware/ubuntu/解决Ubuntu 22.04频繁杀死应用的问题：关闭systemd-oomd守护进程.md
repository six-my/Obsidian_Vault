---
title: 解决Ubuntu 22.04频繁杀死应用的问题：关闭systemd-oomd守护进程
published: 2025-04-14
description: ""
image: ""
tags:
  - ubuntu
  - systemd-oomd
category: ""
draft: false
lang: ""
---

大多数`systemd`服务都可以通过该`systemctl`实用程序进行管理。在这种情况下，我们要禁用该`systemd-oomd`服务。这可以通过以下方式完成：

```
$ systemctl disable --now systemd-oomd
```

您应该会看到类似的内容（取决于您的操作系统）：

```
$ systemctl disable --now systemd-oomd
Removed /etc/systemd/system/multi-user.target.wants/systemd-oomd.service.
Removed /etc/systemd/system/dbus-org.freedesktop.oom1.service.
```

然后，您可以使用以下方法验证该服务是否已禁用：

```
$ systemctl is-enabled systemd-oomd
```

然后你应该看到：

```
$ systemctl is-enabled systemd-oomd
disabled
```

但是，其他服务可能会尝试重新启动该`systemd-oomd`服务。为了防止这种情况，您可以“屏蔽”该服务。例如：

```
$ systemctl mask systemd-oomd
Created symlink /etc/systemd/system/systemd-oomd.service → /dev/null.
```

然后`systemctl is-enabled`现在应该报告：

```
$ systemctl is-enabled systemd-oomd
masked
```

更多`man systemctl`详情见；特别要注意有关屏蔽`systemd`服务的注意事项。


> 参考链接：
> 
>https://dev59.com/askubuntu/SUfYs4cB2Jgan1znI_3U