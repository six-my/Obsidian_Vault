---
title: CPU架构关系
published: 2025-05-05
description: ""
image: ""
tags:
  - cpu
category: ""
draft: false
lang: ""
---

### x86、x86-64、AMD64 和 x64

- x86：指 32 位指令集/架构，也称为IA-32，由英特尔开发，AMD 也使用。任何 x86 架构，但现在仅指 32 位。也称为 i386 或 i686
    
- AMD64：你说得对，英特尔最早推出的是 64 位处理器……但恐怕剩下的就有点……不对劲了。英特尔的[64 位处理器](https://en.wikipedia.org/wiki/IA-64)与 x86 产品线不兼容，几乎可以说是“新处理器”。它几乎失败了 ;)。AMD 推出了一种与 x86 兼容的 64 位设计，而这种设计如今被广泛使用……甚至英特尔也采用了。这就是为什么它经常被称为 AMD64……这是最初的名字。
    
- X86-64：AMD 和英特尔 AMD64 平台实现的通用术语...因此或多或少是同义词
    
- X64：这里需要注意一点……有时是 AMD64/X86-64 的缩写。但有时也用于 AMD64 处理器的特殊模式，该模式在 32 位地址空间中运行程序，但可以访问仅适用于 64 位应用程序的所有 CPU 指令。（但后者正在逐渐消失，因此很可能被用作 AMD64/X86-64 的同义词）

> 参考链接：
> 
>https://www.reddit.com/r/linuxquestions/comments/ra4vjj/difference_between_x86_x8664_amd64_and_x64/

2025-5-5