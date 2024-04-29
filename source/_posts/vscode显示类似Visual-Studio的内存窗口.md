---
title: vscode显示类似Visual Studio的内存窗口
date: 2023-04-17 14:48:40
tags:
    - vscode
    - C/C++
declare: true
toc: 1
---
Visual Studio内存窗口<!--more-->
![20230417145048](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230417145048.png)

```cpp
/* 在每行的末尾打印出当前行的四个字节所对应的ASCII字符（仅打印可打印的ASCII字符，其它字符显示为'.'）

将要显示的内存区域的起始地址和大小作为参数传递。 */
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
