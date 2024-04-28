---
title: Building a picture bed through github and PicGo
date: 2023-04-05 17:24:17
tags:
    - Picture bed
    - PicGo
    - 图床
    - Linux
    - Ubuntu
declare: true
---
1. Create a new repository on github, select **setting->Developer settings->Personal access tokens (classic)** to create a tokens.<!--more-->
2. Download the **PicGo** plugin for vscode, find **Picgo>Pic Bed:Current** in **setting**, fill in the following content:
<!-- ![PicGo_setting](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/PicGo_setting.png) -->

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/PicGo_setting.png" width="40%" height="40%">

**https://cdn.jsdelivr.net/gh/** for CDN acceleration.

3. Remaining problems: wsl cannot be used normally.

------------------------------------

Ubuntu 中报错 `xclip not found,Please install xclip before run picgo`，安装 `xclip`，之后使用**Ubuntu系统自带的剪贴板**就可以了。
