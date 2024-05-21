---
title: openwrt ipv6 开启记录
date: 2023-05-10 20:06:43
tags:
    - openwrt
    - 路由器
declare: true
---
1. 是否双栈下发（同时生成 wan 和 wan6），是否下发 pd（prefix delegation）。
2. 路由器需要安装**odhcp6c,odhcpd,ip6tables,luci-proto-ipv6**，直接使用一个成熟的固件，自带<!--more-->
3. wan口设置：
![20230510225020](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230510225020.png)
5. lan口设置Ipv6分配长度64，高级设置如下：
![20230510225225](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230510225225.png)
6. lan口IPv6设置：
![20230510225325](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230510225325.png)
7. 这个禁止解析 IPv6 DNS 记录的勾要取消掉：
![20230510225519](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230510225519.png)

----------------------------
# 参考
[Openwrt Lan 口无法获取IPV6问题（已解决）](https://www.right.com.cn/forum/thread-3071516-1-1.html)
[IPV6设置求教！（已经解决）](https://www.right.com.cn/forum/thread-2766108-2-1.html)

----------------------------
失败截图：
![20230510225812](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230510225812.png)
![20230510225850](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230510225850.png)

-------------------------------

## 补充

> **校园网不发 pd，但是 wan 口可以拿到一个 2408 开头的 ipv6 公网地址，且是 64 的后缀，所以可以通过中继，让 lan 口拿到 ipv6 地址，并下发。**

1. 网络诊断 ping v6, 必须通过
2. wan 口和 lan 口关闭使用内置的 IPv6 管理
3. IPv6 ULA前缀删除
4. lan 口的IPv6设置中，路由通告服务、DHCPv6服务、NDP代理全改为中继模式
5. /etc/config/dhcp 添加如下配置
```shell
config dhcp 'wan6'
        option interface 'wan'
        option ra 'relay'
        option dhcpv6 'relay'
        option ndp 'relay'
        option master '1'
```

6. 重启路由器，lan 下设备就可以得到 ipv6 地址了。或者重启网卡
7. lan 的 IPV6 设置添加通告的 DNS 服务器地址 2001:4860:4860::8888 和 2001:4860:4860::8844（有时候可以不加）
8. ping 路由器 ipv6 地址（有时候很重要）
9. ping -6 www.baidu.com
10. [ipv6 测试](https://test-ipv6.com/)，通过！！！