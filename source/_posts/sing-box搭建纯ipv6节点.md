---
title: sing-box搭建纯ipv6节点
date: 2024-05-20 15:35:13
tags:
    - 科学上网
---

ipv6节点搭建<!--more-->

```shell
# 查看内核版本
uname -a

apt update && apt upgrade

# 安装 wireguard 内核模块
apt install wireguard-dkms -y
# 亲测这一条并不好用，但会帮你扫清许多依赖的问题，转到手动编译
# 下载并解压wireguard-tools源码并解压
wget https://git.zx2c4.com/wireguard-tools/snapshot/wireguard-tools-1.0.20210914.tar.xz
tar -xf wireguard-tools-1.0.20210914.tar.xz
# 手动编译其源码
cd wireguard-tools-1.0.20210914/src
make && make install
# 验证是否安装成功
wg -h
wg-quick -h

# 重启vps
reboot

touch /etc/wireguard/warp.conf
wget -N https://gitlab.com/fscarmen/warp/-/raw/main/menu.sh && bash menu.sh [option] [lisence/url/token]
# 此时菜单中选择转为双栈网络

bash <(wget -qO- https://raw.githubusercontent.com/fscarmen/sing-box/main/sing-box.sh)
# 一路傻瓜操作
```