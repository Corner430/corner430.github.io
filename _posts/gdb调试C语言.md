---
title: gdb调试C语言
date: 2023-04-22 22:17:15
tags:
    - gdb
    - C/C++
declare: true
---
- 编译 `gcc -std=c11 -g test.cpp -o test` `-g` 选项是为了生成调试信息。
- `./test` 运行程序
- `gdb test` 进入调试模式 <--!more-->

| 命令 | 描述 |
| :---: | :--- |
| `(r)un` | 运行程序 |
| `(b)reak` | 设置断点 |
| `disable b` | 禁用断点 |
| `enable` | 启用断点 |
| `(n)ext` | 执行下一行代码 |
| `step` | 执行下一条指令，进入函数 |
| `(l)ist` | 显示代码 |
| `(p)rint x` | 显示变量的值 |
| `(q)uit` | 退出GDB |
| `clear` | 清除所有断点 |
| `continue` | 继续正常执行 |
| `info b` | 查看断点信息 |
