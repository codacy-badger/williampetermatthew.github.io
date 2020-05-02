---
title: 【转载】你不知道的C++11新语法
date: 2018-08-12 21:23:00
tags: 
  - 洛谷日报
---


------------
# 你不知道的C++11新语法
------------

> 声明：本文为转载文章，转载自[洛谷日报#21](https://www.luogu.org/blog/64456/ni-fou-zhi-dao-di-c-yu-fa)，原作者为colazcy。

------------
随着C++11的发布，C++这门语言有了本质上的提升。C++14,C++17的相继推出，更是让C++这门语言达到了一个新高度。新的标准库设施，新的语法，让我们得以书写更加安全、便捷、高效的程序。

2018年6月编程语言排行榜：
![2018年6月编程语言排行榜](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=3957929647,686454819&fm=173&app=25&f=JPEG?w=640&h=561&s=4CA23472990FD54F0EFDE1DA0000F0B2)

那么这些新的语法究竟是什么？它们如何使用？能为我们编程带来哪些便利？这便是本文所探讨的。

本文参考部分资料，文末已给出原文章地址。

## 新的空指针类型——nullptr

适用度:★★★★★

nullptr是一种特殊的字面值，它可以转化为任意一种指针类型。 原来我们初始化一个空指针都是直接将他赋值为NULL，但NULL实际上是一个宏,其值相当于0。

编译器是这么定义NULL的:
```cpp
#ifdef __cplusplus //如果定义__cplusplus宏，说明正在编译C++语言
#define NUll 0
#else
#define NULL ((void *)0)
#endif
```
也许你会想“我们用NULL还不是照样~~吊打集训队~~”，nullptr好像并没有什么用。

考虑这样一段代码:
```cpp
int f(int x){
    //do something
}
int f(int *x){
    //do something
}
int main(){
    f(NULL);
}
```
很显然，编译失败，对f的调用有二义性。因为NULL相当于0，既可转化为指针，也可转化为整形。将NULL换做nullptr即可，nullptr便是为了解决这种二义性的问题而诞生的。

条件允许的前提下，尽量使用nullptr，它比NULL更加安全， 原来这样写:
```cpp
int a = NULL;
int *b = NULL;
```
现在应该这样写:
```cpp
int a = NULL;
int *b = nullptr;
```
## 避免奇葩错误——constexpr变量

适用度:★★★★☆

在编程中，我们经常遇到需要定义常量的情况，但有些常量却并不是你所想的“常量”。因而会引发一些意想不到的错误。

例如:
```cpp
int a;
const int b = a + 10;
int c[b];//错误，b不是一个常量表达式，它的值每次运行都有可能不一样。
```
b的确是一个常量——它的值在程序的执行期间不会被修改，但是它并不是常量表达式——每次执行程序时都为同一个值，且程序执行期间无法被修改。

使用constexpr而非const来声明常量，让编译器来帮你检查常量是不是每次程序执行都为同一个值。
```cpp
int a;
constexpr b = a + 10;//错误！a不是常量表达式!
```
```cpp
int a;
const int b = a + 10;
constexpr int c = b + 10;//错误，b为常量，但不是常量表达式！
```
```cpp
const int a = 10;
const int b = a + 10;
constexpr int c = b + 10;//正确
```
## 省事好帮手——auto类型指示符

适用度:★★★★★

有些类型名字太长，难以拼写，浪费时间。怎么办？

知道函数的作用，却无法拼写其返回类型，无法保存其返回值。怎么办？

这个时候auto类型指示符就能够助我们一臂之力了。

原来我们这么写:
```cpp
vector<int> vec;
for(vector<int>::iterator it = vec.begin();it != vec.end();it++)//Do something
```
现在可以简单的这么写:
```cpp
vector<int> vec;
for(auto it = vec.begin();it != vec.end();it++)//Do something
```
怎么样？程序瞬间清爽了许多有木有。而且还可以节约大量宝贵的时间

因为编译器是依靠初始值来推断auto变量的类型的，所以auto变量必须要有初始值。

即使是这样也不行:
```cpp
auto a;
a = 10;
```
当然，也不能用auto来定义数组

auto和引用一起会产生一些奇怪的问题:
```cpp
int i = 1,&r = i;//定义变量i,r为i的引用
auto p = r;//没错,p的值为int，其值为1
```
为什么？因为引用即别名。正如我们熟知的，使用引用其实是使用引用的对象，特别当引用被用作初始值的时候，真正参与初始化的其实是引用对象的值。此时编译器以引用对象的类型作为auto的类型。

## 自动类型推断——decltype类型指示符

适用度:★★★★☆

上文提到了auto的用法，有时候我们想要用表达式的类型初始化一个变量，却并不想用表达式的值初始化这个变量。这个时候decltype类型指示符就可以派上用场了。

剧透：下文位置返回类型配合decltype类型指示符有惊喜

我们可以这样用decltype类型指示符来定义变量:
```cpp
int a = 10;
decltype(a) b;
b = 20;
```
但是要注意，decltype只会用表达式的返回值进行推断，并不会执行表达式。例如:
```cpp
int f(){
    cout << "Hello decltype!" << endl;
    return 0;
}
decltype(f()) i = 123;//i的值为123
//程序运行并不会有任何输出，因为f函数并没有实际执行。
```
```cpp
int i = 1;
decltype(i = 123) b = i;
cout << i << endl;//输出1，因为i = 42表达式并未实际执行
```
decltype和auto都可以完成类型推断的任务，那么它们有什么不同呢？
1.处理引用
```cpp
int i = 1,&r = i;//定义int型变量i,r为i的引用。
auto a = r;//此时a的类型为int
decltype(r) b = r;//此时b的类型为int&,即为int的引用。
```
2.处理顶层const

这里引入一个概念：

1.底层const，对象所指向的对象是const的。 2.顶层const，对象本身是const的。

auto会忽略掉顶层const和引用，但是会保留底层const。
```cpp
const int ci = i,&cr = ci;
auto a = ci;//a为int，顶层const被忽略
auto b = cr;//b为int，顶层const和引用均被忽略
auto c = &ci;//c为指向常量int的指针，保留底层const
```
如果要使auto类型为顶层const:
```cpp
int i = 1;
const auto a = i;//a为const int 类型
```
如果decltype使用的表达式是一个变量，decltype会返回该变量的类型（包括引用和顶层const)。

