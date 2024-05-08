---
title: ssh设置禁用密码登录，仅密钥验证
date: 2024-05-08 14:13:50
tags:
---

```shell
# 修改配置文件
vim /etc/ssh/sshd_config

# 内容如下
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no

# 重启服务
systemctl restart sshd
```
<!--more-->

之后导入公钥到服务器`~/.ssh/authroized_keys`即可