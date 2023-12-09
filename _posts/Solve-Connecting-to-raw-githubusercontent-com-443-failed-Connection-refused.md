---
title: >-
  Solve Connecting to raw.githubusercontent.com :443... failed: Connection
  refused.
date: 2023-04-03 16:55:37
tags:
  - hosts
  - raw.githubusercontent.com
---
- get ip
Use the URL: https://www.ipaddress.com/ to get the ip address of the raw.githubusercontent.com website: 185.199.108.133.

- modify the hosts file
`sudo vim /etc/hosts`
Add in the last line: `185.199.108.133 raw.githubusercontent.com`