## 循环宏的优秀替代品——范围for语句

适用度:★★★★★

什么？就算有了auto类型指示符，遍历容器/数组每一个元素你还是嫌麻烦？没事，让范围for语句来帮你。

原来这么遍历容器/数组每一个元素
```cpp
vector<int> vec;
for(auto it = vec.begin();it != vec.end();it++)
    cout << *it << " ";
```
现在这么写:
```cpp
vector<int> vec;
for(auto it : vec)
    cout << it << " ";
```
注意，范围for语句只能遍历每一个元素，所以像遍历1到10这种操作还是得自己乖乖写for循环:)。

## 复杂返回值必备——尾置返回类型

适用度:★★★★☆

普通函数完全不必要尾置返回类型，但是当函数返回类型复杂起来时，尾置返回类型就很有用了。
```cpp
int (*func(int i))[10]{
    //Do something
}
//func(int i)表示调用函数时，需要一个int类型的参数；
//(*func(int i))表示对调用func的结果执行解引用的操作；
//(*func(int i))[10]表示解引用之后得到一个维度为10的数组；
//int (*func(int i))[10]表示数组的数据类型为int；
```
很复杂，对吧？（~~当然对于dalao来说小菜一碟~~）当返回类型更加复杂时，常规写法将会成为Debug噩梦。（话说Markdown好像识别不了尾置返回类型诶）。

//返回一维数组
```cpp
auto func(int i) -> int(*)[10]{
    //Do something
}
```
还有更复杂的（~~我太蒻了给不出常规写法了~~）

二维数组:
```cpp
auto func(int i) -> int(*)[10][10]{
    //Do something
}
```
二重指针:
```cpp
auto func(int i) -> int **{
    //Do something
}
```
除了数组特殊一些以外，平时定义变量怎么写，尾置返回类型就怎么写。程序瞬间清爽了许多有木有。

如果返回值更加复杂，连尾置返回类型的作用都显得微乎其微了怎么办？这时候——

配合decltype食用效果更佳
```cpp
auto func(int a,int b) -> decltype(a+b){
    //Do something
    return a+b;//函数的返回类型即为int
}
```
什么？你连尾置返回类型都嫌麻烦？C++14可以满足你的需求。没错，连尾置返回类型都可以省了，直接返回类型auto就可以了Orz。

## 命名困难户/~~装逼者的宠儿~~——Lambda表达式

适用度:★★★☆☆

假如遇到一道~~毒瘤~~题，既需要从小到大排序，也需要从大到小排序，甚至还要给自己定义的结构体排序。难道排序函数依次叫做cmp1,cmp2,cmp3?~~太没有逼格了吧~~

一个完整的Lambda表达式由以下几个部分构成：
```cpp
[capture list] (params list) mutable exception-> return type { function body }
```
各项具体含义如下

1. capture list：捕获外部变量列表 可以为空，但是不可以省略
2. params list：形参列表 可以为空，但是不可以省略
3. mutable指示符：用来说用是否可以修改捕获的变量 可以省略
4. exception：异常设定 可以省略
5. return type：返回类型 可以省略
6. function body：函数体 可以为空，但是不可以省略

太复杂了，对吧？实际上，OI中我们使用Lambda表达式主要是用于STL的谓词（比如排序），因而我们可以省略很多不必要的部分。

该省略的省略后就十分简单了: 比如从大到小排序:
```cpp
vector<int> vec;
sort(vec.begin(),vec.end(),[](int a,int b){
    return a > b;
});
```
Lambda表达式看似复杂，却能在许多时候为我们提供不小便利。它也是函数式编程的基石。

因考虑篇幅，Lambda表达式并未详细介绍。想要知道更多关于Lambda表达式的内容，可以看看我的另一篇文章。[传送门](https://www.luogu.org/blog/zcy/bian-cheng-li-qi-lambda-biao-da-shi)

鸣谢:

本文参考了以下资料，感谢作者的辛劳付出：

https://www.jianshu.com/p/2d44dae53910

https://blog.csdn.net/zdy0_2004/article/details/69934828

https://www.cnblogs.com/DswCnblog/p/5629165.html

https://blog.csdn.net/y1196645376/article/details/51441503

注:因C++11语法繁杂，有些高级特性只为大型工程而设计，对OI并无太大帮助，因而未能出现在文章中（如继承，多态，泛型编程）等等。本人水平有限，文章难免有错误，望读者多多海涵，可以评论指出错误，一定尽力修正。