---
title: 输入输出优化代码
date: 2018-07-22 15:50:22
tags: 
  - 科技
---
## 输入输出效率比较
一份摘自LOJ的一份读入测试数据，若干读入整数速度测试的结果（单位：毫秒）。 
输入：$3×10^6$个在区间中随机生成的十进制整数。

| # | Lanuage | $[0,2)$ | $[0,8)$ | $[0,2^{15})$ | $[0,2^{31})$ | $[0,2^{63})$ |
| ------ | ------ | ------ | ------ | ------ | ------ | ------ |
| fread | G++ 5.4.0 (-O2) | 13 | 13 | 39 | 70 | 111 |
| getchar | G++ 5.4.0 (-O2) | 58 | 73 | 137 | 243 | 423 |
| cin(关闭同步) | G++ 5.4.0 (-O2) | 161 | 147 | 205 | 270 | 394 |
| cin | G++ 5.4.0 (-O2) | 442 | 429 | 706 | 1039 | 1683 |
| scanf | G++ 5.4.0 (-O2) | 182 | 175 | 256 | 368 | 574 |

下面水下一些常用的输出输出方式：

## cin/cout

采用C++的特性通配流式输入输出的好处是不用处理变量类型，直接写就可以；但是根据上表可以看到，这种输入输出方式十分的耗时，并不适用于大数据编程。
```cpp
#include<iostream>
using namespace std;
int main()
{
	int a;
	short b;
	long long c;
	cin>>a>>b>>c;
	cout<<a<<' '<<b<<' '<<c<<'\n';
}
```

## scanf/printf

采用传统C语言的输入输出的好处是节省时间，适用于大数据编程，一般的题目都可以用这种方式输入输出来通过；缺点是必须对应数据的类型而且不能少","，否则就会CE，scanf不能少"&"否则会导致RE，printf不能加"&"，否则会输出奇奇怪怪的东西（地址）。
```cpp
#include<cstdio>
int main()
{
	int a;
	short b;
	long long c;
	scanf("%d%hd%lld",&a,&b,&c);
	printf("%d %hd %lld\n",a,b,c);
}
```

## cin/cout（关闭同步）

我们可以通过写关闭同步的方式来加快流式输入输出，这种方式的好处是可以加快速度，甚至比刚才介绍的传统输入输出方式还要快；但是请注意：请不要在关闭同步之后采用scanf及printf输入输出，否则后果自负！
```cpp
#include<iostream>
using namespace std;
int main()
{
	ios::sync_with_stdio(false);//警告：请不要在关闭同步之后采用scanf及printf输入输出，否则后果自负！
	int a;
	short b;
	long long c;
	cin>>a>>b>>c;
	cout<<a<<' '<<b<<' '<<c<<'\n';
}
```

## 普通版（基于getchar/putchar）优化：

有些时候我们会因为程序的常数不够优秀而被大数据卡爆，或者我们希望暴力能够得到略微高一点的分数，这时候我们就可以采用基于getchar和putchar函数的输入输出优化。一般见到的输入优化定义的函数名是read，但是经过我多次运用，发现我一般用到的情况都是scanf被卡后才用的，所以为了方便我切换函数我使用scan定义函数。

输入优化：
```cpp
template<typename __Type_of_scan>
void scan(__Type_of_scan &x)
{
	__Type_of_scan f=1;x=0;char s=getchar();
	while(s<'0'||s>'9'){if(s=='-')f=-1;s=getchar();}
	while(s>='0'&&s<='9'){x=x*10+s-'0';s=getchar();}
	x*=f;
}
```
用法示例：
```cpp
int main()
{
	int a;
	short b;
	long long c;
	scan(a),scan(b),scan(c);
}
```
输出优化：
```cpp
template<typename __Type_of_print>
void print(__Type_of_print x)
{
	if(x<0){putchar('-');x=-x;}
	if(x>9)print(x/10);
	putchar(x%10+'0');
}
```
用法示例：
```cpp
int main()
{
	int a=2;
	short b=3;
	long long c=3;
	print(a),putchar(' '),print(b),putchar(' '),print(c),putchar('\n');
}
```

## 黑科技（基于fread/fwrite）优化：

其实这篇博客之前一直都是基于getchar和putchar函数的输入输出优化。直到2018年9月2日我在学校的一次模拟赛中O(n)算法被毒瘤出题人的数据卡爆，我才开始使用基于fread和fwrite函数的输入输出优化。同之前所说的一样，这里所定义的函数名均为之前的优化代码前加"_"的名字，即_scan和_print。

其实fread和fwrite有个缺点就是本地测样例比较麻烦（也有可能是我写的代码太蒟蒻了），而且**采用_print函数后，你需要运行fsh函数！！！**

