---
title: Ubuntu NAT
date: 2023-04-03 18:17:31
tags:
declare: true
---

### NAT
To allow Ubuntu in the LAN to access the Internet, you need to set Ubuntu as a gateway and enable Network Address Translation (NAT).
<!--more-->
- Make sure Ubuntu is connected to the Internet and can access the Internet.

- Enable IP forwarding:
`sudo sysctl net.ipv4.ip_forward=1`

- On the other four Ubuntu computers, set the default gateway to the IP address of the computer that can access the Internet. You can enter the following command in Terminal to set the default gateway:
`sudo route add default gw <gateway_IP_address>`
Make sure to replace **<gateway_IP_address>** with the IP address of an Ubuntu computer with Internet access.

In recent versions of Ubuntu, the `route` command has been replaced by the `ip route` command. Therefore, you can set the default gateway to the IP address of a computer with Internet access using the following command:
`sudo ip route add default via <gateway_IP_address>`

- Add DNS resolution configuration in /etc/resolv.conf
```bash
nameserver 114.114.114.114
nameserver 8.8.8.8
nameserver 8.8.4.4
```

After completing these steps, your Ubuntu will be configured as a gateway with Network Address Translation (NAT) enabled. This way, other computers can connect to the internet through Ubuntu.

Static ip configuration: `/etc/netplan/00-installer-config.yaml`
The configuration takes effect: `sudo netplan apply`


---------------------

Set mirror source

- Backup
`sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup`

- Open the **/etc/apt/sources.list** file.

- Replace with the following
```shell
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse 
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse 
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse 
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse 
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse 
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse 
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse 
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse 
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse 
```

- Update
`sudo apt update`

- Solve vim problems
```shell
sudo apt-get remove vim-common
sudo apt-get install vim
```
