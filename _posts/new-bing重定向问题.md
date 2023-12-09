---
title: new bing重定向问题
date: 2023-05-12 13:48:17
tags:
    - bing
    - openwrt
    - 路由器
declare: true
---
修改hosts文件，添加如下内容：<!--more-->

```bash
    # bing
20.196.210.19 bing.com
20.196.210.19 www.bing.com
20.196.210.19 r.bing.com
20.196.210.19 cn.bing.com
20.196.210.19 edgeservices.bing.com     #' 登陆 Bing 很慢，打不开,添加 '# New Bing Login

40.126.35.80 login.microsoftonline.com
20.190.163.18 login.live.com
13.107.253.59 logincdn.msauth.net
13.107.253.59 acctcdn.msauth.net
13.107.253.59 acctcdn.msftauth.net
13.107.253.59 lgincdnvzeuno.azureedge.net
```
----------------------------------
**或者直接修改openwrt中的hosts文件**
- ssh到openwrt
- 进入/etc，新建一个myhosts文件，并写入上述内容
- 配置**额外的hosts文件**
![20230512141301](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230512141301.png)

-------------------------------------------
openwrt的passwall中添加如下规则至代理列表：
```shell
bing.com
microsoft.com   # 这个本来在直连列表中
```

---------------------------
**重启路由器**

-----------------
chrome使用new bing chat ai
- 安装[Chrome Unlock New Bing AI](https://chrome.google.com/webstore/detail/chrome-unlock-new-bing-ai/nglhdhdfndbadmaiieikpefenkbgpdbf)插件
- [github源代码](https://github.com/malaohu/chrome_extensions_unlock-newbing)
- 参考[最新更新：Chrome体验New Bing AI 扩展](https://jike.info/topic/17774/%E6%9C%80%E6%96%B0%E6%9B%B4%E6%96%B0-chrome%E4%BD%93%E9%AA%8Cnew-bing-ai-%E6%89%A9%E5%B1%95)

------------------------------
- 此时如果还有问题，注意**Bing**的**url**本来是带**cn**的，删除掉：
![20230512223854](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230512223854.png)
- 修改国家为非china：
![20230512224120](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230512224120.png)