---
title: Linux通过davfs2挂载WebDav网盘
date: 2023-12-10 12:43:30
tags:
    - Linux
    - WebDav
---
```bash
sudo apt install davfs2

mkdir /mnt/alist/
sudo mount.davfs http://locaohost:8080/dav/ /mnt/alist/
# 输入账号密码并手动连接
```

---------------
1. 编辑 `/etc/davfs2/davfs2.conf`，找到其中的 `use_lock` 取消注释，并修改值为 `0`。（用于修正复制剪切文件错误）
2. 修改 `/etc/davfs2/secrets`，在末尾添加 `http://localhost:8080/dav/ admin password`
3. 编辑 `/etc/fstab`，在最后一行添加以下内容 `http://localhost:8080/dav/ /mnt/alist/ davfs defaults 0 0`
4. 映射文件夹 `ln -s /mnt/alist/ /home/alist/`