---
title: hexo d报错问题
date: 2023-04-23 19:51:52
tags:
declare: true
---
`hexo d` 和 `ssh -T git@github.com`报错
```shell
kex_exchange_identification: Connection closed by remote host Connection closed by 20.205.243.166 port 22
```
<!--more-->
原因，代理冲突，22端口冲突。
解决方案：
```shell
vim ~/.ssh/config
# 添加如下内容
HostName ssh.github.com
Port 443

# 或者添加如下内容
Host github.com
HostName ssh.github.com
User git
Port 443
```
------------------------------------------------
> 方式一是为了直接连接到 GitHub 的 SSH 服务器（ssh.github.com）而使用的。**它将覆盖默认的 SSH 配置**，并指定了 GitHub 的主机名和端口号。这种方式可以通过简单地运行 ssh git@github.com 来连接到 GitHub。

> 方式二使用了 SSH 的别名（Alias）功能，它定义了一个名为 github.com 的主机别名，实际连接的是 ssh.github.com。这样做的好处是可以通过运行 ssh github.com 来连接到 GitHub，而不需要指定完整的主机名和用户。它还指定了连接时使用的用户名为 git（应该改成自己的用户名），并将端口号设置为 443。

- 总结：
方式一是直接修改默认的 SSH 配置，可以快速连接到 GitHub，但在连接其他 SSH 主机时可能需要手动指定其他参数。
方式二通过别名的方式，提供了一个更简洁的连接方式，并且可以与其他 SSH 主机的配置相分离，使得管理更加灵活。
> 推荐使用方式二

-----------------------------------------------
- 使用https访问

参考
['ssh -vT git@github.com' results in error "kex_exchange_identification: Connection closed by remote host"](https://stackoverflow.com/questions/59692874/ssh-vt-gitgithub-com-results-in-error-kex-exchange-identification-connecti)

[Github SSH 协议拉取代码报错 Connection closed by *.*.*.* port 22](https://blog.csdn.net/Eric_q8666/article/details/127179501)

[Github ssh的连接问题](https://segmentfault.com/a/1190000041889603)