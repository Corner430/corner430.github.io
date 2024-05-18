---
title: x-ui搭建节点
date: 2024-05-16 03:35:11
tags:
---

- [1 安装 FranzKafkaYu/x-ui](#1-安装-franzkafkayux-ui)
- [2 使用 docker-compose 运行](#2-使用-docker-compose-运行)
- [3 进入 web 界面进行设置](#3-进入-web-界面进行设置)
- [4 搭建 vmess+ws 服务端（最通用）](#4-搭建-vmessws-服务端最通用)
- [5 搭建 vless+vision+tls 服务端](#5-搭建-vlessvisiontls-服务端)
- [6 搭建 vless+vision+reality 服务端](#6-搭建-vlessvisionreality-服务端)
- [7 vmess 无 CDN](#7-vmess-无-cdn)
- [8 hysteria2](#8-hysteria2)
<!--more-->

### 1 安装 [FranzKafkaYu/x-ui](https://github.com/FranzKafkaYu/x-ui)

```shell
docker pull enwaiax/x-ui:alpha-zh
```

### 2 使用 docker-compose 运行

```yml
services:
  xui:
    image: enwaiax/x-ui:alpha-zh
    tty: true
    container_name: xui
    volumes:
      - $PWD/db/:/etc/x-ui/
      - $PWD/cert/:/root/cert/
    restart: unless-stopped
    network_mode: host
```

### 3 进入 web 界面进行设置

- 访问 `http://ip:54321`，默认用户名密码为 `admin` `admin`
- 进入面板设置，修改监听端口、密码等
- 切换版本为 `1.8.3`

### 4 搭建 vmess+ws 服务端（最通用）

1. 备注：`vmess+ws`
2. 协议：`vmess`
3. 添加一个用户
4. 网络：`ws`
5. 复制 **`id` 第一段** 追加到 **路径** 后面
6. 添加

![20240516115145](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20240516115145.png)

**可以导入软路由**

![20240516123505](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20240516123505.png)

### 5 搭建 vless+vision+tls 服务端

1. 备注：`vless+vision`
2. 添加用户
3. 打开 `tls` 开关
4. flow：`xtls-rprx-vision`
5. 将域名ip `1.1.1.1` 改为 `1-1-1-1.nip.io` 的形式
6. 安装证书工具
    ```shell
    curl https://get.acme.sh | sh; apt install socat -y || yum install socat -y; ~/.acme.sh/acme.sh --set-default-ca --server letsencrypt
    ```
7. 申请证书，记得**修改 ip 为自己的 ip**
    ```shell
    # 三种方式任选其中一种，申请失败则更换方式
    # 申请证书方式1： 
    ~/.acme.sh/acme.sh  --issue -d 1-1-1-1.nip.io --standalone -k ec-256 --force --insecure
    # 申请证书方式2： 
    ~/.acme.sh/acme.sh --register-account -m "${RANDOM}@chacuo.net" --server buypass --force --insecure && ~/.acme.sh/acme.sh  --issue -d 1-1-1-1.nip.io --standalone -k ec-256 --force --insecure --server buypass
    # 申请证书方式3： 
    ~/.acme.sh/acme.sh --register-account -m "${RANDOM}@chacuo.net" --server zerossl --force --insecure && ~/.acme.sh/acme.sh  --issue -d 1-1-1-1.nip.io --standalone -k ec-256 --force --insecure --server zerossl
    ```

8. 安装证书
    ```shell
    ~/.acme.sh/acme.sh --install-cert -d 1-1-1-1.nip.io --ecc --key-file /etc/x-ui/server.key --fullchain-file /etc/x-ui/server.crt

    # 输出如下
    [Thu May 16 12:13:43 CST 2024] Installing key to: /etc/x-ui/server.key
    [Thu May 16 12:13:43 CST 2024] Installing full chain to: /etc/x-ui/server.crt
    cat: /root/.acme.sh/10-0-0-4.nip.io_ecc/fullchain.cer: No such file or directory
    ```

9. **密钥文件路径**为上面**输出**的 `/etc/x-ui/server.key`
10. **公钥文件路径**为上面**输出**的 `/etc/x-ui/server.crt`
11. 添加

![20240516121901](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20240516121901.png)

> **导入后将 地址 由 `1-1-1-1.nip.io` 改为 `1.1.1.1` 形式的 ip 地址，可以加快速度**

### 6 搭建 vless+vision+reality 服务端

1. 备注：`vless+vision+reality`
2. 端口：`443`
3. 添加用户
4. 打开 `reality` 开关
5. flow：`xtls-rprx-vision`
6. **目标网站的网址部分**改为：`1.1.1.1`，保留端口号
7. 使用 **`id` 第一段** 覆盖 **可选域名**，并追加 `.com` 到**可选域名**的尾部
8. 添加

![20240516122437](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20240516122437.png)

> 如果想要增强伪装，可以点击 **目标网站** 后面的刷新按钮，**不要使用 speedtest** !!!，之后点击修改就好了

### 7 vmess 无 CDN

直接点击添加入站，之后选择 `vmess` 创建节点并添加用户即可

### 8 hysteria2

[ref: 【遥遥领先】黑科技！垃圾VPS线路网速大幅提升！第二代hysteria节点搭建教程，歇斯底里为什么可以提升网速？windows/ios/android手机移动端使用方式，sing-box配置hy2方式](https://www.youtube.com/watch?v=CXj-ID33MhU&t=10s&ab_channel=%E4%B8%8D%E8%89%AF%E6%9E%97)

[hysteria github](https://github.com/apernet/hysteria?tab=readme-ov-file)

[Hysteria 中文文档](https://v2.hysteria.network/zh/)

> **申请证书需要放行 80，443 端口**


**服务器相关指令**

```shell
#一键安装Hysteria2
bash <(curl -fsSL https://get.hy2.sh/)

#生成自签证书
openssl req -x509 -nodes -newkey ec:<(openssl ecparam -name prime256v1) -keyout /etc/hysteria/server.key -out /etc/hysteria/server.crt -subj "/CN=bing.com" -days 36500 && sudo chown hysteria /etc/hysteria/server.key && sudo chown hysteria /etc/hysteria/server.crt

#启动Hysteria2
systemctl start hysteria-server.service
#重启Hysteria2
systemctl restart hysteria-server.service
#查看Hysteria2状态
systemctl status hysteria-server.service
#停止Hysteria2
systemctl stop hysteria-server.service
#设置开机自启
systemctl enable hysteria-server.service
#查看日志
journalctl -u hysteria-server.service
```

**服务器配置文件**

```shell
cat << EOF > /etc/hysteria/config.yaml
listen: :443 #监听端口，可修改

#使用CA证书
#acme:
#  domains:
#    - a.com #你的域名，需要先解析到服务器ip
#  email: test@sharklasers.com

#使用自签证书
#tls:
#  cert: /etc/hysteria/server.crt
#  key: /etc/hysteria/server.key

auth:
  type: password
  password: 123456 #设置认证密码
  
masquerade:
  type: proxy
  proxy:
    url: https://bing.com #伪装网址，可修改
    rewriteHost: true
EOF
```

**客户端配置文件**

```js
server: ip:443
auth: 123456

bandwidth:
  up: 20 mbps
  down: 100 mbps
  
tls:
  sni: a.com
  insecure: false #使用自签时需要改成true

socks5:
  listen: 127.0.0.1:1080
http:
  listen: 127.0.0.1:8080
```

**sing-box配置文件(Android/IOS)**

```json
{
  "dns": {
    "servers": [
      {
        "tag": "cf",
        "address": "https://1.1.1.1/dns-query"
      },
      {
        "tag": "local",
        "address": "223.5.5.5",
        "detour": "direct"
      },
      {
        "tag": "block",
        "address": "rcode://success"
      }
    ],
    "rules": [
      {
        "geosite": "category-ads-all",
        "server": "block",
        "disable_cache": true
      },
      {
        "outbound": "any",
        "server": "local"
      },
      {
        "geosite": "cn",
        "server": "local"
      }
    ],
    "strategy": "ipv4_only"
  },
  "inbounds": [
    {
      "type": "tun",
      "inet4_address": "172.19.0.1/30",
      "auto_route": true,
      "strict_route": false,
      "sniff": true
    }
  ],
  "outbounds": [
    {
      "type": "hysteria2",
      "tag": "proxy",
      "server": "ip",
      "server_port": 443,
      "up_mbps": 20,
      "down_mbps": 100,
      "password": "123456",
      "tls": {
        "enabled": true,
        "server_name": "a.com",
        "insecure": false
      }
    },
    {
      "type": "direct",
      "tag": "direct"
    },
    {
      "type": "block",
      "tag": "block"
    },
    {
      "type": "dns",
      "tag": "dns-out"
    }
  ],
  "route": {
    "rules": [
      {
        "protocol": "dns",
        "outbound": "dns-out"
      },
      {
        "geosite": "cn",
        "geoip": [
          "private",
          "cn"
        ],
        "outbound": "direct"
      },
      {
        "geosite": "category-ads-all",
        "outbound": "block"
      }
    ],
    "auto_detect_interface": true
  }
}
```