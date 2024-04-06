---
title: mysql 远程登录
date: 2024-04-06 23:20:31
tags:
    - mysql
---
解决远程登录 mysql 问题 <!--more-->
```mysql
// 从mysql.user表中选择host，其中用户为'root'
SELECT host FROM mysql.user WHERE User = 'root';

// 创建一个新用户'root'，该用户可以从'ip_address'登录，并使用'some_pass'作为密码
CREATE USER 'root'@'ip_address' IDENTIFIED BY 'some_pass';

// 授予新用户所有权限
GRANT ALL PRIVILEGES ON *.* TO 'root'@'ip_address';

// 刷新权限，使之立即生效
FLUSH PRIVILEGES;

// 为新用户设置新密码
SET PASSWORD FOR 'root'@'ip_address' = 'some_pass';

// 重启系统，使更改生效
systemctl restart
```