---
title: Unix/Linux 上的五种 IO 模型
date: 2024-07-21 02:47:03
tags:
  - C/C++
  - 网络编程
declare: true
toc: 1
---

## Unix/Linux 上的五种 `IO` 模型

### 1 阻塞 `blocking`<!--more-->

![20240616171924](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20240616171924.png)

### 2 非阻塞 `non-blocking`

```cpp
int opt = 1;
setsockopt(sockfd, SOL_SOCKET, SO_NONBLOCK, &opt, sizeof(opt));
```

![20240616171956](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20240616171956.png)

### 3 IO 复用（IO multiplexing）

在 `IO` 复用模型中，使用 `select`、`poll` 或 `epoll` 等系统调用，可以同时监视多个文件描述符，当其中一个或多个文件描述符准备好时，通知应用程序进行处理。这种模型常用于处理高并发连接。

![20240616172046](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20240616172046.png)

### 4 信号驱动(signal-driven)

在信号驱动 `IO` 模型中，应用程序为文件描述符设置信号驱动模式，当数据准备好时，内核会向应用程序发送信号，通知其进行数据处理。应用程序不需要频繁轮询。

设置信号驱动模式：

```cpp
fcntl(sockfd, F_SETFL, O_ASYNC);
signal(SIGIO, signal_handler);
```

![20240616172109](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20240616172109.png)

内核在第一个阶段是异步，在第二个阶段是同步；与非阻塞 `IO` 的区别在于它提供了消息通知机制，不需要用户进程不断的轮询检查，减少了系统 API 的调用次数，提高了效率。

### 5 异步(asynchronous)

在异步 `IO` 模型中，应用程序发起 `IO` 请求后立即返回，内核会在数据准备好并完成读写操作后，**通过回调函数或信号通知应用程序**。应用程序可以在等待 IO 完成的同时执行其他任务。

定义异步 IO 控制块：

```cpp
struct aiocb {
  int aio_fildes;             // 文件描述符
  off_t aio_offset;           // 偏移量
  volatile void *aio_buf;     // 缓冲区
  size_t aio_nbytes;          // 传输字节数
  int aio_reqprio;            // 请求优先级
  struct sigevent aio_sigevent; // 信号事件
  int aio_lio_opcode;         // 操作码
};
```

![20240616172210](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20240616172210.png)
