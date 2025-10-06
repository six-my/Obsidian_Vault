---
title: 旧内核也吃KernelSU计划 - 第一期
published: 2025-05-07
description: ""
image: ""
tags:
  - kernel
category: ""
draft: false
lang: ""
---

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801307.jpeg)

## 序言

我首先科普一个概念，什么是Non-GKI内核，也就是旧内核呢？我们常说的Non-GKI包括了GKI1.0（内核版本4.19-5.4）（5.4为QGKI）和真正Non-GKI（内核版本≤4.14），5.4内核最接近GKI，所以对KPROBE和部分特性支持最好。  
  
那么我们知道这个概念后，就可以收拾收拾准备开始我们的第一课了。但在开头，我们要强调，不要知其然而不知其所以然，大家的确可以直接Fork我的项目，例如编译EvoX的小米Mix2s内核，你可以Fork后自己直接执行Action就可以获取，但是一旦EvoX对内核进行了更新，产生错误后，你就会束手无策，这不好，所以我们第一期就是来讲，本地编译内核。  
  
Ads. 欢迎Fork或Star我的Non-GKI内核自动化项目，不管是自用，还是参与贡献，完全欢迎（禁止用于付费喔）  

地址：[查看链接](https://github.com/JackA1ltman/NonGKI_Kernel_Build "https://github.com/JackA1ltman/NonGKI_Kernel_Build")  
  
## 安装Linux 

如何搭建一个环境呢？  

大部分情况下，你在Windows使用VMware或WSL2都是够用的，即便不能完整调用所有性能。所以这期我们不教授双系统相关内容，就假设我们使用VMware。  

在博通收购VMware后，大发慈悲的给个人用户免费使用全功能的Workstation了，因此我们只需要去VMware官网：[VMware官网](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion "https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion")，注册下载，安装过程不再赘述。  

安装好后，你会看到

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801344.png)

<p align="center">VMware界面</p>

这说明安装成功了，但因为你选择不是WSL，因此我们还需要去下载Linux发行版镜像。我们要开发编译用，其实比较好用的有这几个：Debian，Ubuntu，Arch 这说明安装成功了，但因为你选择不是WSL，因此我们还需要去下载Linux发行版镜像。我们要开发编译用，其实比较好用的有这几个：

Debian，Ubuntu，Arch  

Ubuntu：商业化较为浓厚的发行版，并且强制绑定Snap，但是其软件包比Debian要新，而且是非常常见的编译开发环境，因此本教程选择Ubuntu 24.10  

Debian：服务器常见发行版，Ubuntu也是基于Debian而来的，但是Debian的软件包较老，但也因此符合服务器稳定的需求。  

Arch Linux：所谓的极客发行版，但是其软件包更新频率极高，所以一定能保持最新，我日常使用编译就是Arch，但是对小白来说有上手难度。  

我们去哪里下载镜像呢？找镜像源，我这里推荐清华大学镜像源（mirrors.tuna.tsinghua.edu.cn），当然你也可以选择USTC等等，如果你有需求，可以访问校园镜像联合站（mirrors.cernet.edu.cn）

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801380.png)

<p align="center">清华大学镜像源</p>

点击“获取下载链接”，左侧找到“Ubuntu”，右侧找到“24.10 (amd64, Desktop LiveDVD)”并下载（如果你需要Python2，则安装22.04版本）

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801424.png)

<p align="center">下载镜像</p>

下载结束后，我们打开VMware，点击-“创建新的虚拟机”，选择“自定义（高级）”后点击“下一步”，看到“兼容性”后继续下一步，看到“安装客户机操作系统后”选择“安装程序光盘映像文件”点击浏览并找到你刚刚下载好的ISO镜像文件，点击“打开”，这时候会提示已检测到XXX，此时“下一步”

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801511.png)

<p align="center">选择镜像文件</p>

在“简易安装信息”中，全名就是昵称，用户名是用来登录的名称，密码一定要填写，Linux的权限管理严格，因此密码一定要填写并记住。（但实际上一会安装的时候还是需要重新填写）

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801539.png)

