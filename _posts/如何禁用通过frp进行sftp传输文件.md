---
title: 如何禁用通过frp进行sftp传输文件
date: 2023-10-31 21:10:46
tags:
    - Linux
declare: true
---
[如何禁用通过frp进行sftp传输文件？](https://github.com/fatedier/frp/issues/3736)<!--more-->

有时候，我们有多种连接到服务器的方式，比如通过局域网 ip，通过frp

但是有时候我们希望禁用 frp 的 sftp 服务，但却不要影响别的方式的 sftp 服务。

### 方法一

我们进行 “ssh和sftp服务分离”，frp 本质上是在转发某个指定端口的数据，如果我们分离了 ssh 和 sftp 的端口，那么 frp 自然也就无法对 sftp 进行转发了。

> 需要注意的是，ssh 和 sftp 不再共用端口之后，我们需要在客户端连接的时候指定端口，并为每一个服务端设备做端口转发，否则会连接失败。

### 方法二

本质上，frp 并没有建立 ssh 服务，而是通过本地的 ssh 服务进行转发，这一点可以通过 `echo $SSH_CONNECTION` 来查看。**显然这里的客户端和服务端 ip 都是本地的 ip，而不是 frp 的 ip。**

这就太好了，我们只需要在 `/etc/ssh/sshd_config` 进行一些 ip 规则匹配就好了。

```bash
# /etc/ssh/sshd_config
Match Address 125.223.*,10.0.0.0/24   # 局域网或校内网地址
    X11Forwarding yes

Match Address 127.0.0.1,150.158.17.239  # frp连入地址
    X11Forwarding no
    AllowTcpForwarding no
    ForceCommand /bin/sh
```

> 需要注意的是，这里学校的规则起到了决定性作用。学校禁用了校园网环境向外进行 ssh 连接，也就是说，能通过 frp 连接到服务器的，必然是在校外的 ip，必然是显示 127.0.0.1。对于这样的服务，我们拒绝其 sftp 服务。而校内的 ip 一定是 125.223 打头，我们允许其 sftp 服务。对于局域网的 ip，我们也允许其 sftp 服务。


事实上，只需要添加如下内容就可以了：
```bash
# /etc/ssh/sshd_config
Match Address 127.0.0.1  # frp连入地址
    X11Forwarding no
    AllowTcpForwarding no
    ForceCommand /bin/sh
```

> 发现出了一些问题，vscode 也无法通过 frp 进行连接了，还是要用方法一。