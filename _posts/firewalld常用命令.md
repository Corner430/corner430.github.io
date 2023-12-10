---
title: firewalld常用命令
date: 2023-12-10 16:55:16
tags:
    - Linux
---
Firewalld是Linux系统上用于管理iptables规则的工具，它提供了一个动态的、易于使用的防火墙管理工具。以下是一些firewalld的常用命令：<!--more-->

1. **启动/停止/重启firewalld服务：**
   - 启动：`sudo systemctl start firewalld`
   - 停止：`sudo systemctl stop firewalld`
   - 重启：`sudo systemctl restart firewalld`

2. **设置firewalld开机启动：**
   - `sudo systemctl enable firewalld`

3. **查看firewalld状态：**
   - `sudo systemctl status firewalld`

4. **查看所有防火墙规则：**
   - `sudo firewall-cmd --list-all`

5. **查看特定区域的规则：**
   - `sudo firewall-cmd --zone=public --list-all`

6. **打开/关闭防火墙：**
   - 打开：`sudo firewall-cmd --state`
   - 关闭：`sudo systemctl stop firewalld`

7. **添加/移除服务：**
   - 添加：`sudo firewall-cmd --add-service=service_name`
   - 移除：`sudo firewall-cmd --remove-service=service_name`

8. **添加/移除端口：**
   - 添加：`sudo firewall-cmd --add-port=port_number/tcp`
   - 移除：`sudo firewall-cmd --remove-port=port_number/tcp`

9. **指定接口的区域：**
   - `sudo firewall-cmd --zone=public --change-interface=eth0`

10. **允许/拒绝/移除IP地址：**
    - 允许：`sudo firewall-cmd --add-source=ip_address`
    - 拒绝：`sudo firewall-cmd --add-rich-rule='rule family="ipv4" source address="ip_address" reject'`
    - 移除：`sudo firewall-cmd --remove-source=ip_address`

11. **设置默认区域：**
    - `sudo firewall-cmd --set-default-zone=public`
