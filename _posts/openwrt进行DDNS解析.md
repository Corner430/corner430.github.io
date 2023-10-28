---
title: openwrt进行DDNS解析
date: 2023-05-19 13:03:15
tags:
declare: true
---
1. 在cloudflare添加一条A记录<!--more-->
![20230519130833](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230519130833.png)

2. 获取cloudflare的API key
[API Tokens](https://dash.cloudflare.com/profile/api-tokens)

复制这串密钥
![20230519131101](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230519131101.png)

3. openwrt设置DDNS
![20230519132624](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230519132624.png)

> 这里如果有问题，把route都改成www，包括cloudflare那里的A记录也要更改

4. 使用url转发来避免使用域名加端口号进行访问
- 设置页面规则
![20230519135334](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230519135334.png)
- 添加一条新记录，要和页面规则的相匹配
![20230519135539](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230519135539.png)

[【278】如何随时随地访问家中的软路由和其他设备？远程访问端口转发保姆级教程丨CloudFlare设置动态DNS 域名解析 URL转发](https://youtu.be/Y0HGp94eHGU?t=889)