---
title: Ubuntu新用户创建初始化
date: 2023-05-09 19:26:51
tags:
    - Ubuntu
declare: true
---
## 在Ubuntu中创建新用户并允许其使用sudo命令，您可以按照以下步骤进行操作：
- 以root用户身份登录系统，或使用已经拥有sudo权限的用户账户登录。<!--more-->
- 执行以下命令创建新用户，其中"newuser"是您要创建的新用户名：`sudo adduser newuser`
- 您需要为新用户设置密码，以及其他相关的信息，例如全名、电话等。按照提示输入相关信息即可。
- 接下来，将新用户添加到sudo用户组中，使其拥有sudo权限：`sudo usermod -aG sudo newuser`
- 现在，新用户应该已经可以使用sudo命令了。您可以切换到新用户身份进行测试：
```shell
su - newuser
sudo ls
```
> 如果没有出现权限不足的提示，说明新用户已经拥有sudo权限。
- 配置Shell环境：默认情况下，新用户的Shell环境是Bash。您可以根据需要修改其Shell环境。例如，如果您希望将其Shell环境更改为Zsh，可以使用以下命令：
`sudo chsh -s /usr/bin/zsh username`
- 配置SSH访问：如果您希望新用户可以通过SSH访问系统，您需要为其生成SSH密钥对，并将公钥添加到系统的authorized_keys文件中。您可以使用以下命令为新用户生成SSH密钥对：`sudo -u username ssh-keygen -t rsa`
- 将公钥添加到系统的authorized_keys文件中：`sudo -u username cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys`