---
title: Ubuntu安全防护
date: 2023-05-07 20:27:38
tags:
declare: true
---
ClamAV是一个开源的杀毒软件，可以通过命令行进行使用。<!--more-->

1. 安装ClamAV：
```shell
sudo apt-get update
sudo apt-get install clamav
```
2. 查看是否运行：`sudo systemctl status clamav-freshclam.service`
3. 停止服务：`sudo systemctl stop clamav-freshclam.service`
4. 更新ClamAV病毒库文件：`sudo freshclam`
5. 对系统进行扫描：`sudo clamscan -r /`

--------------------------
- 修改root用户密码：`sudo passwd root`
- 查看当前在Linux系统中已创建的所有用户和用户组：
```shell
cat /etc/passwd    # 查看用户
cat /etc/group     # 查看用户组
```
- 删除用户：`sudo userdel <用户名>`
> 请注意，如果该用户有属于他的文件或目录，这些文件和目录将不会自动删除。为了删除这些文件和目录，请使用以下命令：
`sudo userdel -r <用户名>`
- 删除用户组：`sudo groupdel <用户组名>`
> 请注意，如果该用户组有属于它的文件或目录，这些文件和目录将不会自动删除。为了删除这些文件和目录，请先将它们的所属组修改为其他组或新建一个用户组并将其所属组修改为新的用户组，然后再删除旧用户组。

-----------------------------------------------
使用ufw防火墙
1. 检查防火墙状态：`sudo ufw status`
2. 启用防火墙：`sudo ufw enable`，这将启用防火墙，并默认情况下阻止所有传入的连接。
3. 允许特定端口的传入连接：`sudo ufw allow <port>/<protocol>`
   - 例如，要允许HTTP传入连接，可以使用以下命令：`sudo ufw allow 80/tcp`，这将允许TCP协议下的端口80的传入连接。
4. 允许特定IP地址的传入连接：`sudo ufw allow from <ip_address>`
   - 例如，要允许IP地址为192.168.0.100的主机的传入连接，可以使用以下命令：`sudo ufw allow from 192.168.0.100`
5. 查看当前防火墙规则：`sudo ufw status`，这将显示当前的防火墙规则，包括允许和拒绝的连接。
6. 禁用防火墙：`sudo ufw disable`，这将禁用防火墙，并允许所有传入的连接。