---
title: 【转载】C++中偷懒利器——宏
date: 2018-09-12 21:31:22	
tags: 
  - 洛谷日报
---


------------
# OI中可以用到的Linux基础教程
------------

> 声明：本文为转载文章，转载自[洛谷日报#NAN](https://www.luogu.org/blog/two-three-three/tou-lan-li-qi-hong)，原作者为木木小胖。

------------
提到C++宏，大多数人想到的就是宏函数和宏常量，如```#define MAXN 500```和```#define max(a,b) ((a)>(b)?(a):(b))```这种宏应用。

其实宏还有很多很多更能发挥它威力的应用。宏的用途可不仅限于```constexpr```。几乎任何有重复代码的地方都能用宏大幅度简化，从而节省工作量。

在继续学习之前，先了解一下C++的宏机制。

## C++的宏机制

![C++编译流程 图片来源于网络，侵权删。](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1534684146526&di=a6171b22cb2f4d3370d965160b7c582f&imgtype=0&src=http%3A%2F%2Ftxt25-2.book118.com%2F2017%2F0905%2Fbook132081%2F132080656.jpg)。

众所周知，C++的编译流程主要分为预处理、编译、汇编、链接等步骤，其中宏展开在预处理步骤中进行。预处理步骤主要处理预处理指令。（如宏所用的```#define```以及头文件用的```#include```）

因此，宏在预处理过程中就全部展开，这时候预处理器执行的只是简单的字符串替换。

具体到每个宏，预处理器若识别出一个符号为宏名，就执行宏展开。对于每一个宏名后面的括号里的内容，预处理器根据且只根据逗号分割参数(但是有一个例外，括号内的逗号不会被当作分割符)，也就是说你能写出这样的宏调用：```bxy(q.push, n, ;)```。

如果参数中含有宏，编译器不会在传入宏之前进行宏展开。如果展开式中含有宏，编译器会展开它。但是，如果展开式中含有宏调用且参数中含有宏，编译器会先展开参数中的宏。

上面一段文字太抽象，我就写一个样例示范一下。
```cpp
#define macro(x) (1+mmacro(x)) //由于这里专门讲宏所以就不把宏名全大写了
#define mmacro(x) (2+x)

#define macroexpand(x) x
#define expand(x) macroexpand(x)

expand(macro(1)) //macroexpand的参数将是(1+(2+1))
//展开过程如下：
//expand(macro(1))
//macroexpand(macro(1))
//macroexpand((1+mmacro(1)))
//macroexpand((1+(2+1)))
//(1+(2+1))
```
## 宏中的特殊符号

这些符号是宏独有的功能，其作用相当于直接沟通神灵。【能改变编译器看到的东西

### 井号（#）

单个井号表示将该参数左右加上双引号。

宏被人们所诟病的理由之一就是不能看到宏的展开式进行调试。实际上，只利用我们现在所学的知识，我们是能够看到宏的展开式的。

以下便是一个输出宏展开式的例子
```cpp
#define macroexpand(x) #x
#define expand(x) macroexpand(x)
//expand函数接受一个宏，返回一个字符串，字符串内容即其展开式
#define expand_andprint(x) printf("%s\n",macroexpand(x))
//expand_andprint函数接受一个宏，将其展开式输出
```
### 双井号（##）

双井号表示拼接左右两边的内容生成新的**合法字面常量或标识符**

比如说，我们可以用双井号生成标识符（即变量名）：
```cpp
#define connect(x,y) x##y
expand_andprint(connect(a,b))
//输出 ab
int connect(a,b)=3; //真的能过系列，展开为int ab=3
printf("%d",connect(a,b)); //输出3
```
但是这个看起来十分厉害的功能却没有多少实际应用。因为宏在预处理期就已经展开，宏所能执行的功能只是简单的字符串拼接，于是在大多数情况下，这个功能只是一种语法糖。

双井号还能连接生成数字常量
```cpp
#define oct(x) 0##x
#define hex(y) 0x##y

hex(7f7f7f7f) //真的能过系列，展开为0x7f7f7f7f
```
这个就更没用了【逃

### 井号-at号（#@）

#@表示将参数加上单引号

与单井号类似，就不多说了

注意，这是微软家编译器（VS）专用的符号，不是语言标准内容，在其他编译器上会报错

如果想用字符，可以使用```#x[0]```或者传入字符参数

## 应用

### 例1：switch-case

原代码：
```cpp
#include <cstdio>
int main()
{
    char c;
    int a,b;
    scanf("%d%c%d",&a,&c,&b);
    switch(c)
    {
        case '+':
            printf("%d\n",a+b);
            return 0;
        case '-':
            printf("%d\n",a-b);
            return 0;
        case '*':
            printf("%d\n",a*b);
            return 0;
        case '/':
            printf("%d\n",a/b);
            return 0; 
    }
    return 0;
}
```
加入宏之后
```cpp
#include <cstdio>
using namespace std;

#define charcase(ch,x) case ch: printf("%d\n", a x b); return 0;

int main()
{
    char c;
    int a,b;
    scanf("%d%c%d",&a,&c,&b);
    switch(c)
    {
        charcase('+',+);
        charcase('-',-);
        charcase('*',*);
        charcase('/',/);
    }
    return 0;
}
```
### 例2：双井号的使用

在BFS走迷宫的时候，经常遇到同样的代码片段出现两次，一次针对x一次针对y。那么有没有办法只写一次呢

原代码（代码片段）
```cpp
queue<int> xq;
queue<int> yq;

queue<int> q;
q.push(xb);
q.push(yb);
```
加入宏之后（等效代码片段）
```cpp
#define xy(a,sy,b) a x##sy b a y##sy b
#define bxy(a,sy,b) a ( x##sy ) b a ( y##sy ) b

xy(queue<int>, q, ;)

queue<int> q;
bxy(q.push, b, ;)
```
宏xy将参数中三部分连接在一起并重复两遍，第一遍中间一个参数前面连接上x，第二遍则连接上y。

宏bxy在中间参数左右加上括号。

## 拓展：宏和lambda表达式

观看本节前，建议阅读参考文献中的《编程利器-lambda表达式》。

lambda表达式是闭包的基础，同时也是函数式编程的基础。来个最贴近生活的应用。~~（来源于《编程利器-lambda表达式》）~~

假设有一道毒瘤题，让你定义一个结构体people，然后先根据age字段排序，然后再根据chengji字段排序，最后根据RP字段排序。使用lambda表达式，我们可以免于写cmp1、cmp2、cmp3，可以写成这样：
```cpp
//input
sort(peoples,peoples+n,[](people a,people b){return a.age>b.age;});
//do something
sort(peoples,peoples+n,[](people a,people b){return a.chengji>b.chengji;});
//do something
sort(peoples,peoples+n,[](people a,people b){return a.RP>b.RP;});
//do something
```
我们发现这三行重复特多，打起来特烦，但是没有多少不同的地方。于是，我们就可以利用宏做到打一遍抵三遍的效果。
```cpp
#define arrsort(arr,len,pre) sort(arr,arr+(len),pre)
#define stru_op_pre(stru,fie,oper) [](stru a,stru b){return a.fie oper b.fie;}
//将stru的fie字段按op排序的lambda表达式
#define sortp(fie) arrsort(peoples,n,stru_op_pre(people,fie,>))

sortp(age);
sortp(chengji);
sortp(RP);
```
## 结论：宏可以和lambda表达式结合起来食用，并且更美味。

以下是更多相似的宏：
```cpp
#define op_pre(type,oper) [](type a,type b){return a oper b;}
//返回接受两个类型为type的参数，根据oper排序的lambda函数
#define stru_op_pre(stru,fie,oper) [](stru a,stru b){return a.fie oper b.fie;}
//返回用stru的fie字段根据oper排序的lambda函数
#define int_op_pre(oper) op_pre(int,oper)
//返回接受两个类型为int的参数，根据oper排序的lambda函数
```
## 拓展：Lisp中的宏

![Lisp编译流程。图片来源于网络，侵权删。](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1534680673556&di=f91835f22e409a077bda599de1eb8614&imgtype=0&src=http%3A%2F%2Fimg1.ph.126.net%2FoqDn20qmITe937LN4heWsQ%3D%3D%2F6597278673866412250.png)

C++的宏都是简单的字符串拼接，导致它们只能实现一些很基础的功能。

但是如果C++的宏是C++代码呢？

Lisp的宏就是Lisp在编译时运行的程序，能将表达式变形成Lisp编译器能够接受的形式。

利用宏，我们甚至可以做出内嵌语言，将Lisp改造成一个完全不同的形式。比如，在Lisp里面使用指针，或者使用Brainf**k的语法写程序。

由于Lisp宏的强大一大部分来源于Lisp语法结构（S-expression）的古怪，因此即使是用伪代码，我也很难在C++上将Lisp的宏的强大展示给读者。参考文献中《Lisp的本质》一文对此有通俗易懂的论述，有兴趣的读者可以去看看。

## 参考文献

感谢以下作者辛勤的劳作

- [C++编译过程简介-from 云东](https://www.cnblogs.com/dongdongweiwu/p/4743709.html)
- [C++宏定义详解-from dongfs_love](http://blog.chinaunix.net/uid-21372424-id-119797.html)
- [编程利器——lambda表达式-from colazcy](https://www.luogu.org/blog/64456/bian-cheng-li-qi-lambda-biao-da-shi)
- [Lisp的本质-from Slava Akhmechet，译者Alec Jang](https://blog.csdn.net/d603010999/article/details/48155837)

## 版权信息

本文可任意转载或改编，但须署原作者姓名及原文地址，并且应携带此版权信息。由此改编的文章也应携带此版权信息，以及原文作者姓名及地址。