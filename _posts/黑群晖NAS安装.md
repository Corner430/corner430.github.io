---
title: 黑群晖NAS安装
date: 2023-05-22 00:29:46
tags:
declare: true
---
[img下载](https://xpenology.club/downloads/)
[pat下载](https://www.synology.com/en-global/support/download/DS918+?version=7.1#system)
[Win32 Disk Imager下载](https://sourceforge.net/projects/win32diskimager/)
[芯片无忧ChipEasy](https://www.upan.cc/tools/test/ChipEasy.html)
[芯片无忧ChipEasy英文版](https://www.upan.cc/tools/test/ChipEasy_EN.html)
<!--more-->

# 方案一
通过[arpl](https://github.com/fbelavenuto/arpl)进行引导，出现了两个问题
1. 分配ip不显示，会显示waiting ip ......... error
解决方案，ifconfig自行查看ip，之后浏览器通过ip:7681进入即可
2. build loader时候报错，参照[Error: zImage not Patched #27](https://github.com/fbelavenuto/arpl/issues/27)
解决方案，无

# 方案二
准备如下工具
![20230522003434](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230522003434.png)
1. 使用Win32DiskImager将synoboot.img镜像写入系统
![20230522003613](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230522003613.png)
2. 通过芯片无忧查看U盘的pid 和vid
![20230522003944](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230522003944.png)
> 注意这里的0x不要删除
3. 使用DiskGenius查看引导优盘里面的文件
![20230522004235](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230522004235.png)
> 需要复制到桌面修改，之后再拖回来
4. 浏览器查找黑群晖
https://find.synology.com/
5. 选择手动安装
![20230522004422](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230522004422.png)
6. 创建账号和密码
![20230522010539](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230522010539.png)
7. 黑群啊黑群
![20230522010659](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230522010659.png)
![20230522010744](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230522010744.png)
8. 分享网络，打上勾，就可以通过https://find.synology.com/找到设备。

[保姆级黑群晖安装教程，小白也能轻松安装！！！！](https://zhuanlan.zhihu.com/p/515187738)

第三方套件，[我不是矿神](https://imnks.com/)