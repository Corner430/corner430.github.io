---
title: WSL modify the default installation directory to another disk
date: 2023-04-03 16:13:30
tags:
---
- Check out the WSL distribution
Enter the following command in Windows PowerShell:
`wsl -l -v`<!--more-->

- Export the distribution version as a tar file to the D drive
`wsl --export Ubuntu-20.04 d:\wsl-ubuntu20.04.tar`

- Unregister the current distribution
`wsl --unregister Ubuntu-20.04`

- Re-import and install WSL in d:\wsl-ubuntu20.04
`wsl --import Ubuntu-20.04 d:\wsl-ubuntu20.04 d:\wsl-ubuntu20.04.tar --version 2`

- Set the default login user as the user name during installation
`ubuntu2004 config --default-user corner`

- Delete tar file (optional)
`del d:\wsl-ubuntu20.04.tar`

- Reference
[WSL修改默认安装目录到其他盘](https://www.cnblogs.com/tl542475736/p/14855863.html) 
[WSL2是否可以安装到非C盘的其它位置？](https://answers.microsoft.com/zh-hans/windows/forum/all/wsl2%E6%98%AF%E5%90%A6%E5%8F%AF%E4%BB%A5%E5%AE%89/608a35fd-a8d6-40cc-854d-65bead8ef49d)

