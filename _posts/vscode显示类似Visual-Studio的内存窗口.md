---
title: vscode显示类似Visual Studio的内存窗口
date: 2023-04-17 14:48:40
tags:
declare: true
toc: 1
---
Visual Studio有着这样的内存窗口显示<!--more-->
![20230417145048](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230417145048.png)
怎么在vscode中达到同等效果，有两种方案。

### 插件
VSCode本身并不提供像Visual Studio一样查看内存的功能。不过，你可以通过安装适当的扩展来获得此功能。

有一个名为**CppTools**的扩展可以提供类似于Visual Studio的内存窗口和堆栈窗口。安装此扩展后，你可以通过以下步骤查看内存：

- 在VSCode中打开一个C++项目。
- 启动调试器，并在程序暂停时进入调试模式。
- 在"调试"菜单中选择"打开内存窗口"。
- 在内存窗口中输入你要查看的内存地址。
- 查看该地址处的内存内容。

> 请注意，由于C++语言的灵活性，你需要非常小心地处理指针和内存管理，以避免程序崩溃或导致安全漏洞。因此，使用内存窗口时请务必小心谨慎。

### 程序
```cpp
/* 这个函数接受两个参数：一个指向要打印的内存区域的起始地址的指针和要打印的字节数。它将内存区域中的每个字节打印为两个十六进制数字，每行打印16个字节。*/
#include <stdio.h>
#include <stdint.h>

void print_memory(const void* start_addr, size_t num_bytes) {
    const uint8_t* data = (const uint8_t*)start_addr;
    for (size_t i = 0; i < num_bytes; i++) {
        printf("%02X ", data[i]);
        if ((i+1) % 16 == 0) printf("\n");
    }
    printf("\n");
}


int main() {
    int32_t x = 12345;
    print_memory(&x, sizeof(x));
    return 0;
}

//结果如下：
39 30 00 00 

/* 这是x变量的十六进制表示形式，从低位到高位排列。注意，这个输出的格式仅用于演示目的，实际应用中你可能需要根据需要修改print_memory函数以满足你的具体需求。

需要注意的是，这种方法仅适用于在程序中显示内存的内容，而不是像Visual Studio内存窗口一样在程序运行时调试内存。因此，它可能无法满足某些高级调试需求。*/
```

-------------------------------------------------
下面是使得每行显示四个字节，行首显示地址的程序
```cpp
#include <stdio.h>
#include <stdint.h>

void print_memory(const void* start_addr, size_t num_bytes) {
    const uint8_t* data = (const uint8_t*)start_addr;
    const uintptr_t start_address = (uintptr_t)start_addr;
    const uintptr_t end_address = start_address + num_bytes;
    const size_t bytes_per_row = 4;
    for (uintptr_t i = start_address; i < end_address; i++) {
        if ((i - start_address) % bytes_per_row == 0) {
            printf("%p  ", (void*)i);
        }
        printf("%02X ", data[i - start_address]);
        if ((i - start_address + 1) % bytes_per_row == 0) {
            printf("\n");
        }
    }
    if ((end_address - start_address) % bytes_per_row != 0) {
        printf("\n");
    }
}

int main() {
    int32_t arr[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    print_memory(arr, sizeof(arr));
    return 0;
}
```
效果如下：
![20230417145836](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230417145836.png)

-------------------------------------------
下面是最后的形式，完美对应Visual Studio，在每行末尾，显示对应的ASCII码值
```cpp
/* 在这个修改后的函数中，我们在每行的末尾打印出当前行的四个字节所对应的ASCII字符（仅打印可打印的ASCII字符，其它字符显示为'.'）。为此，我们在循环迭代每个字节时，将当前字节添加到一个缓冲区中，该缓冲区保存当前行中的所有字节所对应的ASCII字符。当该行的所有字节都被打印时，我们在该行的末尾打印出该缓冲区中的内容。

在调用这个函数时，你可以将要显示的内存区域的起始地址和大小作为参数传递。 */
#include <stdio.h>
#include <stdint.h>

void print_memory(const void* start_addr, size_t num_bytes) {
    const uint8_t* data = (const uint8_t*)start_addr;
    const uintptr_t start_address = (uintptr_t)start_addr;
    const uintptr_t end_address = start_address + num_bytes;
    const size_t bytes_per_row = 4;
    char ascii[bytes_per_row + 1]; // 用于存储当前行的ASCII字符
    ascii[bytes_per_row] = '\0';
    for (uintptr_t i = start_address; i < end_address; i++) {
        if ((i - start_address) % bytes_per_row == 0) {
            printf("%p  ", (void*)i);
        }
        printf("%02X ", data[i - start_address]);
        ascii[i % bytes_per_row] = (data[i - start_address] >= 32 && data[i - start_address] <= 126) ? data[i - start_address] : '.'; // 只显示可打印的ASCII字符，其它显示为'.'
        if ((i - start_address + 1) % bytes_per_row == 0) {
            printf(" %s\n", ascii);
        }
    }
    if ((end_address - start_address) % bytes_per_row != 0) {
        // 输出最后一行的ASCII字符
        for (size_t i = (end_address - start_address) % bytes_per_row; i < bytes_per_row; i++) {
            printf("   ");
            ascii[i] = ' ';
        }
        printf(" %s\n", ascii);
    }
}

int main() {
    int32_t arr[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    print_memory(arr, sizeof(arr));
    return 0;
}
```
效果如下：
![20230417150156](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230417150156.png)

现在你可以使用这个函数打印内存中的任何内容，每行将显示四个字节，行首将显示地址，行末将显示这四个字节所对应的ASCII字符，类似于Visual Studio的内存窗口效果。