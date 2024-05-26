---
title: mac ssh wsl 中文乱码
date: 2024-05-26 03:50:37
tags:
---
wsl 终端 locale 查看字符编码，发现有问题，之所以不乱码是因为有 windows 的字符编码。<!--more-->

`.zshrc` 追加 
```shell
export LC_ALL=en_US.UTF-8  
export LANG=en_US.UTF-8
```
