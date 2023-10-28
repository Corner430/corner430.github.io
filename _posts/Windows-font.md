---
title: Windows font
date: 2023-04-03 18:25:28
tags:
declare: true
---
Original font: **msyh.ttc**<!--more-->

----------------

`regedit`
Run the command prompt `regedit` as an administrator, open the registry, and navigate to `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion\Fonts`
Find the font you are satisfied with and right click - modify - copy value data
Find `Microsoft YaHei & Microsoft YaHei UI (TrueType)` and change the following `msyh.ttc` to the font you want. Note: only change the value data, the value name should not be changed

After the modification, exit the registry and restart!
Check if the font has changed?

> Recommended font: **arial.ttf**