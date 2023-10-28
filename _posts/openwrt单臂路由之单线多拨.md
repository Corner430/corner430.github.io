---
title: openwrt单臂路由之单线多拨
date: 2023-06-01 14:48:16
tags:
declare: true
---
# 1. 单臂路由拨号上网设置
## 1.1 接线
- 光猫和路由器是lan-lan
- 路由器和单臂路由是lan-lan<!--more-->

## 1.2 单臂路由拨号
- 修改lan口地址，**取消lan口的桥接设置，**，开启DHCP功能。其余不变
- 手动创建wan口，接口和lan口的物理接口复用。设置PPPOE拨号。
## 1.3 路由器和上网设备设置
- 关闭路由器的DHCP功能，并手动设置路由器为AP模式
- 设置上网的网关和DNS都为单臂路由的地址

## 1.4 修改单臂路由的防火墙的区域设置，使得LAN-WAN之间的数据流通

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230601144607.png" width="70%" height="70%">

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230601144627.png" width="70%" height="70%">

## 1.5 设置单臂路由中DHCP/DNS中设置DNS转发

-------------------------------------
# 2. 单线多拨
> 经测试，如果是多拨的账号不同，是可以拨通的，如果采用同一账号，就拨不通
> 如果拨不通，按照如下效果图中的顺序，自上而下的重新连接各个网络接口

- 拨通的效果图如下：

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230601221149.png" width="70%" height="70%">

设置方式如下：

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230601221258.png" width="70%" height="70%">

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230601221324.png" width="70%" height="70%">

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230601221348.png" width="70%" height="70%">

> 重点注意虚拟wan口的防火墙设置和防火墙的区域设置

## 2.1 单线多拨（双账号）测速图
> 测速基于无线，不准确

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230601221529.png" width="50%" height="50%">

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230601221536.png" width="50%" height="50%">

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230601221543.png" width="50%" height="50%">

## 2.2 关闭多拨测速图

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230601221624.png" width="50%" height="50%">

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230601221629.png" width="50%" height="50%">

<img src="https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230601221634.png" width="50%" height="50%">

# 3. 结论
或许是由于实验室网络接口被计算机网络中心限速的原因，并没有什么宽带叠加的效果，反而会更加不稳定。
**一言以蔽之：没用**

# 4. References
[单臂路由|N1盒子(OpenWRT)单线多拨实现网速叠加|Owen带你一起玩](https://youtu.be/0uNntGhMLvI)