<p align="center">用户信息</p>

“命名虚拟机”这个步骤你可以更改虚拟机名称，也可以不改，这一步我们不改并选择“下一步”，在处理器配置中，如果我们是4核8线程处理器，就要填写低于8核，防止主机崩溃，例如“处理器数量”填写2核，“内核数量”填写2，这样就是2核4线程，或者直接填写4处理器+1内核数量，或2+3等等  

下一步后，“此虚拟机的内存”中，我们填写的内存大概是本机的一半，例如我是8GB（8192MB）内存，则填写4096M，然后“下一步”，“网络”保持NAT“下一步”，“SCSI控制器”我们保持默认“下一步”，“磁盘类型”默认“下一步”，“选择磁盘”默认“创建新虚拟磁盘”“下一步”

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801578.png)

<p align="center">磁盘大小</p>

我们建议用于编译的虚拟机磁盘要大于100GB后“下一步”，“磁盘文件”默认“下一步”，最后点击“完成”  

启动虚拟机后，我们根据Ubuntu的提示进行安装即可（不安装额外软件、擦除磁盘并安装Ubuntu），我们需要耐心等待，安装时间在20-60分钟不等，取决于你的设备和网络

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801614.png)

<p align="center">Ubuntu用户设置</p>

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801658.png)

<p align="center">Ubuntu安装</p>

## 搭建环境

我们安装完毕后，启动输入密码后，会进入默认的Gnome桌面环境，你可以先适应一下这个桌面环境，找找他的桌面啦，菜单啦，设置啦，然后我们“菜单”中找到“终端”

“终端”很有用，我们在Linux下要大量使用终端进行操作，因此可以在左侧找到“终端”Logo，右键“收藏”（Pin to Dock）

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801712.png)

<p align="center">终端</p>

接下来我们需要安装一些必要的包，命令复制后一键执行：  

```text
sudo apt-get install git ccache automake flex lzop bison gperf build-essential zip curl zlib1g-dev g++-multilib libxml2-utils bzip2 libbz2-dev libbz2-1.0 libghc-bzlib-dev squashfs-tools pngcrush schedtool dpkg-dev make optipng maven libssl-dev pwgen libswitch-perl policycoreutils minicom libxml-sax-base-perl libxml-simple-perl bc libc6-dev-i386 libx11-dev lib32z-dev libgl1-mesa-dev xsltproc unzip device-tree-compiler python3 zstd libc6 binutils libc6-dev-i386 gcc g++ p7zip p7zip-full -y
```


![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801765.png)

<p align="center">安装基础软件包</p>

按提示输入密码后，即可开始安装并等待结束 
（虽然你会用得到Magic Tool，但是我在本教程中不会教授任何有关信息）  

安装结束后，我们接下来输入mkdir -p Kernel，mkdir这个软件包的目的是在指定位置创建目录，-p是他的参数，用于递归创建目录（上级目录不存在也会自动创建）  

```text
mkdir -p Kernel && mkdir -p Gcc && mkdir -p Clang
```

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801794.png)

<p align="center">文件管理器</p>

接下来我们可以短暂忘记我们还有桌面和文件管理器这回事了，并全身心投入仅终端的黑色海洋中  
  
## 获取内核源码

众所周知，你没有源码就没有内核，我应该怎么找到我的源码呢？OPPO系和小米都有官方开源的内核，但是他们的内核都有各种各样的问题（例如沟槽的驱动缺失，编译脚本过老），因此我们需要找其他大佬修改过的内核源码

我们要如何寻找呢？首先用firefox打开Github.com

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801844.png)

<p align="center">搜索</p>

在上方的搜索栏中遵守以下搜索规则：

1.手机厂商官方内核源码的作者：OPPO叫oppo-source，一加叫OneplusOSS，小米叫MiCode 

2.找到你的CPU代号（谷歌搜索例如：snapdragon 865 codename，你需要的是name->SM8250，而不是codename->kona）后，搜索关键词组合：kernel 厂商 CPU代号（kernel oneplus sm8250）

