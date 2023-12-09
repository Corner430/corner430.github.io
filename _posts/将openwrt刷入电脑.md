---
title: 将openwrt刷入电脑
date: 2023-05-30 23:07:44
tags:
  - 路由器
  - openwrt
declare: true
---
> x86 设备刷openwrt

3865U软路由太贵，
单臂旁路由推荐斐讯N1，不到一百块钱。
N270小主机，两个千兆的网口，闲鱼几十块钱。<!--more-->
[esirPG固件](https://drive.google.com/drive/folders/1dqNUrMf9n7i3y1aSh68U5Yf44WQ3KCuh)
![20230531002538](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230531002538.png)
esirPG固件的默认信息：**路由器初始IP: 192.168.5.1  初始用户名: root 密码: password**
零度推荐的固件，软路由默认登录密码为 netflixcn.com

# 直接刷Openwrt 至 U 盘
- 下载[balena](https://etcher.balena.io/)
- 将镜像刷入U盘，U盘启动。
> 如果有 硬盘 的转USB的转接设备，就可以直接按照U盘的方式刷入硬盘，之后从这个硬盘启动即可。

# PE刷Openwrt
- 下载PE系统，[IT天空](https://www.itsk.com/) 或者wepe 或者 老毛桃
![20230530233054](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230530233054.png)
- 将PE系统刷入U盘
- 将Diskimage.exe 和 openwrt的img镜像文件拖入U盘
- 启动PE
- 删除硬盘原有的一切分区
- 将镜像写入硬盘

<!-- ![20230530234029](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230530234029.png) -->

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230530234029.png" width="70%" height="70%">

- OK

# Openwrt刷Openwrt
[旧电脑刷路由系统Openwrt的第三种种方法](https://youtu.be/zbxTvRO9SKY?t=348)

# openwrt镜像
[X86_64多功能版本](https://drive.google.com/drive/folders/1pdJuzpwuhdq6vnN0Ovtb5r-JB7Z5bw38)

-------------------------------------------
# 设置为单臂旁路由
- 回车确认，进入到软路由的命令行界面
- 修改网络配置文件
```shell
vi /etc/config/network
```
- 给它一个静态的ip
<!-- ![20230531000211](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230531000211.png) -->
<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230531000211.png" width="60%" height="60%">
- 通过同局域网的设备，在浏览器进入软路由界面
- 设置Lan口
  - 网关改为主路由ip
  - DNS服务器同上
  - 关闭DHCP服务器
- 配置PassWall
- 将需要上网的设备的**网关和DNS服务器**改为软路由的ip地址，或者在主路由中统一设置。



---------------------------------------------
# 参考
- [网关代理](https://corner430.github.io/2023/05/26/%E7%BD%91%E5%85%B3%E4%BB%A3%E7%90%86/#more)
- [旧电脑刷路由系统Openwrt的几种方法，总有一种适合你！](https://www.youtube.com/watch?v=zbxTvRO9SKY&t=2s&ab_channel=%E9%9F%A9%E9%A3%8ETalk)
- [软路由安装教程，闲置笔记本设置旁路由，双臂路由！科学上网更快更稳定！2020 | 零度解说](https://www.youtube.com/watch?v=nEU4hbZYj6c&t=783s&ab_channel=%E9%9B%B6%E5%BA%A6%E8%A7%A3%E8%AF%B4)
- [免费安装软路由，一台闲置的旧笔记本就够了！科学上网更快更稳定！](https://www.freedidi.com/741.html)
- [零成本体验软路由/闲置笔记本变身OpenWRT软路由_轻松上Youtuber_快来体验看看软路由适不适合你](https://www.youtube.com/watch?v=VN_V9CzG6AM&t=817s&ab_channel=%E6%88%91%E6%98%AF%E9%98%BF%E7%9A%AE%E5%95%8A)
- [恩山大神OpenWrt固件功能简介&&Google Drive网盘的使用指南](https://www.youtube.com/watch?v=e4IAZdAZ60w&ab_channel=eSirPlayGround)