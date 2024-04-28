---
title: Ubuntu挂载NAS网络驱动器
date: 2023-07-23 14:13:38
tags:
    - NAS
    - Ubuntu
delcare: true
---
校外向校内服务器传输数据极为不便，唯有NAS可完美解决问题，并实现**储算分离**<!--more-->
```shell
sudo apt update
sudo apt install smbclient
sudo apt install samba

# 列出 \\nas地址下的文件夹，其中nas为nas所在地址
smbclient -L \\nas -U <username>
```
然后按提示输入账号对应的密码

将//nas/main文件夹映射到本地文件夹/mnt/nas
`sudo mount -t cifs -o username=<username>,password=<password>,vers=1.0 //nas/main /mnt/nas`

-----------------------------------
针对实验室服务器，改成开机自动挂载，并为每个人分配**限额文件夹或者单独的限额账户**即可
问题在于这样仍然在内网，如果同步外网数据呢？此时就要用到**Cloud Sync**，便可不限速不限额的完美解决一切问题
甚至单独搭建一个git server都是没有问题的

----------------------
[Ubuntu 挂载 NAS 网络驱动器](https://yangruoqi.site/ubuntu-nas/)