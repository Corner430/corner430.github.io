---
title: C/C++语言回顾(上)
date: 2022-03-05 10:26:28
tags:
    - C/C++
declare: true
toc: 1
---

[C/C++参考手册](https://doc.bccnsoft.com/docs/cppreference/index.html)

## C语言知识回顾

### 1 函数的递归调用
<!--more-->
```C
#include<stdio.h>

int f(int n) {
	if (n == 1) {
		return 1;//一定写结束条件
	}
	return n * f(n - 1);//第一步是写好公式
}


int main() {
	int n = 5;
	int result;
	result = f(n);
	printf("n! = %d", result);
	return 0;
}
```

> 最重要的在于写好公式！！！

### 2 scanf的使用

```C
#define _CRT_SECURE_NO_WARNINGS //解决scanf编译报错问题
#include <stdio.h>;

int main()
{
	int a;
	scanf("%d", &a); //一定要在变量前加&符号,和我们自己写的函数一样，如果不传递地址，就不会真正改变所要改变的变量的值
                    //和指针的传递是一样的
	printf("a = %d\n", a);
	return 0;
}
```

### 3 scanf的某些问题
```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>;

int main() {
	int a, b;
	scanf("%d%d", &a, &b);
	printf("a = %d\nb= %d", a, b);
	return 0;
}
```

### 4 scanf清空缓冲区

```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>;

//微软自创
//清空缓冲区，VS2012 fflush(stdin)
//stdin是标准输入
//VS2017-2022 rewind(stdin)

int main() {
	int i;
	while (rewind(stdin),scanf("%d", &i) != EOF) {
		printf("i = %d\n", i);
	}
	return 0;
}
```

### 5 循环读取字符

```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>

int main() {
	char c;
	while (scanf("%c", &c) != EOF) {
		if (c != '\n') {
			printf("%c", c - 32);
		}
		else {
			printf("\n");
		}
	}
	return 0;

}
```

### 6 scanf的混合输入
```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>


//一个scanf读取多种混合类型
//混合输入时，在%c前加“ ”
int main() {
	int i;
	char c;
	float k;
	int ret;
	ret = scanf("%d %c%f", &i, &c, &k);
	printf("i = %d,c = %c, k = %f", i, c, k);
	return 0;
}
```

### 7 自增运算

```C
#include<stdio.h>

int main() {//后加加和后减减不太好理解。
	int i = -1, j;
	j = i++ > -1;//相当于 j = i > -1;i++;

	printf("i = %d, j = %d\n", i, j);
	return 0;
}
```

### 8 scanf的标准输入缓冲区
```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>;

int main() {
	int j;
	char i;
	scanf("%c", &i);
	printf("i = %c\n", i);
	scanf("%d", &j);
	printf("j = %d\n", j);
	return 0;
}
```

### 9 if与else

```C
#define _CRT_SECURE_NO_WARNINGS     //在vs studio必须加这个宏才能使用scanf
#include<stdio.h>

// #include <stdio.h>
//   int scanf( const char *format, ... );

int main() {
	int i;
	while (scanf("%d", &i) != EOF) {    //scanf是有返回值的!!!
		printf("i = %d\n", i);
	}
	return 0;
}
```

### 10 变量和常量

```C
#include<stdio.h>	//引用头文件

#define	PI 3	//PI就是符号常量

int main() {
	int a = 3;	//a就是一个变量
	a = 5;
	//PI = 10;	//符号常量不可赋值
	printf("%d\n", PI);

	return 0;
}
```

### 11 数组
```C
#include<stdio.h>

void print(int a[], int numsize) {
	int i;
	for (i = 0; i < numsize; i++) {
		printf("a[i] = %d\n", a[i]);
	}
}

int main() {
	//int i, j;
	//j = 10;
	int a[5] = { 1,3,5,7,9 };
	//i = 5;
	//a[5] = 20;
	print(a, 5);
	return 0;
}
```

### 12 字符数组

```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>

int main() {
	char c[8] = { 'h','e','l','l','o' };
	char d[5] = "how";
	printf("%s----%s\n", c, d);

	char e[20],f[20];
	scanf("%s%s", e,f);
	printf("%s-----%s", e,f);

	return 0;
}
```

### 13 字符数组的传递
```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>

void print(char d[]) {
	int i = 0;
	while (d[i]) {//注意这里是单引号
		printf("%c", d[i++]);
		//i++;
	}
	printf("\n");
}


int main() {
	char c[10] = "hello";
	print(c);
	return 0;
}
```

### 14 gets和puts
```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>


//char* strcpy(char* to, const char* from);const修饰代表可以放字符串常量
int main() {
	char a[20];
	gets(a);

	printf("a的长度是：%d\n", strlen(a));
	//puts(a);
	char d[20];
	strcpy(d, a);
	puts(d);
	return 0;
}
```

### 15 指针的本质
```C
#include<stdio.h>

int main() {
	int i = 5;
	//int *a, *b, *c;
	int* i_point = &i;//&就是取地址，也称引用
	return 0;
}
```

### 16 指针的传递
```C
#include<stdio.h>

void change(int* j) {
	*j = 5;
}


int main() {

	int i = 10;
	//int* p = &i;
	printf("before change i = %d\n", i);
	change(&i);
	printf("after change i = %d\n", i);
	return 0;
}
```

### 17 指针与自增和自减与运算符
```C
#include<stdio.h>

int main() {
	int a[3] = { 3,7,9 };
	int* p = a;//让指针p指向a的开头
	int j;
	j = *p++;//相当于j = *p;p++;
	printf("a[0] = %d,j = %d,*p = %d\n", a[0], j, *p);
	return 0;
}
```

### 18 指针与动态内存申请

```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

//#include <stdlib.h>
//void* malloc(size_t size);
//功能： 函数指向一个大小为size的空间，如果错误发生返回NULL。 存储空间的指针必须为堆，不能是栈。这样以便以后用free函数释放空间

int main() {
	int i;//申请多大空间
	scanf("%d", &i);
	char* p;
	p = (char*)malloc(i);
	strcpy(p, "malloc success.");
	puts(p);
	p++;
	free(p);


	return 0;
}
```

### 19 栈空间和堆空间的差异
```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

char* print_stack() {
	char a[20] = "I am print_stack.";
	puts(a);//可以正常打印
	return a;
}

char* print_malloc() {
	char* p = (char*)malloc(30);
	p = "I am print_malloc.";
	puts(p);
	return p;
}

int main() {
	char* p;
	p = print_stack();//栈空间会随着函数执行的结束而释放
	//puts(p);//无法正常打印栈空间里面的值
	p = print_malloc();//堆空间不会随子函数的结束而释放，必须自己free
	puts(p);//堆空间的值可以正常打印
	return 0;
}
```

### 20 字符指针和字符数组的初始化
```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

int main() {
	char* p = "hello";
	char c[10] = "hello";
	c[0] = 'H';
	p[0] = 'H';//不可以对常量区数据进行修改
	printf("%c\n", p[0]);
	printf("%c\n", c[0]);
	//p = "world";//合法
	//c = "world";//非法

	return 0;
}
```

### 21 二级指针的偏移(可以换用c++的引用)
```C
#include<stdio.h>

void change(int** p, int* q) {
	*p = q;
}

int main() {
	int i = 10;
	int j = 5;
	int* pi = &i;
	int* pj = &j;
	printf("i = %d,*pi = %d,j = %d,*pj = %d\n", i, *pi, j, *pj);
	change(&pi, pj);//改变指针pi的指向
	printf("after change : i = %d,*pi = %d,j = %d,*pj = %d\n", i, *pi, j, *pj);
	return 0;
}
```

```C++
#include<stdio.h>
#include<stdlib.h>

//把&写到形参的位置是C++的语法,称为引用

void modify_num(int& b) {
	b += 1;
}

void modify_pointer(int*& p) {//引用符号挨着变量名
	p = (int*)malloc(20);
	p[0] = 5;
}//在子函数操作p和在主函数操作p手法一致

int main() {
	int a = 10;
	modify_num(a);
	printf("a = %d\n", a);

	int* p = NULL;
	modify_pointer(p);
	printf("p[0] = %d\n", p[0]);
	return 0;
}
```

### 22 结构体

```C
#include<stdio.h>
#include<stdlib.h>


//此结构体大小为68字节，因为存在对齐（按字编址），对齐是为了提高CPU访问内存的效率
struct student {
	int num;//num是结构体成员
	char name[20];
	char sex;
	int age;
	float score;
	char addr[30];
};//结构体类型声明，注意这里一定要加分号


int main() {
	struct student s = { 1001,"lele",'m',20,98.5,"shenzhen" };

	return 0;
}
```

### 23 结构体指针

```C
#include<stdio.h>
#include<stdlib.h>

struct student{
	int num;
	char name[20];
	char sex;
};

int main() {
	struct student s = { 1001,"wangle",'M' };
	struct student* p;
	p = &s;
	//printf("%d,%s,%c\n",*p.num,*p.name,*p.sex);优先级问题
	printf("%d,%s,%c\n", (*p).num, (*p).name, (*p).sex);//正确
	printf("%d,%s,%c\n", p->num, p->name, p->sex);//等价上一行
	return 0;
}
```

### 24 typedef的使用
#include<stdio.h>

```C
//给结构体类型以及结构体指针类型起别名
typedef struct student {
	int num;
	char name[20];
	char sex;
}stu,*pstu;

typedef int INTEGER;//注意分号

int main() {
	stu s = {1001,"wangle",'M'};
	pstu p;//等价stu* p1;
	INTEGER  i = 10;//为了实现代码即注释
	p = &s;
	printf("i = %d,p->num = %d\n", i, p->num);
	return 0;
}
```

## C++知识回顾
### 1 运算符与表达式
```C++
#include <iostream>
using namespace std;

int main()
{
    // c = a + 10;   //常量为int型
    // c = a + 10L;  //常量为long int型
    // c = a + 10.0; //常量为double型,占8个字节
    // c = a + 10.f; //常量为float型,占4个字节
    // c = a + 31;
    // c = a + 037;     //0开头代表八进制
    // c = a + 0x1f;     //0x开头代表十六进制

    cout << sizeof(int) << endl;
    cout << sizeof(10.0) << endl;  // 0开头代表八进制
    cout << sizeof(10.0f) << endl; // 0x开头代表十六进制

    //输出都是31
    cout << 31 << endl;
    cout << 037 << endl;
    cout << 0x1f << endl;

    cout << 1 + '0' << endl; //输出49

    //字符串常量的可连接性
    cout << "I am a string" << endl;
    cout << "I am a "
            "string"
         << endl;
    return 0;
}
```

```C++
#include <iostream>
using namespace std;

int main()
{
    int a = 1, b = 2, c = 3;
    c = (a = b); // c最终得到的是b的值
    cout << c << endl;

    a = 100, b = 200, c = 300;
    a = b > c;
    cout << a << endl; //输出0(假)
    a = b < c;
    cout << a << endl; //输出1(真)

    return 0;
}
```

```C++
#include <iostream>
using namespace std;

int main()
{
    int x = 1;
    int y = 2;
    cout << (x > y ? 'A' : 'B') << endl;
    x = 2;
    y = 1;
    cout << (x > y ? 'A' : 'B') << endl;
    return 0;
}
```

### 2 bitset操作
```C++
#include <iostream>
#include <bitset>
using namespace std;

//位运算符
int main()
{
    // short int x = -178;
    // short int y = x << 1;                     // x左移一位, >>为右移操作
    // cout << bitset<sizeof(x) * 8>(x) << endl; //以补码形式输出x的所有位
    // cout << bitset<sizeof(y) * 8>(y) << endl;
    // cout << endl; //输出换行的方式之一

    // x = -111;
    // y = 1234;
    // short int z = x & y; //按位与操作
    // cout << bitset<sizeof(x) * 8>(x) << endl;
    // cout << bitset<sizeof(y) * 8>(y) << endl;
    // cout << bitset<sizeof(z) * 8>(z) << endl;

    //按位异或
    // short int x = -111;
    // short int y = 1234;
    // short int z = x ^ y; //按位异或操作
    // cout << bitset<sizeof(x) * 8>(x) << endl;
    // cout << bitset<sizeof(y) * 8>(y) << endl;
    // cout << bitset<sizeof(z) * 8>(z) << endl;

    short int x = -111;
    short int y = ~x; //按位取反操作
    cout << bitset<sizeof(x) * 8>(x) << endl;
    cout << bitset<sizeof(y) * 8>(y) << endl;
    return 0;
}
```

### 3 类型转换
> 注意：类型转换只体现在结果上，并不会改变被转换变量的类型。
```C++
#include <iostream>
using namespace std;

int main()
{
    //类型转换：数据位少的向数据位多的转换
    float a = 100.5;
    int b = a;
    cout << b << endl;
    cout << endl;

    unsigned int k = 1;
    int m = -2;
    long n = -2;
    cout << m + k << endl; //输出为4294967295,即int向unsigned int转换
    cout << n + k << endl;
    return 0;
}
```

```C++
#include <iostream>
using namespace std;

int main()
{
    short int a = 1;
    char b = 'x';
    cout << sizeof(a + b) << endl; //两者相加竟然向int转换
    return 0;
}
```

### 4 控制流-分支语句
```C++
switch(表达式){
	case 常量表达式1: 语句系列1;[break;]
	case 常量表达式2: 语句系列2;[break;]
	case 常量表达式3: 语句系列3;[break;]
	······
	case 常量表达式n-1: 语句系列n-1;[break;]
	case 常量表达式n: 语句系列n;[break;]
	default: 语句系列n+1;
}
```
> 如果不加break,就会导致后面的语句系列全部执行

### 5 控制流-循环语句
```C++
do{
	语句1;
	语句2;
	if(表达式)
		break;
	语句3;
	语句4;
}while(false);
```
> break只能放在循环中，通过这种方式可以跳过语句3和语句4.

### 6 函数与程序结构
```C++
#include<iostream>
using namespace std;
int main(){
	{
		int a = 0;
	}
	cout<<a<<endl;
	return 0;
}
```
报错信息如下
```shell
error: 'a' was not declared in this scope
     cout << a << endl;
```

```C++
#include <iostream>
using namespace std;
int main()
{
    int a = 10;
    {
        int a = 0;
        cout << "inside:" << a << endl;
    }
    cout << "outside:" << a << endl;
    return 0;
}
```

### 7 静态变量
```C++
#include <iostream>
using namespace std;
void f()
{
    static int a = 0;	//定义静态变量a，可类似与全局变量使用，递归时可用
    ++a;
    cout << a << endl;
}

int main()
{
    for (int i = 0; i <= 5; i++)
    {
        f();
    }
    return 0;
}
```

### 8 指向函数的指针
```C++
#include <iostream>
using namespace std;

int add(int a, int b)
{
    return a + b;
}

int minu(int a, int b)
{
    return a - b;
}

int main()
{
    int (*p)(int, int); //定义一个指向函数的指针
    char op = '+';
    if (op == '+')
    { //如果是加法，就让p指向加法那个函数
        p = add;
    }
    else
        p = minu; //否则就让p指向减法那个函数
    cout << p(3, 4) << endl;
    return 0;
}
```

### 9 一维数组
```C++
// char s1[] = "I am a student";//
// char *s = "I am a student";//这两句是不同的，这是一个字符串常量，只能读，不能写
```

```C++
#include <iostream>
using namespace std;

int main()
{
    char *s1 = "I am a student.";
    *s1 = 'S';
    cout << s1 << endl;
    return 0;
}

//报错信息如下
// warning: ISO C++ forbids converting a string constant to 'char*' [-Wwrite-strings]
//      char *s1 = "I am a student.";
```


//下面是一个指针数组
```C++
#include <iostream>
using namespace std;

int main()
{
    int a[5] = {1, 2, 3, 4, 5};
    int *p[5] = {a, a + 1, a + 2, a + 3, a + 4};
    for (int i = 0; i < 5; i++)
    {
        cout << *p[i] << " ";
    }
    cout << endl;
    return 0;
}
```

### 10 二维数组
```C++
#include <iostream>
using namespace std;

void array2D(int a[][3], int n)		//注意这里函数里面的形参
{
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < 3; j++)
        {
            cout << a[i][j] << "\t";
        }
        cout << endl;
    }
}

int main()
{
    int a[4][3] = {
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9},
        {10, 11, 12}};
    array2D(a, 4);//以这种方式来调用函数
    return 0;
}
```

### 11 二维字符串数组
```C++
#include <iostream>
using namespace std;
int main()
{
    char *s2D[] = {		//二维数组
        "I",
        "am",
        "a",
        "string"};
    for (int i = 0; i < 4; i++)
    {
        cout << s2D[i] << endl;
    }
    return 0;
}
//会有编译警告
//warning: ISO C++ forbids converting a string constant to 'char*' [-Wwrite-strings]
```

### 12 结构体（上）
```C++
#include <stdio.h>
int main()
{
    struct			//注意到这个类型太长了，这就要用到typedef了
    {
        int x;
        int y;
    } point;		//这样就定义了一个结构体，其中point为结构体名
    point.x = 10;
    point.y = 11;
    printf("%d, %d", point.x, point.y);
    return 0;
}

//用一下typedef
#include <stdio.h>
int main()
{
    typedef struct //注意到这个类型太长了，这就要用到typedef了
    {
        int x;
        int y;
    } Point; 
    Point point;
    point.x = 10;
    point.y = 11;
    printf("%d, %d", point.x, point.y);
    return 0;
}


//一般而言，会将结构体变量名放在函数外
#include <stdio.h>
typedef struct //注意到这个类型太长了，这就要用到typedef了
{
    int x;
    int y;
} Point;

int main()
{
    Point point;
    point.x = 10;
    point.y = 11;
    printf("%d, %d", point.x, point.y);
    return 0;
}

//还有这种方式
#include <stdio.h>
struct Point 
{
    int x;
    int y;
};

int main()
{
    struct Point point;	//此时定义变量需要用这种方式，还是有些长
    point.x = 10;
    point.y = 11;
    printf("%d, %d", point.x, point.y);
    return 0;
}
```

### 13 结构体（中）
```C++
//自引用结构
#include <stdio.h>
typedef struct Point
{
    int x;
    int y;
    struct Point *next;
} Point;
```

### 14 结构体（下）
> 这个结构体就已经包含C++下类的功能了，但还是有些区别
```C++
#include <iostream>
using namespace std;
struct Point
{
    int x; // C++下的结构体中的成员变量
    int y;
    Point *next; // C++下的结构体可以这样定义
    Point()
    { //构造函数（构造结构体的实体时，这个函数将会被自动执行）
        next = NULL;
        x = 0;
        y = 0;
    }
    void print(); // C++下的结构体可以包含成员函数，为了代码美观，一般这里只写声明
};

void Point::print() //这里需要结构体的名字加两个冒号(Point::print())
{
    cout << "(" << this->x << "," << this->y << ")" << endl; //注意这里用了this
}

int main()
{
    Point p1, p2, p3, p4, p5;
    Point *p;
    p1.x = 1;	p1.y = 0;
    p2.x = 4;	p2.y = 1;
    p3.x = 2;	p3.y = 4;
    p4.x = 3;	p4.y = 2;
    p5.x = 1;	p5.y = 6;

    p1.next = &p2; //注意这里是用.而不是用->
    p2.next = &p3;
    p3.next = &p4;
    p4.next = &p5;
    p5.next = NULL;

    for (p = &p1; p != NULL; p = p->next)
    {
        p->print(); //注意这里调用函数用的是->
    }
    return 0;
}
```

### 15 类（上）
```C++
#include <iostream>
using namespace std;
class Point //类这里要改为class
{
public:    //如果想直接通过类的实体（类变量）直接取到类的分量的话，需要将分量设置成public型。例如这里，加上public关键字
    int x; // C++下的结构体中的成员变量
    int y;
    Point *next; // C++下的结构体可以这样定义
    Point()
    { //构造函数（构造结构体的实体时，这个函数将会被自动执行）
        next = NULL;
        x = 0;
        y = 0;
    }
    void print(); // C++下的结构体可以包含成员函数，为了代码美观，一般这里只写声明
};

void Point::print() //这里需要结构体的名字加两个冒号(Point::print())
{
    cout << "(" << this->x << "," << this->y << ")" << endl; //注意这里用了this
}

int main()
{
    Point p1, p2, p3, p4, p5;
    Point *p;
    p1.x = 1;
    p1.y = 0;
    p2.x = 4;
    p2.y = 1;
    p3.x = 2;
    p3.y = 4;
    p4.x = 3;
    p4.y = 2;
    p5.x = 1;
    p5.y = 6;

    p1.next = &p2; //注意这里是用.而不是用->
    p2.next = &p3;
    p3.next = &p4;
    p4.next = &p5;
    p5.next = NULL;

    for (p = &p1; p != NULL; p = p->next)
    {
        p->print(); //注意这里调用函数用的是->
    }
    return 0;
}
```

### 16 类（中）
```C++
#include <iostream>
using namespace std;
class Point
{
public:
    Point()
    {
        x = 0;
        y = 0;
        next = NULL;
    }

private: //私有类型，不能在外面直接访问
    int x;
    int y;
    Point *next;
};

int main()
{
    Point P;
    P.x;
    return 0;
}

// 报错信息'int Point::x' is private within this context
//      P.x;
```

### 17 类（下）
> 访问私有属性值的方式
```C++
#include <iostream>
using namespace std;
class Point
{
public:
    Point()
    {
        this->x = 0;
        this->y = 0;
        this->next = NULL; //注意这里有分号
    }
    int getX();
    int getY();
    void inPutX(int x);
    void inPutY(int y);
    Point *getNext();
    void inPutNext(Point *next);

private:
    int x;
    int y;
    Point *next;
};
int Point::getX() { return this->x; }
int Point::getY() { return this->y; }
void Point::inPutX(int x) { this->x = x; }
void Point::inPutY(int y) { this->y = y; }
Point *Point::getNext() { return this->next; }
void Point::inPutNext(Point *next) { this->next = next; }	//把参数的值赋给this->next

int main()
{
    Point P1, P2;
    P1.inPutX(10);
    cout << P1.getX() << endl;
    cout << P1.getNext() << endl;
    P1.inPutNext(&P2);
    cout << P1.getNext() << endl;
    return 0;
}
```

> 类的空间分配
```C++
#include <iostream>
using namespace std;
class Point
{
public:
    Point()
    {
        this->x = 0;
        this->y = 0;
        this->next = NULL; //注意这里有分号
    }
    int x;
    int y;
    Point *next;
};
int main()
{
    Point *P = new Point;       //关键词new,动态空间分配
    cout << P->next << "," << P->y << endl;
    return 0;
}
```

### 18 格式化输入输出
```C++
//scanf时候，缓冲区的问题
#include <stdio.h>
int main()
{
    int a;
    float b;
    char c;
    char str[10];
    scanf("%d", &a);
    scanf("%f", &b);
    getchar(); //用getchar()接收缓冲区内容,也可以用fflush(stdin);清空输入缓冲区
    scanf("%c", &c);
    scanf("%s",str);//这时候不需要清空输入缓冲区
    return 0;
}
```

### 19 cin、cout
```C++
#include <iostream>
using namespace std;
int main()
{
    int a;
    float b;
    char c;
    char str[10];

    std::cin >> a;  //如果不加using namespace std,此处必须要加std::,否则可加可不加
    cin >> b;
    cin >> c;//不需要清空缓冲区
    cin >> str;

    std::cout << "int var : " << a << endl;     //<<endl即换行
    cout << "float var : " << b << endl;
    cout << "char var : " << c << endl;
    cout << "string var : " << str << endl;
    return 0;
}
```

### 20 字符的输入输出
```C++
//输入一个字符：getchar()
//输出一个字符：putchar(字符变量或常量)
//getchar()和putchar()的效率优于scanf()和printf()
#include <iostream>
using namespace std;
int main()
{
    char ch;
    int i = 1;
    while (ch = getchar())      //这里也会出现输入缓冲区的问题
    {
        cout << i << ":" << (int)ch << endl;
        ++i;
    }
    return 0;
}

//输入“空格abc制表符d回车”,它会将回车也存进ch，直到输入缓冲区内无数据才再次开始等待
```

#### 字符输入输出要注意的问题
```C++
//输入一个数字n,接下来要输入n组数据，每组两个字符，每输入一组数据则输出这组数据，每组输出一行。
#include <iostream>
using namespace std;
int main()
{
    int n;
    char a, b;
    scanf("%d", &n);
    while (n > 0)
    {
        //fflush(stdin);可以用清空缓冲区解决，也可以在两个scanf()后面加getchar()解决
        scanf("%c%c", &a, &b);
        printf("%c%c\n", a, b);
        --n;
    }
    return 0;
}
```

```C++
//cin、cout和getchar、putchar的区别
int main(){
    char ch;
    int i = 1;
    while(true){
        cin>>ch;
        cout<<i<<":"<<(int)ch<<endl;
        ++i;
        if((int)ch == 10)
            i = 1;
    }
    return 0;
}

// 输入输出信息如下：
// abc d   f
// 1:97
// 2:98
// 3:99
// 4:100
// 5:102
```
> cin从键盘缓冲中取字符的时候会过滤掉空格、制表和回车，这些特殊字符不会传给ch

### 21 字符串的输入输出
```C++
//gets()不接收回车，而scanf以空格为当前串的输入结束标记，当cin用于接收字符串时和scanf一样以空格为当前串的输入结束标记
char s[maxSize];
gets(s);
scanf("%s", s);
```

```C++
#include <iostream>
using namespace std;
int main()
{
    string s;
    getline(cin, s);

    for (int i = 0; i < s.length(); ++i)
    {
        cout << i << ":" << (int)s[i] << endl;//如果s为string类型，则用cout<<s来输出串
    }
    return 0;
}
```

>gets、scanf和cin会自动在串尾补上一个'\0'字符作为字符串的结束标记
>用puts(s)、printf("%s",s)或cout>>s可以输出s串，事实上，这三个函数都是输出从第一个字符到'\0'之前的所有字符，并且puts会自动换行

cin的某些注意事项:([可以参考PAT刷题 1009 说反话 (20 分)(https://corner647.gitee.io/2022/03/05/PAT%E5%88%B7%E9%A2%98/#1009-%E8%AF%B4%E5%8F%8D%E8%AF%9D-20-%E5%88%86)])
1. `cin`是没有返回值，问题在于`>>`输入操作符，应该考虑的是它返回了什么
2. “`>>`”操作重载函数`istream& operator>>(istream&, T &);`的返回值，其中第二个参数由`cin>>`后续参数类型决定。其返回值类型为`istream&`类型，大多数情况下其返回值为`cin`本身（非0值），只有当遇到EOF输入时，返回值为0。所以会有这种`cin`连续读取的方法。当输入所有数据后，通过输入`EOF`的方法，可以退出`while(cin>>a)`这样的循环。
3. 输入`EOF`的方法，windows下输入`ctrl+z`, Linux下输入`ctrl+d`。
4. `cin`是不接收空格的，空格作为其分界符。当`cin`读取到空格时，`cin`会从缓冲区读出，但却不会被`cin`接收到字符串当中去。

### 22小问题
**const修饰指针（*）：**
```C++
const int* a = & [1]          //非常量数据的常量指针    指针常量
int const *a = & [2]          //非常量数据的常量指针     a is a pointer to the constant char variable
int* const a = & [3]          //常量数据的非常量指针指针常量 常量指针 a is a constant pointer to the (non-constant) char variable
const int* const a = & [4]    //常量数据的常量指针
```
可以参考《Effective c++》Item21上的做法，
如果const位于星号*的左侧，则const就是用来修饰指针所指向的变量，即指针指向为常量；
如果const位于星号的右侧，const就是修饰指针本身，即指针本身是常量。
因此，[1]和[2]的情况相同，都是指针所指向的内容为常量，这种情况下不允许对内容进行更改操作，如不能*a = 3 ；
[3]为指针本身是常量，而指针所指向的内容不是常量，这种情况下不能对指针本身进行更改操作，如a++是错误的；
[4]为指针本身和指向的内容均为常量。