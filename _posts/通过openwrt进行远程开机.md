---
title: 通过openwrt进行远程开机
date: 2023-05-18 08:35:19
tags:
declare: true
---
1. 防火墙->端口转发，开启openwrt的外网访问。<!--more-->
2. 网络适配器->属性->配置
![20230518084317](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230518084317.png)
3. 开启如下服务，参照google
![20230518084506](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230518084506.png)
4. 设置主板允许WOL（wake on lan）
5. openwrt网络唤醒
![20230518084609](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230518084609.png)