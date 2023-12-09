---
title: WSL开机自启动并在后台运行
date: 2023-05-25 15:07:30
tags:
    - windows
    - WSL
declare: true
---
- 在Windows中创建一个vbs脚本文件，例如wsl.vbs，该文件包含以下内容：
```shell
Set ws = CreateObject("Wscript.Shell")
ws.run "wsl -d <distribution_name>", 0
```
> 其中，`<distribution_name>`是您的WSL发行版的名称。例如，如果您的发行版名称为Ubuntu，则应将其替换为Ubuntu。
> `0`是一个常量，它指示在调用Shell函数时，所调用程序的窗口样式是隐藏的。0表示隐藏窗口，1表示正常窗口，2表示最小化窗口。
- 将vbs脚本文件复制到Windows启动文件夹中。要打开Windows启动文件夹，请按`Win + R`键，输入`shell:startup`，然后单击`确定`。
- 重新启动计算机以使更改生效。
- references [Dev-blog by WS](https://www.cnblogs.com/wswind/p/17201979.html)