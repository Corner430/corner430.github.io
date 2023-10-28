---
title: openwrt ipv6 开启记录
date: 2023-05-10 20:06:43
tags:
declare: true
---
1. 删除原有的wan,wan6口。
2. 新建wan口拨号，看是否是双栈下发，即同时生成wan & wan6.
3. 路由器需要安装**odhcp6c,odhcpd,ip6tables,luci-proto-ipv6**<!--more-->
4. wan口设置：
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