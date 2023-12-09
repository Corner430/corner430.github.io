---
title: 小新pro16 安装 Ubuntu
date: 2023-12-09 21:26:42
tags:
    - Ubuntu
---
1. 开机黑屏<!--more-->
显卡驱动问题，安装显卡驱动。
点击 `ctrl+alt+F2` 进入命令行模式，输入用户名和密码，然后执行以下命令：

```bash
ubuntu-drivers devices  #查看系统推荐的Nvidia版本，带有 recommended 标签的是推荐版本
sudo apt install nvidia-driver-xxx  #安装Nvidia驱动
sudo reboot  #重启
```

2. 检测到窗口系统采用wayland协议,腾讯会议暂不兼容,程序即将退出!

```bash
sudo vim /etc/gdm3/custom.conf
#WaylandEnable=false #取消注释
sudo service gdm restart
```

3. 输入法
先在区域和语言中设置中文，需要点击**管理已安装的语言**，把缺少的语言包安装上。

注意到这时候的**键盘输入法系统**是`ibus`，`ibus`和 **智能拼音** 相互匹配。

设置中点击**键盘**，在**输入源**中添加**中文（智能拼音）**，重启后生效。

4. 可执行软件装在 `/usr/local/bin` 下