输入优化：
```cpp
char gc()
{
	static char buf[100000],*p1=buf,*p2=buf;
	return p1==p2&&(p2=(p1=buf)+fread(buf,1,100000,stdin),p1==p2)?EOF:*p1++;
}
template<typename __Type_of__scan>
void _scan(__Type_of__scan &x)
{
	__Type_of__scan f=1;x=0;char s=gc();
	while(s<'0'||s>'9'){if(s=='-')f=-1;s=gc();}
	while(s>='0'&&s<='9'){x=x*10+s-'0';s=gc();}
	x*=f;
}
```
用法示例：
```cpp
int main()
{
	int a;
	short b;
	long long c;
	_scan(a),_scan(b),_scan(c);
}
```
输出优化：
```cpp
char buf[100000],*pp=buf;
void pc(const char c)
{
	if(pp-buf==100000)fwrite(buf,1,100000,stdout),pp=buf;
	*pp++=c;
}
template<typename __Type_of__print>
void _print(__Type_of__print x)
{
	if(x<0){pc('-');x=-x;}
	if(x>9)_print(x/10);
	pc(x%10+'0');
}
void fsh(){fwrite(buf,1,pp-buf,stdout);pp=buf;}
```
用法示例：
```cpp
int main()
{
	int a=2;
	short b=3;
	long long c=3;
	_print(a),pc(' '),fsh(),_print(b),pc(' '),fsh(),_print(c),pc('\n'),fsh();
}
```

## 关于一次性输入输出多个数
**警告：可变模板仅在开启 -std=c++11 或 -std=gnu++11 时可用**  
**Warning:variadic templates only available with -std=c++11 or -std=gnu++11**  

我们以普通版（基于getchar/putchar）优化的为例  
下面是一个多输入加多输出的示例
```cpp
#include<bits/stdc++.h>
using namespace std;
template<typename tpos>
void scan(tpos &x)
{
	tpos f=1;x=0;char s=getchar();
	while(s<'0'||s>'9'){if(s=='-')f=-1;s=getchar();}
	while(s>='0'&&s<='9'){x=x*10+s-'0';s=getchar();}
	x*=f;
}
template<typename tpos,typename... Tpos>
void scan(tpos &x,Tpos&... X)
{
	scan(x),scan(X...);
}
template<typename tpop>
void print(tpop x)
{
	if(x<0)putchar('-'),x=-x;
	if(x>9)print(x/10);
	putchar(x%10+'0');
}
template<typename tpop,typename... Tpop>
void print(tpop x,Tpop... X)
{
	print(x),putchar(' '),print(X...);
}
int main()
{
	int a;
	short b;
	long long c;
	scan(a,b,c);
	print(a,b,c),putchar('\n');
	return 0;
} 
```
简单来说，就是在scan函数的下面加上
```cpp
template<typename tpos,typename... Tpos>
void scan(tpos &x,Tpos&... X)
{
	scan(x),scan(X...);
}
```
在print函数的下面加上
```cpp
template<typename tpop,typename... Tpop>
void print(tpop x,Tpop... X)
{
	print(x),putchar(' '),print(X...);
}
```
然后调用
```cpp
	scan(a,b,c);
	print(a,b,c),putchar('\n');
```
事实上，你也可以类比此普通版优化，推出黑科技优化的多输入输出  
下面就是一个例子
```cpp
#include<bits/stdc++.h>
using namespace std;
char gc()
{
    static char buf[100000],*p1=buf,*p2=buf;
    return p1==p2&&(p2=(p1=buf)+fread(buf,1,100000,stdin),p1==p2)?EOF:*p1++;
}
template<typename tpo_s>
void _scan(tpo_s &x)
{
    tpo_s f=1;x=0;char s=gc();
    while(s<'0'||s>'9'){if(s=='-')f=-1;s=gc();}
    while(s>='0'&&s<='9'){x=x*10+s-'0';s=gc();}
    x*=f;
}
template<typename tpo_s,typename... Tpo_s>
void _scan(tpo_s &x,Tpo_s &...X)
{
	_scan(x),_scan(X...);
}
char buf[100000],*pp=buf;
void pc(const char c)
{
    if(pp-buf==100000)fwrite(buf,1,100000,stdout),pp=buf;
    *pp++=c;
}
template<typename tpo_p>
void _print(tpo_p x)
{
    if(x<0){pc('-');x=-x;}
    if(x>9)_print(x/10);
    pc(x%10+'0');
}
void fsh(){fwrite(buf,1,pp-buf,stdout);pp=buf;}
template<typename tpo_p,typename... Tpo_p>
void _print(tpo_p x,Tpo_p ...X)
{
	_print(x),pc(' '),_print(X...);
}
int main()
{
	int a;
	short b;
	long long c;
	_scan(a,b,c);
	_print(a,b,c),pc('\n'),fsh();
	return 0;
} 
```
事实上，这个也就是在_scan函数的下面加上
```cpp
template<typename tpo_s,typename... Tpo_s>
void _scan(tpo_s &x,Tpo_s &...X)
{
	_scan(x),_scan(X...);
}
```
在print函数的下面加上
```cpp
template<typename tpo_p,typename... Tpo_p>
void _print(tpo_p x,Tpo_p ...X)
{
	_print(x),pc(' '),_print(X...);
}
```
然后调用
```cpp
	_scan(a,b,c);
	_print(a,b,c),pc('\n'),fsh();
```

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。