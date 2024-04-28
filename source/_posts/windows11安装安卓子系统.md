---
title: windows11安装安卓子系统
date: 2023-05-18 11:55:37
tags:
    - windows
declare: true
---
1. 将Hyper-v和虚拟机平台打上勾<!--more-->
![20230518115729](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230518115729.png)

2. Microsoft store 更新到最新版本（在美区状态下）

3. 下载安卓子系统的安装软件
[下载界面](https://store.rg-adguard.net/)
![20230518120021](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230518120021.png)
其中，提取码：9p3395vx91nr

下载最后一个
![20230518120144](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230518120144.png)

4. 双击打开进行安装
![20230518120825](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230518120825.png)

> 如果无法打开，需要采用如下操作
- 以管理员身份运行powershell
- 进入到文件所在目录
- 运行安装命令
`add-Appxpackage "MicrosoftCorporationII.WindowsSubsystemForAndroid_1.7.32815.0_neutral_~_8wekyb3d8bbwe_2.Msixbundle"`

5. 打开开发者模式
![20230518121212](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230518121212.png)

6. 下载[adb 的命令安装包 platform-tools_r31.0.3-windows](https://www.freedidi.com/wp-content/uploads/2021/10/platform-tools_r31.0.3-windows.zip)
> 解压之后，将里面的**所有文件**复制到C盘的windows文件夹下

7. 此时在终端运行以下命令
```shell
adb version     # 查看版本号
adb connect 127.0.0.1:58526 # 连接到安卓子系统，需要查看对应的端口号
adb devices     # 查看当前连接设备
adb install .\tiktok.apk    # 安装自己下载的apk文件
```
![20230518122730](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230518122730.png)