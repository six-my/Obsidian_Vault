---
title: 美化 Ubuntu 中的终端
published: 2025-04-14
description: ""
image: ""
tags:
  - ubuntu
  - 终端
  - 美化
category: ""
draft: false
lang: ""
---

一、给 firefox 或 chrome 先安装好浏览器扩展 chrome-gnome-shell，这个扩展可以使浏览器与本机通信，直接从浏览器上安装任意扩展

二、给本机装好 [gnome-tweaks](https://zhida.zhihu.com/search?content_id=646512492&content_type=Answer&match_order=1&q=gnome-tweaks&zhida_source=entity) [gnome-shell-extensions](https://zhida.zhihu.com/search?content_id=646512492&content_type=Answer&match_order=1&q=gnome-shell-extensions&zhida_source=entity) 这两个扩展，使你可以「可视化」地管理 gnome扩展

```text
sudo apt install chrome-gnome-shell gnome-tweaks gnome-shell-extensions -y
```

三、去 [gnome 官方扩展仓库](https://zhida.zhihu.com/search?content_id=646512492&content_type=Answer&match_order=1&q=gnome+%E5%AE%98%E6%96%B9%E6%89%A9%E5%B1%95%E4%BB%93%E5%BA%93&zhida_source=entity)，直接寻找你要的扩展，如果顺利，可以看到【install】按钮，这个问题就解决了。

如果不顺利，找【Download】并选择契合你当前 gnome 版本的存档：作者仓库链接是公开的，你会看到使用说明。

四、访问 https://link.zhihu.com/?target=http%3A//extensions.gnome.org ，安装 Dash to Panel 扩展，如下：

![](../../assets/vmware/ubuntu/美化%20Ubuntu%20中的终端/美化%20Ubuntu%20中的终端-20251006214803295.webp)

```text
sudo apt install neofetch

neofetch
```