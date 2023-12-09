---
title: C/C++语言回顾（中）
date: 2022-03-11 14:21:01
tags:
    - C/C++
declare: true
toc: 1
---

[C/C++参考手册1](https://doc.bccnsoft.com/docs/cppreference/index.html)
[C/C++参考手册2](http://www.cplusplus.com/reference/clibrary/)

### 1 线性表的顺序存储
```C++
//顺序表的定义:
#define MaxSize 50 //定义线性表的长度
typedef struct
{
    ElemType data[MaxSize]; //顺序表的元素
    int len;                //顺序表的当前长度
} SqList;                   //顺序表的类型定义
```
<!--more-->

动态分配的数组是否还属于顺序存储结构？ 
>那当然仍然属于！依然可以随机存取。

- C的初始动态分配语句为 :
`L.data = (ElemType *)malloc(sizeof(ElemType) * InitSize);`

- C++ 的初始动态分配语句为 :
`L.data = new ElemType[InitSize];`

---

#### 顺序表的增删改查
```C++
#include <iostream>
using namespace std;
#include <stdio.h>
#include <stdlib.h>

#define MaxSize 50    //定义线性表的长度
typedef int ElemType; //顺序表中的元素类型

//静态分配
typedef struct
{
    ElemType data[MaxSize]; //顺序表的元素
    int length;             //顺序表的当前长度
} SqList;                   //顺序表的类型定义

// //动态分配
// #define InitSize 100
// typedef struct{
//     ElemType *data;
//     int capacity;//动态数组的最大容量
//     int length;
// }SeqList;
// L.data = (ElemType *)malloc(sizeof(ElemType) * InitSize);


bool ListInsert(SqList &L, int i, ElemType e) // i代表要插入的位置，从1开始，e是要插入的元素
{
    if (i < 1 || i > L.length + 1)
    { //判断要插入的位置是否合法
        return false;
    }
    if (L.length >= MaxSize) //元素存满了，不能再存了
        return false;
    for (int j = L.length; j >= i; j--)
    { //移动顺序表中的元素
        L.data[j] = L.data[j - 1];
    }
    L.data[i - 1] = e; //数组下标从0开始，插入第一个位置，访问的下标为0
    L.length++;
    return true;
}

void PrintList(SqList L) //这里不必再对L进行引用
{                        //打印顺序表元素
    for (int i = 0; i < L.length; i++)
    {
        cout << L.data[i] << ' ';
    }
    cout << endl;
}

bool ListDelete(SqList &L, int i, ElemType &e)
{
    if (i < 1 || i > L.length)
    { //如果删除的位置不合法
        return false;
    }
    if (L.length == 0)
    { //没有元素，无需删除
        return false;
    }

    e = L.data[i - 1];                 //获取顺序表中对应的元素，赋值给e
    for (int j = i; j < L.length; j++) //其实可以直接用i
    {
        L.data[j - 1] = L.data[j];
    }
    L.length--;
    return true;
}

int LocateElem(SqList L, ElemType e)
{
    for (int i = 0; i < L.length; i++)
    {
        if (L.data[i] == e)
        {
            return i + 1; //加1就是元素在顺序表中的位置
        }
    }
    return 0;
}

int main()
{
    SqList L;     //顺序表的名称
    ElemType del; //用来存储要删除的元素
    bool ret;     //查看返回值,是否插入成功
    //首先手动在顺序表中赋值
    L.data[0] = 1;
    L.data[1] = 2;
    L.data[2] = 3;
    L.length = 3; //总计三个元素

    ret = ListInsert(L, 2, 60); //往第二个位置插入60这个元素
    if (ret)
    {
        cout << "插入成功" << endl;
        PrintList(L);
    }
    else
    {
        cout << "插入失败" << endl;
    }

    ret = ListDelete(L, 1, del);
    if (ret)
    {
        cout << "删除成功" << endl;
        cout << "删除的元素是:" << del << endl;
        PrintList(L);
    }
    else
    {
        cout << "删除失败" << endl;
    }

    int elem_pos = LocateElem(L, 60);
    if (elem_pos)
    {
        cout << "查找成功" << endl;
        cout << "元素位置为:" << elem_pos << endl;
    }
    else
    {
        cout << "查找失败" << endl;
    }
    return 0;
}
```

### 2 线性表的链式存储
```C++

```