---
title: 路由AP模式设置
date: 2023-05-13 00:49:34
tags:
declare: true
---
对于自带AP模式的路由，可以直接设置，如果不带AP模式，就要采用桥接（手动AP）的方式。<!--more-->

首先，对于WAN口，保持断开状态不要动，LAN 口 的ip地址要手动设置，例如
- r2s: 192.168.1.1
- tp_link: 192.168.1.2
- lj: 192.168.1.3

![20230514160537](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230514160537.png)
![20230514162625](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230514162625.png)
![20230514162718](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230514162718.png)


关闭除一级路由外的所有路由的DHCP功能
![tp_dhcp](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/tp_dhcp.png)
挑选一个路由开启无线，其余无线关闭

--------------------------------
![20230514162847](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230514162847.png)

-------------------------------
> 这样配置，将会导致所有的路由器的wan口都失效。

-----------------------------
> 重启所有路由器！！！