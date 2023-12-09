---
title: How do I use clash proxy wsl in windows?
date: 2023-04-05 19:42:16
tags:
  - clash
  - wsl
  - 科学上网
toc: 1
declare: true
---
### 修改允许应用或功能通过 Windows Defender防火墙

`Control Panel > System and Security > Windows Defender Firewall > Allow an app or feature through Windows Defender Firewall. `
<!--more-->
1. 找是否有clash-win64.exe的规则配置，注意不是 Clash For Windows ，CFW本身只是clash的一个前端，在启动CFW的时候有概率防火墙只添加CFW本身，而不添加作为核心的clash的防火墙规则，这个时候则需要我们手动修改。
2. 如果已经有了clash-win64.exe的规则，则只需要配置专有和公共网络同时允许即可。
3. 如果没有clash-win64.exe的规则，可以通过下方的允许其他应用手动添加规则，具体clash核心文件的路径可以通过任务管理器后台或 **Clash for Windows\resources\static\files\win\x64\clash-win64.exe** 类似的路径查询到。添加规则的时候同时允许专用和公共即可。

### 在wsl中配置代理（方案一）
将下面几行脚本加入你的bashrc或者zshrc：

```bash
# proxy
export HOSTIP=$(cat /etc/resolv.conf | grep "nameserver" | cut -f 2 -d " ")
export http_proxy="http://$HOSTIP:7890"
export https_proxy="http://$HOSTIP:7890"
export all_proxy="socks5://$HOSTIP:7890"
export ALL_PROXY="socks5://$HOSTIP:7890"
```

### 在wsl中配置代理（方案二）
将下面几行脚本加入你的bashrc或者zshrc：

```bash
# proxy
proxy () {
  export HOSTIP=$(cat /etc/resolv.conf | grep "nameserver" | cut -f 2 -d " ")
  export http_proxy="http://$HOSTIP:7890"
  export https_proxy="http://$HOSTIP:7890"
  export ALL_PROXY="http://$HOSTIP:7890"
  #export all_proxy="socks5://$HOSTIP:7890"
  #export ALL_PROXY="socks5://$HOSTIP:7890"
  #export ALL_PROXY="http://$host_ip:7890"
  echo "HTTP Proxy on"
}
# no proxy
unproxy () {
  unset http_proxy 
  unset https_proxy
  #unset all_proxy
  unset ALL_PROXY
  echo "HTTP Proxy off"
}
```

### Reference

[WSL配置Proxy代理引导](https://halc.top/p/6088c65c)

[What's the difference between `hostip` and `windows ip` when set WSL2 proxy by Clash `Allow LAN`?](https://superuser.com/questions/1742501/whats-the-difference-between-hostip-and-windows-ip-when-set-wsl2-proxy-by-c)

[Use proxy in WSL2🚀](https://gist.github.com/aucker/d0fce5477e02cdd7fa76c1c81a87a610)