---
title: kernel
published: 2025-04-15
description: ""
image: ""
tags:
  - kernel
category: ""
draft: false
lang: ""
---

repo init -u https://github.com/OnePlusOSS/kernel_manifest -b oneplus/sm8550 -m oneplus_ace3_v.xml

repo sync -j$(nproc)

./kernel_platform/oplus/build/oplus_build_kernel.sh kalama gki