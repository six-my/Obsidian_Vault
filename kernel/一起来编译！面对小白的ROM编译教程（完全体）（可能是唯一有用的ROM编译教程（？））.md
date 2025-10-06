---
title: 一起来编译！面对小白的ROM编译教程（完全体）（可能是唯一有用的ROM编译教程（？））
published: 2025-05-07
description: ""
image: ""
tags:
  - rom
category: ""
draft: false
lang: ""
---

我最近本闲来无事，可登时却有一位A先生找上门来。一问何事，才知，A先生觉得自己在编译ROM的知识方面有所欠缺，这才上门来拜见我了。想到我把这篇文章写出来给大家看看也是很不错的，我便带着大家把编译ROM这一门学问好好讲一讲。  
  
1.VPS的选择 (如果诸位自己有编译环境的话可以跳过这一步，配置环境这一步后几天讲)  
  
什么是VPS？这里大家可谓有所不知，VPS是由几个洋文组成起来的缩写。V，是代表着虚拟，P，是代表着私人，S，是代表着服务器，组成起来便成为了虚拟私人服务器这一名词。想必大家也都听我解释厌烦了，那我就来说说怎么选择“虚拟私人服务器”罢。 

(装不来鲁迅了，不装了)  

首先，我们先来看看能跑包的最低配置。 运存是比较重要的一件事，要用来编译的服务器肯定是8GB起步，不然到后面编译systemui的时候够你好受的。  

核心也很重要，核心这玩意，在VPS中当然是越多越好，安卓编译也是很能利用多核心的优势。  

还有，不要问我: 大佬大佬，Swap行不行啊？答案就一个，不行。  

为啥？举个栗子，A先生有4GB物理内存，他加了8G的Swap，结果没跑到一半系统kernel panic了。  
为啥？Swap效率太低了，而且容错率也不是很高，在编译的时候没卵用。  
  
老朋友，硬盘问题： 硬盘怎么选？当然是越快越好。如果速度优先，能不用普通sata的SSD就不用，尽量用高性能nvme。你普通编译一个设备，一个ROM的话源码空间也一般占不了多少，但如果你要编译多设备或者多ROM，容量往上选。一个基于los的ROM源码一般不超过100GB，基于AOSP的一般不超过200G。

然后是网络：  

国外服务器 国外服务器 国外服务器 

这里给大家推荐几个运营商:  

aws: aws的账户闲鱼上满多的，还比较便宜，而且aws的虚拟nvme效率是真的顶，给个好评  

vultr: vultr唯一的优点就是能白嫖。10\$换100$，针不辍。 Azure: azure不仅能白嫖，而且配置十分多样，微软的服务很给力，什么小玩意都有。  

Linode: 没用过，A君说好用（）  

最后，系统：  

首选Ubuntu18.04LTS。

首选Ubuntu18.04LTS。

首选Ubuntu18.04LTS。  
  
为什么？  
1. Ubuntu Server相较于其它几个服务器系统，比较稳定（18.04可是LTS）  
2. Ubuntu Server上的编译环境比较好搞  
3. Ubuntu Server比较适用于新手 然后就到了下一步——  
  
2.开始配置编译环境  

从运营商那里拿到你的用户名和密码，用SSH链接上你的服务器，然后我们就准备开始了！！！  
  
是不是很兴奋？当然，我们连第一步都没有跨出，谈何兴奋（  
  
在你的终端里面输入: 

```text
sudo apt-get update && sudo apt-get install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev libwxgtk3.0-dev  
```
  
输入了root用户的密码，一路都选y，如果没有任何报错，那么恭喜你！你已经成功的安装了依赖！下一步，就是安装仓库管理工具，Repo。

如果你连git是啥都不知道那也没关系，命令一样打就是了（  
  
在你的终端里面一行一行输入：  

```text
mkdir -p ~/bin  
curl [查看链接](https://storage.googleapis.com/git-repo-downloads/repo "https://storage.googleapis.com/git-repo-downloads/repo") > ~/bin/repo  
sudo cp ~/bin/repo /bin/repo  
sudo chmod a+x /bin/repo  
```
  
repo就这样被你安装好了。  
  
下一步呢？  
  
下一步便是输入你的git用户名和邮箱  
  
在你的终端里面一行一行输入： 

（替换xxx@xxx.com为你自己的邮箱，xxx为你自己的用户名，不用注册，不用去引号，嗯）  

```text
git config --global user.email "xxx@xxx.com"  
git config --global user.name "xxx"  
```

  
新建一个文件夹，这个文件夹会用来存放你的源码：  
  
在你的终端里面一行一行输入：  

```text
mkdir ~/文件夹的名字  
cd ~/文件夹的名字  
```

  
现在，我们位于你刚刚创建的文件夹里面。  

接下来我们要做的就是开始同步源码。  

问题来了，你一断开SSH源码同步就会断掉，怎么办？ 先别去啥百度上找解决方案了，GNU给了我们解决方案——screen。  
  
Screen会创建出一个平行工作区，即使你断开了SSH/putty链接，这个工作区里面的东西也会保留下来。  
  
下面我们创建一个名为sync的工作区，然后进去:  
  
在你的终端里面输入: screen -S sync  
  
