---
title: 【转载】GCC自带位运算系列函数
date: 2018-08-12 21:52:08
tags: 
  - 洛谷日报
---


------------
# GCC自带位运算系列函数
------------

> 声明：本文为转载文章，转载自[洛谷日报#26](https://www.luogu.org/blog/Ilovehimforever/gcc-hei-ke-ji-zhi-builtin-ji-lie-han-shuo)，原作者为I_love_him52。

------------
谈到GCC的黑科技，大家想到的一定是这句：
```cpp
#pragma GCC optimize (3)//吸氧
```
抑或是这句：
```cpp
#pragma GCC diagnostic error "-std=c++11"//C++11
```
然而又有多少人知道`__builtin_xxx()`这群神奇的存在？

举个栗子：树状数组的核心思想就是一个叫做`lowbit()`的函数，它是这样写的：
```cpp
inline int lowbit(const int &x)
{
    return x & -x;
}
```
什么，你说长？你嫌慢？
```cpp
#define lowbit(x) ((x) & -(x))
```
什么，你还是不想自己写？非得用内置函数？

那么恭喜你，这是你新的出路：
```cpp
lowbit(x) == (1 << __builtin_ctz(x))
```
当然，对于`lowbit()`函数，大家看不到什么好处（喜欢这样用的才有问题吧(-_-)）

再举一个栗子：[P2704](https://www.luogu.org/problemnew/show/P2704)， 一道状压DP入门题目，它要求输出：

仅一行，包含一个整数K，表示最多能摆放的炮兵部队的数量。

那么，DP的时候，对于每一行的状态，若它在二进制下的第`i`位以`0`表示不放，以`1`表示放的话，我们就得统计它二进制下`1`的个数，于是考虑用一个`inline`的函数`count()`来统计：
```cpp
inline int count(int x)//摘自本人AC代码
{
    int ret = 0;
    while(x)
    {
        ret += x & 1;
        x >>= 1;
    }
    return ret;
}
```
统计一下，这里一共有```135```字节的内容。

使用它的代码长这样：`sum[tot] = count(i);`，改一下，变成这样：`sum[tot] = __builtin_popcount(i);`

有人又要钻牛角尖：就算你把函数定义的码量给省了，但是你每次调用函数都会增加`strlen("__builtin_pop") == 13`字节的码量啊？

那还不简单`define`下就好了

除了上述函数，本人另外还整理了一部分用得到的`__builtin_`系列函数：

`__builtin_ffs(x)` 返回`x`的二进制下第一位`1`的位置（从`1`开始）
`__builtin_clz(x) `返回`x`二进制下最高有效位到最高位的`1`上一位的长度（即最高位开始连续`0`的个数）
`__builtin_ctz(x)` 与上一个函数相反，返回`x`的二进制下最低位开始连续`0`的个数（即第一个函数 - 1）
`__builtin_parity(x) `返回`x`二进制下`1`的个数的奇偶性
`__builtin_popcount(x) `返回`x`二进制下`1`的个数
另外以上函数的唯一参数都为`unsigned int`类型，并且都有`unsigned long long`版本，即在函数名后面加上`ll`，Like` __builtin_popcountll(x)`。

对于其他的`__builtin_`系列函数，可以自行查阅`GNU C`所提供的文档

又双叒叕及：感谢[@ComeIntoPower](https://www.luogu.org/space/show?uid=11751) 管理员大大普及：可以在程序开头加入这样一行：
```cpp
#pragma GCC target ("popcnt")
```
根据大大的说法，这条`GCC`指令可以让`__builtin_popcount`被编译器识别为一条指令。

什么用呢？就是加速！时 间 减 半！它本身就够快了，还可以加速%%%，鄙人真是孤陋寡闻。

注意：有些计算机可能不支持`popcnt`指令，然后`GCC`就会`GG`。（大部分计算机都有）

还有一点注意，有一种说法是NOI系列赛事中禁止使用以下划线开头的函数，因此在NOI系列比赛中使用有风险，这只是给平时做题提供一些便利

此外，既然是`黑科技`自然有地方不给用，我已经测试了各大`OJ`对于该系列函数的支持情况：（务必选用`G++/GCC`作为编译器）

洛谷: OJBK

Poj: OJBK

Lydsy: OJBK

Hdu: A+B 莫名WA，但是编译应该OJBK

CodeVS: OJBK

Vijos: OJBK

Uoj: OJBK

Codeforces: OJBK

JoyOI: 评测机炸了，待更新

ContestHunter: OJBK

Zoj: 编译过了，也是莫名WA

SPOJ: OJBK

UVa: OJBK

AtCoder: OJBK

LibreOJ: OJBK

比赛: 这个请问官方

...待补充