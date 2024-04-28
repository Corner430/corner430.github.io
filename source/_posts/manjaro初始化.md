---
title: manjaro初始化
date: 2023-05-08 12:51:44
tags:
    - manjaro
    - Arch
    - Linux
declare: true
---
1. 更新系统：`sudo pacman -Syu`<!--more-->

> 报错`Failed to prepare transaction`，解决方法：`sudo pamac updata`

2. 安装输入法：`sudo pacman -S fcitx-im fcitx-configtool`
3. 更改镜像源：
```shell
sudo pacman-mirrors -c China -m rank
sudo pacman -Syyu
```
4. 安装软件：
```shell
sudo pacman -S vim
sudo pacman -S yay
sudo pacman -S debtap
sudo pacman -S docker
yay -S google-chrome
yay  -S  wemeet
```

-----------------------------------
添加清华大学的软件源：
1. 打开/etc/pacman.conf文件，并将以下内容添加到文件末尾：
```shell
[archlinuxcn]
SigLevel = Optional TrustedOnly
# SigLevel = Never
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

2. 打开终端，并使用以下命令导入清华大学的公钥：`sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring`
3. 更新系统的软件包缓存，以获取从清华大学的软件源中提供的软件包信息：`sudo pacman -Syyu`

> 请注意，清华大学的软件源和其他第三方软件源可能包含不受信任的软件包。在使用这些源时，请务必小心谨慎，只从可信的来源安装软件包。

-------------------------------
[配置bash](https://corner430.github.io/2023/04/03/Configure-oh-my-zsh/#more)

-------------------------------
# 桌面美化
[如何将 Linux 美化成 MacOS 风格？](https://youtu.be/fzwnHKdpbWM)
[Manjaro-Kde美化 最后一次发桌面美化](https://www.bilibili.com/video/BV1pJ411N75i/?share_source=copy_web&vd_source=a7ae9163cb2cd121bfd86ea1f4ecd2ef)
[[Linux]4分钟完成manjaro系统美化](https://www.bilibili.com/video/BV1yE411J7Z8/?share_source=copy_web&vd_source=a7ae9163cb2cd121bfd86ea1f4ecd2ef)
[你的Manjaro能有多漂亮和有效率？简单美化教程，deepin又被我干掉了](https://www.bilibili.com/video/BV1Lt41167bp/?share_source=copy_web&vd_source=a7ae9163cb2cd121bfd86ea1f4ecd2ef)
[漂亮的Linux桌面环境，这才是电脑系统应该有的样子](https://www.bilibili.com/video/BV1vt411E7GP/?share_source=copy_web&vd_source=a7ae9163cb2cd121bfd86ea1f4ecd2ef)

---------------------------------
# pacman使用
Pacman 是 Arch Linux 中的一个命令行软件包管理器，可以用于安装、更新、卸载软件包，以及管理系统的依赖关系。下面是一些常用的 Pacman 命令：
- 更新软件包数据库：`sudo pacman -Syy`
- 安装软件包：`sudo pacman -S package_name`
- 更新已安装的软件包：`sudo pacman -Syu`
- 搜索软件包：`pacman -Ss package_name`
- 显示已安装的软件包：`pacman -Q`
- 显示已安装软件包的详细信息：`pacman -Qi package_name`
- 卸载软件包：`sudo pacman -R package_name`
- 卸载软件包及其依赖项：`sudo pacman -Rs package_name`
- 清理已下载的软件包：`sudo pacman -Sc`
- 清理已下载的软件包和旧版本的软件包：`sudo pacman -Scc`
> 在这些命令中，`sudo` 表示以管理员权限运行命令，`-S` 表示安装软件包，`-Ss` 表示搜索软件包，`-Q` 表示列出已安装的软件包，`-R` 表示卸载软件包，`-Syu` 表示更新已安装的软件包等等。


------------------------------------
# 常用软件安装
- yay是一个用Go语言写的一个AUR助手，有些时候官方仓库没有你想要的软件，就需要通过yay来安装：`sudo pacman -S yay`
- [Manjaro-KDE安装配置全攻略 - ayamir的文章 - 知乎](https://zhuanlan.zhihu.com/p/114296129)

----------------------------
安装向日葵
```shell
yay -Sy sunloginclient
sudo systemctl start runsunloginclient.service
sudo systemctl enable runsunloginclient.service
```

----------------------------
deb转换为pacman
```shell
sudo debtap -u
sudo debtap -q *.deb
sudo pacman -U *.pkg.tar.xz
```

----------------------------
安装 N 卡驱动
```shell
sudo mhwd -a pci nonfree 0300   # 官方的方法，但是不好用
mhwd-kernel -li​             # 查看内核
​sudo pacman -S nvidia​​    # 根据上述内核安装驱动
# 安装 390xx 还是 470xx 还是 510xx 可以去图形化硬件信息，看看现在推荐的是哪个版本
# 重启 就可以使用 nvidia-smi 查看显卡信息
```
[官网方案](https://wiki.manjaro.org/index.php/Configure_Graphics_Cards/zh-cn)

---------------------------------
安装 wechat 参考 [deepin-wine-wechat-arch](https://github.com/vufa/deepin-wine-wechat-arch)