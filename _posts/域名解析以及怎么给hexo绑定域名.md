---
title: 域名解析以及怎么给hexo绑定域名
date: 2021-05-30 19:11:41
tags:
declare: true
---
> temp email
### 1 首先进入[freenom官网](https://www.freenom.com/zh/index.html)
- 全程以美国IP进行，点击**Partners**申请账号。（需要美国身份信息）
- 申请一个域名<!--more-->
### 2 进入[Cloudflare官网](https://dash.cloudflare.com/login)
其实freenom自带的已经有域名解析功能了，但这个是专业的，而且还能套CDN防止被墙
### 3 解析
- 添加域名到cloudflare
- 需要填写域名服务器，耐心等待检查
- 添加A记录可以直接解析到IP上
- 添加CNAME可以指向另一个网址
- 直接输入'@'就是不加前缀
### 4 绑定到hexo
1. 找到当前项目，点击Settings->Pages
在Custom domain中输入自己的域名
2. 在cloudflare->DNS中添加CNAME记录（不要勾选小云朵）
3. 在本地博客文件夹的source中新建一个CNAME文件
这个文件不能有任何格式，txt都不行。内容填写刚才的域名即可。
### 5 域名购买推荐
[NameSilo](https://www.namesilo.com/?rid=d7fe456te)，[万网](https://wanwang.aliyun.com/)
如果要续费，注意好次年的价格！

### References
[Nameservers教程](https://caq98i.top/article/?page=100)