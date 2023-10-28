---
title: Windows安装Docker并集成到wsl
date: 2023-04-12 09:18:50
tags:
---
- 在wsl安装docker：
```shell
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh --mirror Aliyun    # 其中--mirror Aliyun为可选
```
<!--more-->

- 弹窗如下：
![98009494d24e4163004a06f208c1aed](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/98009494d24e4163004a06f208c1aed.png)

- [WSL 2 上的 Docker 远程容器入门](https://learn.microsoft.com/zh-cn/windows/wsl/tutorials/wsl-containers)

> Desktop版本占用空间过大，但可以应用到所有的系统，包括windows系统。不建议使用。