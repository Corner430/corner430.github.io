---
title: Ubuntu设置网络唤醒
date: 2023-05-24 18:55:57
tags:
    - Ubuntu
declare: true
---
1. 确保计算机的BIOS设置中已启用唤醒功能。<!--more-->
2. 安装ethtool工具：
```shell
sudo apt install ethtool
```
3. 确认网卡支持唤醒功能。在终端中运行以下命令来检查：
```shell
ifconfig   # 查看网络接口的名称
sudo ethtool <interface>

# 其中，<interface>是网络接口的名称，如eth0或enp2s0。在ethtool输出中，查找Wake-on:的行，如果它的值为d表示该接口支持唤醒功能。
```

4. 激活唤醒功能。在终端中运行以下命令：
```shell
sudo ethtool -s <interface> wol g

# 其中，<interface>是网络接口的名称，如eth0或enp2s0。此命令将启用接口的唤醒功能，并将其设置为通过网络唤醒（magicpacket）来唤醒计算机。
```

5. 确认唤醒功能已经激活。在终端中运行以下命令：
```shell
sudo ethtool <interface>

# 再次查找Wake-on:的行，如果它的值为g表示该接口已经成功启用唤醒功能。
```

-----------------------------------------------------------
- 创建一个开机自启动的服务来执行`sudo ethtool -s enp2s0 wol g`命令。
1. 创建一个新的服务单元文件。在终端中输入以下命令创建一个新的服务单元文件：`sudo vim /etc/systemd/system/wol.service`
2. 在打开的文件中，输入以下内容：
```bash
[Unit]
Description=Wake-on-LAN Configuration
After=network.target

[Service]
ExecStart=/usr/sbin/ethtool -s enp2s0 wol g

[Install]
WantedBy=default.target
```
> 请确保将enp2s0替换为要启用网络唤醒的网络接口的正确名称。

3. 启用服务并重新启动系统测试即可