3.或者搜索你的手机代号（查询地址：storage.googleapis.com/play_public/supported_devices.html），关键词组合：kernel 手机代号（kernel polaris）

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801881.png)

<p align="center">内核搜索</p>

本次教程就以相对流行知名度较高的Realking内核为例，同时感谢Realking内核作者Rohail33的辛勤付出，该内核适配MIUI/OxygenOS/ColorOS/Android13类原生  

因为我们是一加8，因此完美符合该内核适配的机型（小米10系和一加8系）

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801929.png)

<p align="center">Realking内核</p>

我们点击“Code”，并复制所需要的git链接，并且在左面的按钮打开branches/tags，获取你需要的分支，我们是一加，因此选择op-staging  

（通常分支会展示该内核适配的Android版本，OS，或者设备这样，包括但不限于Lineage-21代表适配的是类原生LineageOS21，而11.0代表适配Android11等等，但是需要你自己分辨）

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214801956.png)

<p align="center">Git获取</p>

（Github可以使用镜像源加速，这里不展示如何使用）  
输入命令：  

```text
git clone --recursive XXX.git -b op-staging Kernel/android_kernel （自行替换XXX.git）  
```

（git clone是git软件包提供的克隆repo功能，recursive能够保证能将submodule一同git下来，-b代表选择分支，Kernel/xxx代表git repo后生成的文件夹）

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214802012.png)

<p align="center">你最好不要遇到submodule错误</p>

该作者很良心的修补了内核并放置了KernelSU-Next（抛开争议，仅作教学），因此作为教学我们就使用它提供的配置进行一次完整的编译  

当git没有报错结束后，我们就可以执行cd Kernel/android_kernel进入目录  

输入ls -al ./后，则会展示当前源码下所有文件

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214802045.png)

<p align="center">内核目录</p>

其实说到这里，你会发现有些命令我没有解释。因为很基础，而且占用大量篇幅，所以我希望各位，不管是百度还是AI，你一定要了解这些命令的作用到底是干嘛才行  

这个作者提供的build_kernel.sh，这是研究编译所用命令的很好的Shell文件，但是我们不采用该文件，我们直接输入命令编译  

编译内核，主要使用的软件包就是make，python，gcc，clang，不管是gcc还是clang其实都有独立编译C成二进制文件的能力，但是对于复杂的内核来说，我们有些时候则需要两人共同协作  
  
## 编译内核  

我们只有内核，没有gcc和clang是万万不可的，我们还需要获取gcc和clang  

通过cd ~/回到home目录  

对于Non-GKI来说，比较常用的Clang有LLVM，Proton Clang，ZyC Clang，WeebX Clang，AOSP Clang，以及类原生提供的Clang（例如LineageOS Clang 12等等）  

我们选择LLVM：github.com/llvm/llvm-project/releases/download/llvmorg-19.1.0/LLVM-19.1.0-Linux-X64.tar.xz  

（你可以使用wget XXX或curl XXX -O XXX.tar.xz来下载文件）

（tar.xz压缩包解压命令为：mkdir Clang/clang-llvm && tar -C Clang/clang-llvm/ -xvf LLVM-19.1.0-Linux-X64.tar.xz --strip-components）

而Gcc，通常使用Google GCC Android 4.9，但由于谷歌删除了旧版GCC，因此可以Git本人的Gcc备份到Gcc目录中  

地址：github.com/JackA1ltman/Google-GCC-Android-4.9

git方式同上，你需要学会举一反三  

到这里，你已经获取到Gcc/gcc-64，Gcc/gcc-32，Clang/clang-llvm了，那么我们就可以开始工作了

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214802142.png)

<p align="center">三个目录并排展示</p>

我们获取到后，cd Kernel/android_kernel，首先利用ls -al arch/arm64/configs/vendor  
（你的defconfig文件，不一定在vendor，也有可能就在arch/arm64/configs中）  

