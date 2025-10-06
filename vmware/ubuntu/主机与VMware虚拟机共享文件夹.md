---
title: 主机与VMware虚拟机共享文件夹
published: 2025-04-14
description: ""
image: ""
tags:
  - VMware
category: ""
draft: false
lang: ""
---

打开 “设置 -> 选项 -> 共享文件夹”

![](../../assets/vmware/ubuntu/主机与VMware虚拟机共享文件夹/主机与VMware虚拟机共享文件夹-20251006214803132.jpg)

选择“总是启用”

![](../../assets/vmware/ubuntu/主机与VMware虚拟机共享文件夹/主机与VMware虚拟机共享文件夹-20251006214803164.jpg)

点击“添加”，进入共享文件夹向导。点击“下一步”

![](../../assets/vmware/ubuntu/主机与VMware虚拟机共享文件夹/主机与VMware虚拟机共享文件夹-20251006214803184.jpg)

点击“浏览”

![](../../assets/vmware/ubuntu/主机与VMware虚拟机共享文件夹/主机与VMware虚拟机共享文件夹-20251006214803206.jpg)

选择需要共享的文件夹，点击确定

![](../../assets/vmware/ubuntu/主机与VMware虚拟机共享文件夹/主机与VMware虚拟机共享文件夹-20251006214803227.jpg)

“名称”即虚拟机中显示的名称，点击“下一步”->“完成”

![](../../assets/vmware/ubuntu/主机与VMware虚拟机共享文件夹/主机与VMware虚拟机共享文件夹-20251006214803244.jpg)

用此方法添加全部需要共享的文件夹，添加完成后如下图所示，点击“确定”

![](../../assets/vmware/ubuntu/主机与VMware虚拟机共享文件夹/主机与VMware虚拟机共享文件夹-20251006214803265.jpg)

终端运行

```text
sudo apt install open-vm-tools-desktop

sudo mkdir /media/vmware

sudo mount -t fuse.vmhgfs-fuse .host:/ /media/vmware -o allow_other
```

```text
su

echo '.host:/ /media/vmware fuse.vmhgfs-fuse allow_other,defaults 0 0' >> /etc/fstab
```

重启


> 参考链接：
> 
>https://zhuanlan.zhihu.com/p/650638983