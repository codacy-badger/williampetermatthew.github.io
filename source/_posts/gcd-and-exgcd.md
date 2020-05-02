---
title: gcd和exgcd
date: 2018-11-07 10:24:24
tags: 
  - 数学
---
# gcd
STL库中有个template化的非递归版gcd，需要调用`<alogorithm>`  
使用方式是
```cpp
c=__gcd(a,b);
```
手写的话：

递归版：
```cpp
int gcd(int a,int b){return (!b)?a:gcd(b,a%b);}
```
非递归版：
```cpp
int gcd(int a,int b){while(b){int t=a%b;a=b;b=t;}return a;}
```
# exgcd
递归版：
```cpp
int exgcd(int a,int b,int &x,int &y)
{
    if(!b){x=1,y=0;return a;}
    int t=gcd(b,a%b,y,x);y-=a/b*x;return t;
}
```
非递归版：
```cpp
int exgcd(int a,int b,int &x,int &y)
{
	int t=a%b,x0=1,y0=0,x1=0,y1=1;x=x1;y=y1;
	while(t)
	{
		x=x0-a/b*x1,y=y0-a/b*y1;
		x0=x1,y0=y1;x1=x,y1=y;
		a=b,b=t,t=a%b;
	}
	return a;
}
```
