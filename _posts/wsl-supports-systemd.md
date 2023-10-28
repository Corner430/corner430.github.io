---
title: wsl supports systemd
date: 2023-04-03 16:09:51
tags:
---
- Add these lines to the */etc/wsl.conf* (note you will need to run your editor with sudo privileges, e.g: sudo vim */etc/wsl.conf*):
```shell
[boot]
systemd=true
```

> Pay attention to port conflicts

- Reference
[Systemd support is now available in WSL!](https://devblogs.microsoft.com/commandline/systemd-support-is-now-available-in-wsl/)