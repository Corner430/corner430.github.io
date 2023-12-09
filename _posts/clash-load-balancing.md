---
title: clash load balancing
date: 2023-04-04 09:38:31
tags:
  - clash
  - 科学上网
declare: true
toc: 1
---
### 1. The computer uses Paesers to achieve load balancing <!--more-->
Replace the content in **setting->Profiles->Parsers** with the following:
```shell
parsers:
  - reg: 'slbable$'
    yaml:
      append-proxy-groups:
        - name: ⚖️ 负载均衡-散列
          type: load-balance
          url: 'http://www.google.com/generate_204'
          interval: 300
          strategy: consistent-hashing
        - name: ⚖️ 负载均衡-轮询
          type: load-balance
          url: 'http://www.google.com/generate_204'
          interval: 300
          strategy: round-robin
      commands:
        - proxy-groups.⚖️ 负载均衡-散列.proxies=[]proxyNames
        - proxy-groups.0.proxies.0+⚖️ 负载均衡-散列
        - proxy-groups.⚖️ 负载均衡-轮询.proxies=[]proxyNames
        - proxy-groups.0.proxies.0+⚖️ 负载均衡-轮询
```
- `reg: 'slbable$'`Triggered when the suffix of the url is `slbable`
- **负载均衡-轮询**Representatives use different ip to access
- **负载均衡-散列**Hash algorithm is used on behalf of the same second-level domain name, and high-quality nodes are selected for access. Avoid ip risk control problems.
- `interval: 300`Represents a speed measurement and conversion every 300 seconds, and nodes that time out are not counted

### 2. On the computer side, the `.yaml` file is dragged in to take effect
1. Export url through v2ray.
2. Use [subconverter](https://github.com/tindy2013/subconverter) and [简易版转换工具](https://bulianglin.com/archives/51.html) to achieve subscription conversion.
3. Open the converted link in the browser.
4. Save as `.yaml` file
5. Fill in `file\\file path#slbable` in `setting->url` to trigger subscription refresh, the essence is to refer to local files.

### 3. Rewrite the .yaml file, common to all platforms.
1. Open the .yaml file with an editor, vscode is the best.
2. Find **proxy-groups->proxies (first)**, add two options
```shell
#Added to the first proxy policy group
      - ⚖️ 负载均衡-轮询
      - ⚖️ 负载均衡-散列
```
3. Find the last group and add the following content after it.
```shell
#Add proxy policy group
  - name: ⚖️ 负载均衡-散列
    type: load-balance
    url: http://www.google.com/generate_204
    interval: 300
    strategy: consistent-hashing
    proxies:
      - P1
      - P2
      - P3
  - name: ⚖️ 负载均衡-轮询
    type: load-balance
    url: http://www.google.com/generate_204
    interval: 300
    strategy: round-robin
    proxies:
      - P1
      - P2
      - P3
```
Among them, P1, P2, and P3 represent nodes.
4. Import the configuration file to the required device.