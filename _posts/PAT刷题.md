---
title: PAT刷题
date: 2022-03-05 11:32:10
tags:
declare: true
toc: 1
---
[浙大PAT刷题](https://pintia.cn/problem-sets/994805260223102976/problems/type/7)
### [1001 害死人不偿命的(3n+1)猜想 (15 分)](https://pintia.cn/problem-sets/994805260223102976/problems/994805325918486528)
卡拉兹(Callatz)猜想：

对任何一个正整数 n，如果它是偶数，那么把它砍掉一半；如果它是奇数，那么把 (3n+1) 砍掉一半。这样一直反复砍下去，最后一定在某一步得到 n=1。卡拉兹在 1950 年的世界数学家大会上公布了这个猜想，传说当时耶鲁大学师生齐动员，拼命想证明这个貌似很傻很天真的命题，结果闹得学生们无心学业，一心只证 (3n+1)，以至于有人说这是一个阴谋，卡拉兹是在蓄意延缓美国数学界教学与科研的进展……<!--more-->

我们今天的题目不是证明卡拉兹猜想，而是对给定的任一不超过 1000 的正整数 n，简单地数一下，需要多少步（砍几下）才能得到 n=1？

**输入格式**：
每个测试输入包含 1 个测试用例，即给出正整数 n 的值。

**输出格式：**
输出从 n 计算到 1 需要的步数。

**输入样例**：
`3`

**输出样例**：
`5`

**代码**
```C
#include<stdio.h>

int main(){
    int n,count = 0;
    scanf("%d",&n);
    while(n!=1){
        if(n%2 == 0){
            n = n/2;
        }
        else{
            n = (3*n+1)/2;
        }
        count++;
    }
    printf("%d\n",count);
    return 0;
}
```

### [1002 写出这个数 (20 分)](https://pintia.cn/problem-sets/994805260223102976/problems/994805324509200384)
读入一个正整数 n，计算其各位数字之和，用汉语拼音写出和的每一位数字。

**输入格式**：
每个测试输入包含 1 个测试用例，即给出自然数 n 的值。这里保证 n 小于 10^100

**输出格式**：
在一行内输出 n 的各位数字之和的每一位，拼音数字间有 1 空格，但一行中最后一个拼音数字后没有空格。

**输入样例**：
`1234567890987654321123456789`

**输出样例**：
`yi san wu`

**代码**
```C
#include<stdio.h>
#include<string.h>

int main(){
    char n[108] = "0";
    scanf("%s",n);          //用字符串接收数字
    int len;                 //用于计算字符串的长度
    len = strlen(n);
    int num = 0,temp;                 //用于计算各位之和
    for(int i = 0;i < len;i++){
        temp = (int)(n[i] - 48);        //强制类型转换
        num = num + temp;
    }
    //printf("%d\n",num);
    char str[10][5] =  {"ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu"};   //定义字符串数组
    //printf("%s",str[1]);
    if(num<10){                         //num绝对不可能大于900
        printf("%s",str[num]);
    }
    else if(num<100){
        printf("%s %s",str[num/10],str[num%10]);
    }
    else{
        temp = num/100;
        num = num - temp * 100;
        printf("%s %s %s",str[temp],str[num/10],str[num%10]);
    } 

    return 0;
}
```

### [1004 成绩排名 (20 分)](https://pintia.cn/problem-sets/994805260223102976/problems/994805321640296448)
读入 n（>0）名学生的姓名、学号、成绩，分别输出成绩最高和成绩最低学生的姓名和学号。

**输入格式**：
每个测试输入包含 1 个测试用例，格式为

>第 1 行：正整数 n
>第 2 行：第 1 个学生的姓名 学号 成绩
>第 3 行：第 2 个学生的姓名 学号 成绩
>  ... ... ...
>第 n+1 行：第 n 个学生的姓名 学号 成绩

其中`姓名`和`学号`均为不超过 10 个字符的字符串，成绩为 0 到 100 之间的一个整数，这里保证在一组测试用例中没有两个学生的成绩是相同的。

**输出格式**：
对每个测试用例输出 2 行，第 1 行是成绩最高学生的姓名和学号，第 2 行是成绩最低学生的姓名和学号，字符串间有 1 空格。

**输入样例**：
```C++
3
Joe Math990112 89
Mike CS991301 100
Mary EE990830 95
```

**输出样例**：
```C++
Mike CS991301
Joe Math990112
```


**代码**
```C++
//用结构体来存储每位学生的信息，用一个结构体指针来指向一个结构体数组。
//用find函数来找出结构体数组中成绩最低和最高的结构体

#include <stdio.h>
#include <stdlib.h> //要用到malloc函数
#include <iostream>
using namespace std;

typedef struct stduent
{
    char name[11];//注意题意，这里要用10以上
    char num[11];
    int score;
} stu;

void find(int &min_i, int &max_j, stu *p, int len) //找出成绩最高和最低的学生
{
    for (int i = 0; i < len; i++)
    {
        if (p[i].score < p[min_i].score)
        {
            min_i = i;
        }
        if (p[i].score > p[max_j].score)
        {
            max_j = i;
        }
    }
}

int main()
{
    int n;
    scanf("%d", &n);
    stu *p = (stu *)malloc(sizeof(stu) * n);
    for (int i = 0; i < n; i++)
    {
        scanf("%s%s%d", p[i].name, p[i].num, &(p[i].score));
    }
    int min_i = 0, max_j = 0;
    find(min_i, max_j, p, n);
    cout << p[max_j].name << ' ' << p[max_j].num << endl;
    cout << p[min_i].name << ' ' << p[min_i].num << endl;
    return 0;
}
```

```C++
下面是优化之后的代码
#include <iostream>
using namespace std;

struct student
{
    string name, num;//不需要指定长度
    int score;
};

int main()
{
    int n;
    cin >> n;
    student temp;
    student max = {"", "", 0};   //最大值最起码要设为最小的
    student min = {"", "", 100}; //最小值最起码要设为最大的
    while (n--)
    {
        cin >> temp.name >> temp.num >> temp.score;
        max = temp.score >= max.score ? temp : max;
        min = temp.score <= min.score ? temp : min;
    }

    cout << max.name << ' ' << max.num << endl;
    cout << min.name << ' ' << min.num << endl;
    return 0;
}
```

### [1005 继续(3n+1)猜想 (25 分)](https://pintia.cn/problem-sets/994805260223102976/problems/994805320306507776)

卡拉兹(Callatz)猜想已经在[1001](https://pintia.cn/problem-sets/994805260223102976/problems/994805325918486528)中给出了描述。在这个题目里，情况稍微有些复杂。

当我们验证卡拉兹猜想的时候，为了避免重复计算，可以记录下递推过程中遇到的每一个数。例如对 n=3 进行验证的时候，我们需要计算 3、5、8、4、2、1，则当我们对 n=5、8、4、2 进行验证的时候，就可以直接判定卡拉兹猜想的真伪，而不需要重复计算，因为这 4 个数已经在验证3的时候遇到过了，我们称 5、8、4、2 是被 3“覆盖”的数。我们称一个数列中的某个数 n 为“关键数”，如果 n 不能被数列中的其他数字所覆盖。

现在给定一系列待验证的数字，我们只需要验证其中的几个关键数，就可以不必再重复验证余下的数字。你的任务就是找出这些关键数字，并按从大到小的顺序输出它们。

**输入格式**：
每个测试输入包含 1 个测试用例，第 1 行给出一个正整数 K (<100)，第 2 行给出 K 个互不相同的待验证的正整数 n (1<n≤100)的值，数字间用空格隔开。

**输出格式**：
每个测试用例的输出占一行，按从大到小的顺序输出关键数字。数字间用 1 个空格隔开，但一行中最后一个数字后没有空格。

**输入样例**：

```C++
6
3 5 6 7 8 11
```

**输出样例**：
`7 6`

**代码**
基本思想:对每一个输入的数字i进行验证，把验证过的数字对应的b[i]标记为1，所有b[i]=-1的数字即为关键数字,所有b[i] = 0的数字代表本次测试未曾用到。最后对关键数字数字从大到小进行输出。

b的数组长度必须要设定的很大，因为3n+1时候可能会出现数组越界的情况

```C++
#include <iostream>
using namespace std;

int main()
{
    int b[10000] = {0}; //用桶排序的思想存储标记,为防止数组越界，这里设定数组长度为10000
    int K;
    cin >> K;
    int *a = (int *)malloc(sizeof(int) * K); //定义一个长度为K的数组，用于暂时存储输入的数据
    for (int i = 0; i < K; i++)
    {
        cin >> a[i];
    }

    for (int i = 0; i < K; i++) //把输入的数据的标志位设为-1，-1代表未被覆盖，1代表被覆盖
    {
        b[a[i]] = -1;
    }

    for (int i = 0; i < K; i++)//对每一个输入的数字i进行验证
    {
        if (b[a[i]] == 1)
            continue;
        while (a[i] != 1)
        {
            if (a[i] % 2 == 0)
            {
                a[i] = a[i] / 2;
                b[a[i]] = 1;
            }
            else
            {
                a[i] = (3 * a[i] + 1) / 2;
                b[a[i]] = 1;
            }
        }
    }
    free(a);
    int flag = 1;
    for (int i = 100; i > 0; i--)
    { //保证每个测试用例的输出占一行，按从大到小的顺序输出关键数字。数字间用 1 个空格隔开，但一行中最后一个数字后没有空格。
        if (b[i] == -1 && flag == 1)
        {
            cout << i;
            flag++;
            continue;
        }
        if (b[i] == -1 && flag > 1)
        {
            cout << ' ' << i;
        }
    }
    return 0;
}
```
这里给出[柳婼 の blog姐姐](https://www.liuchuo.net/archives/452)的解法，其中[向量(vector)](https://doc.bccnsoft.com/docs/cppreference/cppvector.html)一个封装了动态大小数组的顺序容器（Sequence Container）。跟任意其它类型容器一样，它能够存放各种类型的对象。可以简单的认为，向量是一个能够存放任意类型的动态数组。

[sort](http://www.cplusplus.com/reference/algorithm/sort/)是库algorithm中的一个函数

**本题重点就是一方面是想到桶排序的思想，另一方面是注意到b的数组长度需要很大。**

### [1006 换个格式输出整数 (15 分)](https://pintia.cn/problem-sets/994805260223102976/problems/994805318855278592)
让我们用字母 B 来表示“百”、字母 S 表示“十”，用 12...n 来表示不为零的个位数字 n（<10），换个格式来输出任一个不超过 3 位的正整数。例如 234 应该被输出为 BBSSS1234，因为它有 2 个“百”、3 个“十”、以及个位的 4。

**输入格式：**
每个测试输入包含 1 个测试用例，给出正整数 n（<1000）。

**输出格式：**
每个测试用例的输出占一行，用规定的格式输出 n。

**输入样例 1：**
`234`

**输出样例 1：**
`BBSSS1234`

**输入样例 2：**
`23`

**输出样例 2：**
`SS123`

**代码**
过于简单，不加详述
```C++
#include <iostream>
using namespace std;

int main()
{
    int num;
    cin >> num;
    for (int i = 0; i < num / 100; i++)
    {
        cout << 'B';
    }
    for (int i = 0; i < (num - num / 100 * 100) / 10; i++)
    {
        cout << 'S';
    }
    for (int i = 1; i <= (num % 10); i++)
    {
        cout << i;
    }
    return 0;
}
```

### [1007 素数对猜想 (20 分)](https://pintia.cn/problem-sets/994805260223102976/problems/994805317546655744)
让我们定义$d_n = p_{n+1} - p_n$，其中$p_i$是第i个素数。显然有$d_1 = 1$，且对于n>1有$d_n$是偶数。“素数对猜想”认为“存在无穷多对相邻且差为2的素数”。现给定任意正整数`N`($<10^5$)，请计算不超过`N`的满足猜想的素数对的个数。

**输入格式:**
输入在一行给出正整数`N`。

**输出格式:**
在一行中输出不超过`N`的满足猜想的素数对的个数。

**输入样例:**
`20`
**输出样例:**
`4`

**代码**
```C++
#include <iostream>
using namespace std;

bool is_prime(int n)//判断是否是素数
{
    for (int i = 2; i * i <= n; i++)//注意i的条件
    {
        if (n % i == 0)
        {
            return false;
        }
    }
    return true;
}

int main()
{
    int N;
    cin >> N;
    if (N < 5)//如果N小于5，没有满足条件的素数对
    {
        cout << 0;
        return 0;
    }
    int count = 0;
    for (int i = 5; i <= N; i++)
    {
        if (is_prime(i) && is_prime(i - 2))//判断存在多少个满足条件的素数对
        {
            count++;
        }
    }
    cout << count;
    return 0;
}
```

### [1008 数组元素循环右移问题 (20 分)](https://pintia.cn/problem-sets/994805260223102976/problems/994805316250615808)
一个数组A中存有N（>0）个整数，在不允许使用另外数组的前提下，将每个整数循环向右移M（≥0）个位置，即将A中的数据由($A_0 A_1 ⋯ A_{N−1}$)变换为($A_{N−M} ⋯ A_{N−1} A_0 A_1 ⋯ A_{N−M−1}$)（最后M个数循环移至最前面的M个位置）。如果需要考虑程序移动数据的次数尽量少，要如何设计移动的方法？

**输入格式:**
每个输入包含一个测试用例，第1行输入N（1≤N≤100）和M（≥0）；第2行输入N个整数，之间用空格分隔。

**输出格式:**
在一行中输出循环右移M位以后的整数序列，之间用空格分隔，序列结尾不能有多余空格。

**输入样例:**
```C++
6 2
1 2 3 4 5 6
```

**输出样例:**
`5 6 1 2 3 4`

**代码**
```C++
//分析:数组的循环右移，可以用逆序数组的组合来实现，因为数组长度最大只有100，所有可以直接使用顺序表进行操作
//只要保证如果右移位数的大小超出了数组的长度，不能出现数组越界的情况

#include <iostream>
using namespace std;

#define MaxSize 120 //定义线性表的最大长度
typedef int ElemType;

typedef struct //用顺序表定义数组
{
    ElemType data[MaxSize];
    int length;
} SqList;

void reverse(SqList &L, int left, int right)
{ //对线性表L的left到right这一部分进行逆序
    for (ElemType temp; left < right; left++, right--)
    {
        temp = L.data[left];
        L.data[left] = L.data[right];
        L.data[right] = temp;
    }
}

void right_move(SqList &L, int M) //将线性表L循环右移M位
{
    reverse(L, 0, L.length - M - 1);
    reverse(L, L.length - M, L.length - 1);
    reverse(L, 0, L.length - 1);
}

int main()
{
    SqList L;
    cin >> L.length; //确定数组长度
    int M;           //将数组循环右移M位
    cin >> M;
    for (int i = 0; i < L.length; i++) //输入数组数据
    {
        cin >> L.data[i];
    }
    // if(L.length == 1){
    //     cout<<L.data[0];
    //     return 0;
    // }
    right_move(L, M % L.length); //如果右移位数的大小超出了数组的长度，要保证不出现数组越界的情况
    int flag = 0;
    for (int i = 0; i < L.length; i++) //保证在一行中输出循环右移M位以后的整数序列，之间用空格分隔，序列结尾不能有多余空格。
    {
        if (flag)
        {
            cout << ' ' << L.data[i];
        }
        else
        {
            cout << L.data[i];
            flag++;
        }
    }
    return 0;
}
```

### [1009 说反话 (20 分)](https://pintia.cn/problem-sets/994805260223102976/problems/994805314941992960)
给定一句英语，要求你编写程序，将句中所有单词的顺序颠倒输出。

**输入格式：**
测试输入包含一个测试用例，在一行内给出总长度不超过 80 的字符串。字符串由若干单词和若干空格组成，其中单词是由英文字母（大小写有区分）组成的字符串，单词之间用 1 个空格分开，输入保证句子末尾没有多余的空格。

**输出格式：**
每个测试用例的输出占一行，输出倒序后的句子。

**输入样例：**
`Hello World Here I Come`

**输出样例：**
`Come I Here World Hello`

**代码**
```C++
//分析，定义一个字符数组用于存储字符串，用游标i指向数组的最后一个字符，依次向前遍历，当遇到空格或者起点时，将刚才遍历的字符从前向后///用游标j取出。
//需要注意的是，PAT的OJ系统需要用fgets,而不能用gets

#include <iostream>
using namespace std;
#include <string.h>

int main()
{
    char a[100];
    // cin >> a; //这里用cin或者scanf会出现无法读取' '之后的内容
    fgets(a, 100, stdin);//如果用gets()，OJ会出现编译错误,stdin代表读取输入缓冲区的值
    int len, i;
    len = strlen(a) - 1;//fgets会接收回车，所以长度要长1
    for (i = len - 1; i > 0; i--)
    {
        if (a[i] == ' ')
        {
            for (int j = i + 1; a[j] != ' ' && a[j] != '\n'; j++)
            {
                cout << a[j];
            }
            cout << ' ';
        }
    }
    if (i == 0)
        for (; a[i] != ' ' && a[i] != '\n'; i++)
        {
            cout << a[i];
        }
    return 0;
}
```
这里给出[柳婼 の blog姐姐](https://www.liuchuo.net/archives/524)的解法，用到了[栈](https://doc.bccnsoft.com/docs/cppreference/cppstack.html)，使问题简化,但用的cin，**不理解为什么可以通过。**
**这里对于这个问题做出解答。**
1. `cin`是没有返回值，问题在于`>>`输入操作符，应该考虑的是它返回了什么
2. “`>>`”操作重载函数`istream& operator>>(istream&, T &);`的返回值，其中第二个参数由`cin>>`后续参数类型决定。其返回值类型为`istream&`类型，大多数情况下其返回值为`cin`本身（非0值），只有当遇到EOF输入时，返回值为0。所以会有这种`cin`连续读取的方法。当输入所有数据后，通过输入`EOF`的方法，可以退出`while(cin>>a)`这样的循环。
3. 输入`EOF`的方法，windows下输入`ctrl+z`, Linux下输入`ctrl+d`。
4. `cin`是不接收空格的，空格作为其分界符。当`cin`读取到空格时，`cin`会从缓冲区读出，但却不会被`cin`接收到字符串当中去。

### [1010 一元多项式求导 (25 分)](https://pintia.cn/problem-sets/994805260223102976/problems/994805313708867584)
设计函数求一元多项式的导数。(注：$x_n$(n为整数)的一阶导数为$nx_{n−1}$。)

**输入格式:**
以指数递降方式输入多项式**非零项系数**和指数（绝对值均为不超过 1000 的整数）。数字间以空格分隔。

**输出格式:**
以与输入相同的格式输出导数多项式非零项的系数和指数。数字间以空格分隔，但结尾不能有多余空格。注意“零多项式”的指数和系数都是 0，但是表示为 `0 0`。

**输入样例:**
`3 4 -5 2 6 1 -2 0`

**输出样例:**
`12 3 -10 1 6 0`

**代码**
```C++
//此题比较简单，只需注意当输入是常数多项式的情况即可，还要注意给出的是所有非零项系数
#include <iostream>
using namespace std;
#define MaxSize 1000

typedef struct
{
    int data[MaxSize];
    int length = 0;
} SqList;

int main()
{
    SqList L;
    while (cin >> L.data[L.length]) //将输入的数据存入顺序表中
    {
        L.length++;
    }
    if (L.data[1] == 0) //如果输入是常数多项式
    {
        cout << "0 0";
        return 0;
    }
    int flag = 0;
    for (int i = 0; i < L.length - 1; i = i + 2)
    {
        if (L.data[i] != 0 && L.data[i + 1] != 0)
        {
            if (flag != 0)
            {
                cout << ' ' << L.data[i] * L.data[i + 1] << ' ' << L.data[i + 1] - 1;
            }
            else
            {
                cout << L.data[i] * L.data[i + 1] << ' ' << L.data[i + 1] - 1;
                flag++;
            }
        }
    }
    return 0;
}
```

下面给出**[柳婼 の blog姐姐](https://www.liuchuo.net/archives/526)**的解法，不需要数组存储，节省了空间。
```C++
#include <iostream>
using namespace std;

int main()
{
    int a, b, flag = 0;//flag用来判断是否已经有过输出
    while (cin >> a >> b)
    {
        if (b != 0)
        {//当b!=0时，因为给出的是所有非零项系数，所以必定会有输出，先判断flag是否为1，如果为1表示已经有过输出，那么在前面要先输出一个空格
            if (flag != 0)
            {
                cout << ' ';
            }
            cout << a * b << ' ' << b - 1;//输出 a * b 和 b – 1，然后将flag标记为1表示已经有过输出
            flag = 1;
        }
    }
    if (flag == 0)//当没有输出并且b==0的时候，输出“0 0”
    {
        cout << "0 0";
    }
    return 0;
}
```

### [1011 A+B 和 C (15 分)](https://pintia.cn/problem-sets/994805260223102976/problems/994805312417021952)
给定区间$[{−2}^{31},2^{31}]$内的3个整数$A、B$和$C$，请判断$A+B$是否大于$C$。

**输入格式：**
输入第 1 行给出正整数 T (≤10)，是测试用例的个数。随后给出 T 组测试用例，每组占一行，顺序给出 A、B 和 C。整数间以空格分隔。

**输出格式：**
对每组测试用例，在一行中输出 Case #X: true 如果 A+B>C，否则输出 Case #X: false，其中 X 是测试用例的编号（从 1 开始）。

**输入样例：**
```C++
4
1 2 3
2 3 4
2147483647 0 2147483646
0 -2147483648 -2147483647
```

**输出样例：**
```C++
Case #1: false
Case #2: true
Case #3: true
Case #4: false
```

**代码**
```C++
//分析:可以用队列来暂存需要用到的数字，需要注意的是，要用long int
#include <iostream>
#include <queue>
using namespace std;

int main()
{
    queue<long int> q; //定义一个队列
    long int T, a, b, c;
    cin >> T; //输入测试用例的个数
    while (cin >> a >> b >> c)
    {
        q.push(a);
        q.push(b);
        q.push(c);
        // i++;
        // if (i == T)//这里可以用来代替循环终止条件
        // {
        //     break;
        // }
    }
    int i = 1; // i代替X,用作测试用例的编号（从 1 开始）
    while (!q.empty())
    {
        a = q.front();
        q.pop();
        b = q.front();
        q.pop();
        c = q.front();
        q.pop();
        if (!q.empty()) //如果不是最后一个输出，则输出换行，否则不输出
            cout << "Case #" << i << ((a + b) > c ? ": true" : ": false") << endl;
        else
            cout << "Case #" << i << ((a + b) > c ? ": true" : ": false");
        i++;
    }
    return 0;
}
```

下面给出[柳婼 の blog姐姐](https://www.liuchuo.net/archives/822)的解法，和我一开始所想一致，但是担心控制台误判，未曾采用。

### [1012 数字分类 (20 分)](https://pintia.cn/problem-sets/994805260223102976/problems/994805311146147840)
给定一系列正整数，请按要求对数字进行分类，并输出以下 5 个数字：
 - $A_1$ = 能被 5 整除的数字中所有偶数的和；
 - $A_2$ = 将被 5 除后余 1 的数字按给出顺序进行交错求和，即计算 $n_1−n_2+n_3−n_4⋯$；
 - $A_3$ = 被 5 除后余 2 的数字的个数；
 - $A_4$ = 被 5 除后余 3 的数字的平均数，精确到小数点后 1 位；
 - $A_5$ = 被 5 除后余 4 的数字中最大数字。

**输入格式：**
每个输入包含 1 个测试用例。每个测试用例先给出一个不超过 1000 的正整数 N，随后给出 N 个不超过 1000 的待分类的正整数。数字间以空格分隔。

**输出格式：**
对给定的 $N$ 个正整数，按题目要求计算 $A_1$~$A_5$并在一行中顺序输出。数字间以空格分隔，但行末不得有多余空格。
若分类之后某一类不存在数字，则在相应位置输出 `N`。

**输入样例 1：**
`13 1 2 3 4 5 6 7 8 9 10 20 16 18`

**输出样例 1：**
`30 11 2 9.7 9`

**输入样例 2：**
`8 1 2 4 5 6 7 9 16`

**输出样例 2：**
`N 11 2 N 9`

**代码**
```C++
#include <iostream>
#include <stdio.h>
using namespace std;
int main()
{
    int N;
    cin >> N; //正整数的个数
    int temp, count = 0, num = 0;
    int k = 0;
    int a[5] = {0, 0, 0, 0, 0}; //用来存储题意上的A1~A5
    while (cin >> temp)
    {
        if (temp % 10 == 0)
        {
            a[0] += temp;
        }
        else if (temp % 5 == 1)
        { //用k来代替(-1)^k
            if (k == 0)
            {
                a[1] += temp;
                k = 1;
                num++;
            }
            else
            {
                a[1] -= temp;
                k = 0;
            }
        }
        else if (temp % 5 == 2)
        {
            a[2]++;
        }
        else if (temp % 5 == 3)
        {
            a[3] += temp;
            count++;
        }
        else if (temp % 5 == 4)
        {
            a[4] = (temp >= a[4] ? temp : a[4]);
        }
    }
    if (a[0])
    {
        cout << a[0] << ' ';
    }
    else
        cout << 'N' << ' ';
    if (num)
    {
        cout << a[1] << ' ';
    }
    else
        cout << 'N' << ' ';
    if (a[2])
    {
        cout << a[2] << ' ';
    }
    else
        cout << 'N' << ' ';
    if (a[3])
    {
        printf("%.1f ", (float)a[3] / count);
    }
    else
        cout << 'N' << ' ';
    if (a[4])
    {
        cout << a[4];
    }
    else
        cout << 'N';

    return 0;
}
```

下面给出[柳婼 の blog姐姐](https://www.liuchuo.net/archives/528)的解法，只是她用到了类似数组的[C++ Vectors](https://doc.bccnsoft.com/docs/cppreference/cppvector.html)，占用了更大的空间。

### [1013 数素数 (20 分)](https://pintia.cn/problem-sets/994805260223102976/problems/994805309963354112)
令 $P_i$表示第 i 个素数。现任给两个正整数 $M≤N≤10^4$，请输出 $P_M$到 $P_N$的所有素数。

**输入格式：**
输入在一行中给出 $M$ 和 $N$，其间以空格分隔。

**输出格式：**
输出从 $P_M$到 $P_N$的所有素数，每 10 个数字占 1 行，其间以空格分隔，但行末不得有多余空格。

**输入样例：**
`5 27`

**输出样例：**
```C++
11 13 17 19 23 29 31 37 41 43
47 53 59 61 67 71 73 79 83 89
97 101 103
```

**代码**
```C++
#include <iostream>
using namespace std;

bool is_prime(int x)//判断x是否是素数
{
    for (int i = 2; i * i <= x; i++)
    {
        if (x % i == 0)
        {
            return false;
        }
    }
    return true;
}

int main()
{
    int m, n, k = 0;//k用来保证空格和十个之后换行
    int count = 0;//count用于记录当前i是第几个素数
    cin >> m >> n;//m用来指出第m个素数，n同理
    for (int i = 1; count <= n; i++)
    {
        if (is_prime(i) && count < m)
        {
            count++;
        }
        else if (is_prime(i) && count >= m)
        {
            if (k % 10 == 0)
            {
                cout << i;
            }
            else
            {
                cout << ' ' << i;
            }
            count++;
            k++;
            if (k % 10 == 0)
            {
                cout << endl;
            }
        }
    }
    return 0;
}
```

下面给出[柳婼 の blog姐姐](https://www.liuchuo.net/archives/530)的解法，只是她用到了类似数组的[C++ Vectors](https://doc.bccnsoft.com/docs/cppreference/cppvector.html)，占用了更大的空间

### [1014 福尔摩斯的约会 (20 分)](https://pintia.cn/problem-sets/994805260223102976/problems/994805308755394560)
大侦探福尔摩斯接到一张奇怪的字条：

```C
我们约会吧！ 
3485djDkxh4hhGE 
2984akDfkkkkggEdsb 
s&hgsfdk 
d&Hyscvnm
```

大侦探很快就明白了，字条上奇怪的乱码实际上就是约会的时间`星期四 14:04`，因为前面两字符串中第 1 对相同的大写英文字母（大小写有区分）是第 4 个字母 `D`，代表星期四；第 2 对相同的字符是 `E` ，那是第 5 个英文字母，代表一天里的第 14 个钟头（于是一天的 0 点到 23 点由数字 0 到 9、以及大写字母 `A` 到 `N` 表示）；后面两字符串第 1 对相同的英文字母 `s` 出现在第 4 个位置（从 0 开始计数）上，代表第 4 分钟。现给定两对字符串，请帮助福尔摩斯解码得到约会的时间。

**输入格式：**
输入在 4 行中分别给出 4 个非空、不包含空格、且长度不超过 60 的字符串。

**输出格式：**
在一行中输出约会的时间，格式为 `DAY HH:MM`，其中 `DAY` 是某星期的 3 字符缩写，即 `MON` 表示星期一，`TUE` 表示星期二，`WED` 表示星期三，`THU` 表示星期四，`FRI` 表示星期五，`SAT` 表示星期六，`SUN` 表示星期日。题目输入保证每个测试存在唯一解。

**输入样例：**
```C
3485djDkxh4hhGE 
2984akDfkkkkggEdsb 
s&hgsfdk 
d&Hyscvnm
```

**输出样例：**
`THU 14:04`

**代码**
```C++

```