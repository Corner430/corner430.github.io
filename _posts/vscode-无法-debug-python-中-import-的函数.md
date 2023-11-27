---
title: vscode 无法 debug python 中 import 的函数
date: 2023-11-26 21:02:10
tags:
declare: true
---
找到当前项目的 .vscode/launch.json 文件，修改配置项 "justMyCode": false，即可。<!--more-->

> 注意 debug 时，要选中所配置的 launch.json 文件，否则仍然是全局配置在生效。