好了，我们进入到这个名字叫sync的工作区里了，下一步就是开始同步源码。 找到你要编译的ROM的manifest仓库/android仓库/android_manifest仓库（三选一，有哪个选哪个），在分支选择那里选择你需要的安卓版本（也可能是lineage-17.1 lineage-18.1之类的）然后查看一下README.MD文件。

举个栗子，exTHmUI:

![[../assets/kernel/一起来编译！面对小白的ROM编译教程（完全体）（可能是唯一有用的ROM编译教程（？））/一起来编译！面对小白的ROM编译教程（完全体）（可能是唯一有用的ROM编译教程（？））-20251006214801288.jpeg]]

<p align="center">初始化仓库</p>

看到那张图了吗？（如果是英文请找带有repo init的语句）  
  
非常好，下面在你的终端里输入repo init那个语句，输入y之后我们就可以真正的开始同步源码了。  
（如果说/usr/lib/python啥的没找到，输入sudo apt install python） 

在你的终端里输入: 

```text
repo sync -j$(nproc --all) -c -j$(nproc --all) --force-sync --no-clone-bundle –no-tags 
```

这个时候看到一大片输入那就表示你做的对，Git已经开始在clone东西了。  
  
这时候，使用快捷键: Ctrl+A+D， 我们便可以暂时离开这个工作区。这个时候你可以去泡杯咖啡，等待repo sync干完它的事情。  
  
大概过了半个小时，我们回来看看： 在你的终端里输入: screen -R sync  
  
如果输出已经结束了，而且最后一行是repo sync finished successfully，那么就可以开始下一步，也是最后一步了。  
  
3.林克，是大头！（划去）编译  
（这里假定的你设备已经有OFFICIAL，并且设备树已经加入了manifest豪华套餐）  
  
让我们先了解一下你需要知道的源码的大体结构吧:  

源码目录/  
+ device  
+ vendor  
+ kernel  

首先，如果你想要成功的做出一个包，需要三个东西，设备树(device)，厂商私有文件(vendor)，内核源码(kernel)。  
  
拿小米6举个栗子：  

源码目录/  
+ device/  
+ xiaomi/  
+ sagit/（这里面放的是设备树文件）  
+ vendor/  
+ xiaomi/  
+ sdm-835-common（可能写错了，这个common的vendor有的设备/厂家没有）  
+ sagit  
+ kernel/  
+ xiaomi/  
+ msm8998（没记错的话）  
  
那么，我们下一步就是找到这些文件了，去哪里找？GitHub啊！  
  
首先处理一下vendor： 去Github注册一个帐号，搜索 proprietary_vendor_厂商英文名，应该就会一大堆。  
  
点进第一个仓库，看一下分支。

用左上角的分支切换到你需要的安卓版本后，应该是一大堆文件夹，没关系，我们全部都用git克隆下来：  

在终端里面输入  

```text
git clone https://github.com/那个仓库拥有者的用户名/仓库名(就是proprietary_vendor_厂商英文名) -b 你所选择的安卓版本的分支 vendor/厂商英文名  
```

回车，过了一会之后百分百，很好，那么我们的vendor就处理好了。  
  
Kernel是一样的道理，在github上搜索 android_kernel_厂商名_芯片组名(举个栗子，msm8998是835，sdm-845是845，这里也可能是设备名），找到之后点开一个仓库，是一大堆大杂烩，查看你所需要的安卓版本对应的分支名，把它clone下来:  
  
在你的终端里面输入:  
  
```text
git clone https://github.com/那人的用户名/仓库名 -b 分支名 kernel/厂商名/设备名或者是芯片组名  
```

回车，等进度条百分百即可  

这个时候你还差一步之遥就可以开始编译了！  

在终端里输入:  

```text
. build/envsetup.sh
```
 
 没有输出或者输出是include xxx是正常的  
  
下一步：

在终端里输入

lunch ROM缩写_设备代号-userdebug

等一会，lunch应该会自动把你的device tree克隆下来（如果显示error，那么很可能表明你的设备还没有被官方适配，这个后面几篇帖子再说处理方式）

等lunch结束之后，输入：  

```text
mka bacon 
``` 
  
看到那滚动起来的输出了吗？我们成功了！你成功了！你正在编译你的第一个安卓ROM！  

（如果有error下期讲处理，如果你真的很急欢迎到我的论坛提问，如果你觉得自己有解决能力的，请访问Stackoverflow论坛查找解决方案）  

这个时候脱离工作区，关闭SSH，你可以去看场电影，或者是睡个觉。 醒来之后，访问工作区，如果看到successful之类的字眼，那么恭喜你，你的第一个包成功做出来了。  
  
怎么下载？好问题，请百度搜索scp的使用方式，这里就不多提了。  
  
编译的包的地址在 ~/源码目录/out/target/product/设备代号/下， 名字应该是 ROM名-日期-设备名-UNOFFICIAL（或者是其它啥乱七八糟的玩意）.zip  
  
用scp把它下载下来，用twrp刷入，完成了~  
  
如果你对编译有问题，或者是想要交个朋友的，欢迎来我的论坛做客: forum.tasariya.tk 注册个帐号即可发帖，如果我看到都会回复的（  

> 参考链接：
> 
>https://www.coolapk.com/feed/24966438?shareKey=Mjc4ODRkZmE2YTQyNjgxYWZkMTc~&shareUid=5827990&shareFrom=com.coolapk.market_15.2.2