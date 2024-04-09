---
title: cpp 基础
date: 2024-04-09 16:18:10
tags:
    - C/C++
---
1. 预处理：`gcc -E hello.c -o hello.i`
2. 编译：`gcc -S hello.i -o hello.s`
3. 汇编：`gcc -c hello.s hello.o` 得到二进制文件
4. 链接：`gcc hello -o hello`  得到可执行程序

```cpp
// <stdio.h> 在 /usr/include

//C++中三目运算符返回的是变量,可以继续赋值
 (a > b ? a : b) = 100;

// const修饰的是指针，指针指向可以改，指针指向的值不可以更改
const int *p1 = &a;
p1 = &b; // 正确
//*p1 = 100;  报错

// const修饰的是常量，指针指向不可以改，指针指向的值可以更改
int *const p2 = &a;
// p2 = &b; //错误
*p2 = 100; // 正确

// const既修饰指针又修饰常量
const int *const p3 = &a;
// p3 = &b; //错误
//*p3 = 100; //错误

//技巧：看const右侧紧跟着的是指针还是常量, 是指针就是常量指针，是常量就是指针常量
```

当数组名传入到函数作为参数时，被退化为指向首元素的指针