我如何找到我需要的那个defconfig文件呢？首先，原则上是你的设备代号_defconfig的命名规则，但是对于OPPO系设备来说，就是CPU代号（例如kona）-perf_defconfig，而perf代表性能  

所以我们的defconfig文件位置就是arch/arm64/configs/vendor/kona-perf_defconfig  

记住它  

然后

```text
export PATH=~/Clang/clang-llvm/bin:$PATH  
```

（临时在本终端下将对应目录下二进制文件变成直接执行的命令）  

```text
export O=out  
export ARCH=arm64  
export SUBARCH=arm64  
export CROSS_COMPILE=~/Gcc/gcc-64/bin/aarch64-linux-android-  
export CROSS_COMPILE_ARM32=~/Gcc/gcc-32/bin/arm-linux-androideabi-  
export CLANG_TRIPLE=aarch64-linux-gnu-  
```

上述步骤是在干嘛呢？第一个，设定输出目录为out，第二个设置架构为arm64，第三个设置副架构为arm64，第四个设置arm64所使用的GCC交叉编译器，第五个设置arm32（COMPAT_VSDO）所需的arm32的GCC交叉编译器，第六个设置备用Clang编译器  

执行后，开始编译第一步  

```text
make CC="ccache clang" NM=llvm-nm OBJDUMP=llvm-objdump STRIP=llvm-strip vendor/kona-perf_defconfig
```

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214802173.png)

<p align="center">执行成功后如此提示</p>

然后开始正式编译  

```text
make -j$(nproc --all) CC="ccache clang" NM=llvm-nm OBJDUMP=llvm-objdump STRIP=llvm-strip
```

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214802211.png)

<p align="center">开始编译</p>

若是很顺利，就会像截图中的那样提示，没有Error 2，并生成了Image/Image.gz/Image.gz-dtb

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214802258.png)

<p align="center">编译成功</p>

（但你通常编译内核很难会如此顺利，我会单开一期来帮助各位解决少部分问题，并和各位一起讨论）  
  
## 打包并变得可以刷入  

只是这样是无法刷入的，你需要根据你编译出来的内容来决定你，和你是否拥有该内核对应的原始Boot镜像来决定你要进行哪种方式  

"AnyKernel是一种在即使没有ramdisk的情况下也能够能够将内核刷入到任意ROM中update.zip的模板" - Koush  

那么很显然MKBOOTIMG这种方式就是需要ramdisk和kernel本身，甚至还有dtbo  

对于Realking内核来说，我们采用Anykernel3打包的方式  

Anykernel3项目：github.com/osm0sis/AnyKernel3  

其实Anykernel3所使用的anykernel.sh我们也得进行编辑，但是realking内核自带了Anykernel3（文件夹为anykernel） 

那么我们需要做的就是，利用下面的命令进行打包  

```text
mkdir -p tmp  
cp -fp $IMAGE_DIR/Image.gz tmp  
cp -fp $IMAGE_DIR/dtbo.img tmp  
cp -fp $IMAGE_DIR/dtb tmp  
cp -rp anykernel/* tmp  
cd tmp  
7za a -mx9 tmp.zip *  
cd ..  
rm *.zip  
cp -fp tmp/tmp.zip RK-OPKona.zip  
rm -rf tmp  
```

这些命令又是在干什么呢？创建目录，使用只读并保留源文件属性的方式复制文件到tmp文件夹，进入tmp目录后利用7z进行压缩zip,返回上一级目录后，删除原来存在的zip文件，并将zip从tmp目录复制出来到内核源码目录后，删除tmp文件夹  

挺复杂的吧，其实，你只需要利用例如vim或nano等工具创建一个pack.sh的文件  

在开头写上#!/usr/bin/env bash后回车，把上面的东西复制下来并保存后，就可以使用bash pack.sh来快速执行上述内容啦  

当你到了这一步，这个RK-OPKona.zip就可以通过adb sideload，或者twrp，或者orangefox的方式来输入进设备了  
  
## MKBOOTIMG的方式打包

