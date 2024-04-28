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
| `nexti` | 执行下一行汇编代码 |
| `step` | 执行下一条指令，进入函数 |
| `(l)ist` | 显示代码 |
| `(p)rint x` | 显示变量的值 |
| `(q)uit` | 退出GDB |
| `clear` | 清除所有断点 |
| `continue` | 继续正常执行 |
| `info b` | 查看断点信息 |
| `watch(expr)` | 监视变量expr的变化 |
| `ref` | 刷新屏幕 |
| `info registers` | 查看寄存器信息 |

```gdb
(gdb) p &a
$2 = (int *) 0x7fffffffe17c

(gdb) x/4xw 0x7fffffffe17c
0x7fffffffe17c:	0x00000001	0x55555310	0x00005555	0xf7af809b

(gdb) x/10xw &a

(gdb) x/4i 0x7fffffffe17c
   0x7fffffffe17c:	add    %eax,(%rax)
   0x7fffffffe17e:	add    %al,(%rax)
   0x7fffffffe180:	adc    %dl,0x55(%rbx)
   0x7fffffffe183:	push   %rbp
(gdb) disas
```

`lay next` 会显示一些代码以一种比较好的视图