假设我很凑巧搞到了这个boot.img，那我们就可以通过谷歌的工具MKBOOTIMG来进行boot镜像打包了  

首先，找到这个地址：android.googlesource.com/platform/system/tools/mkbootimg  
git clone XXX -b main-kernel-build-2024 mkboottools --depth=1  

注意，不要将该工具git到源码目录中，因为他同样需要调用out文件夹  

假设在~/mkboottools  

输入命令  

```text
FORMAT_MKBOOTING=$(echo `mkboottools/unpack_bootimg.py --boot_img=boot.img --format mkbootimg`)  

mkbootimg/unpack_bootimg.py --boot_img boot.img  
```

之后你会获取到out文件夹，以及该文件夹中包含的kernel，ramdisk，（和你不一定需要的dtb）  

之后我们通过cp -fr android_kernel/out/arch/arm64/boot/Image.gz out/kernel将gz压缩包复制并改名成kernel  

若你解包后还有dtb，则同样的方式从boot复制，至于你需要dtb而编译后没有生成dtb，一般就是内核Makfile中缺失了这个步骤  

（如果你的探索之心能够支撑你坚持下去，不妨拆开我的Non-GKI自动化编译项目中的示例yaml来获取生成的方式吧）

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214802312.png)

<p align="center">所需文件</p>

之后，我们就该执行最后的命令了  

```text
eval "mkbootimg/mkbootimg.py $FORMAT_MKBOOTING -o boot_realking.img"  
```

按理来说，执行后boot_realking.img就该生成了，不过你也要仔细检查这个boot的大小和原始镜像是否差不多  
  
## DLC：手动修补内核

假设你在使用rsuntk或者magic的KernelSU分支的时候，你会发现，直接编译会报错，并给了一个链接  

目前的大部分KernelSU分支都对Non-GKI设备取消了Kprobe的支持，这也就意味着，你必须手动修补后才能使用KernelSU  

我该做什么呢？首先，访问KernelSU的官网：kernelsu.org/zh_CN/guide/how-to-integrate-for-non-gki.html  

找到“手动修改内核源码”章节

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214802355.png)

<p align="center">修补</p>

（其实官网给的教程很不错，但是鉴于还是有很多人不懂得如何通过patch进行修补）  

如你所见，你需要修改大概6个文件  

这个时候我建议你使用KDE推出的kate，或者vscode等高级文本编辑工具或者开发工具来进行文件编辑，这里我使用kate  

首先你可以通过直接打开kate，或者像我一样，kate fs/exec.c

![](../assets/kernel/旧内核也吃KernelSU计划%20-%20第一期/旧内核也吃KernelSU计划%20-%20第一期-20251006214802396.png)

<p align="center">修补完整</p>

其他文件同理，总共是fs/exec.c，fs/open.c，fs/read_write.c，fs/stat.c，drivers/input/input.c，fs/devpts/inode.c  

对于4.9或更老的设备，需要参考方框下面的内容，而且你可能还需要修补selinux来让KernelSU可以正常安装卸载模块：github.com/sticpaper/android_kernel_xiaomi_msm8998-ksu/commit/646d0c8  
至于path_umount，我建议参考：github.com/tiann/KernelSU/pull/1464#issue-2190561852  
还有其他向后移植汇总：github.com/backslashxx/KernelSU/issues/4  
别忘了，defconfig文件中加入CONFIG_KSU=y  
  
## 结语

总体来说，编译内核是个比较复杂的事情，即便你已经很熟练，但是不同的内核还是会有不同的问题  

你需要一个灵活的大脑，愿意去搜索资料解决问题的心，但不要依赖AI，在个人的实践中，AI面对计算机领域的问题出错几率还是很高  

通篇读下来，你也就知道了，为什么很多人不愿意为此撰写教程，我写这些内容，花了8个小时，但还只是冰山一角

> 参考链接：
> 
>https://www.coolapk.com/feed/63739401?shareKey=MDBkMDc3M2ExNDUwNjgxYjA1OGU~&shareUid=5827990&shareFrom=com.coolapk.market_15.2.2