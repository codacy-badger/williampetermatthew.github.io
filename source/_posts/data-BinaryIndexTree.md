---
title: 树状数组
date: 2018-11-08 17:01:45
tags: 
  - 数据结构
---


```cpp
int lowbit(int x)
{
    return x&(-x);
}
```
# [树状数组1](https://www.luogu.org/problemnew/show/P3374)
单点修改，区间查询。
```cpp
//需要先
	add(i,a[i]);
```
```cpp
void add(int x,long long d)
{
    for(;x<=nx;x+=lowbit(x))
        tree[x]+=d;
}
   add(x,k);
```
```cpp
long long ask(int x)
{
    long long s;
    for(s=0;x;x-=lowbit(x))
        s+=tree[x];
    return s;
}
```
```cpp
long long range_ask(int xa,int xb)
{
    long long s=0;
    s+=ask(xb);
    s-=ask(xa-1);
    return s;
}
	printf("%lld\n",range_ask(xa,xb));
```
完整代码如下：
```cpp
#include<bits/stdc++.h>
using namespace std;
long long tree[500005];
int nx;
int lowbit(int x)
{
    return x&(-x);
}
void add(int x,long long d)
{
    for(;x<=nx;x+=lowbit(x))
        tree[x]+=d;
}
void range_add(int xa,int xb,long long d)
{
    add(xa,d);
    add(xb+1,-d);
}
long long ask(int x)
{
    long long s;
    for(s=0;x;x-=lowbit(x))
        s+=tree[x];
    return s;
}
long long range_ask(int xa,int xb)
{
    long long s=0;
    s+=ask(xb);
    s-=ask(xa-1);
    return s;
}
int q;
int main()
{
    scanf("%d%d",&nx,&q);
	for(int i=1,x;i<=nx;i++)
	{
		scanf("%d",&x);
		add(i,x);
	}
    for(int i=1,opt,x,xa,xb;i<=q;i++)
    {
        scanf("%d",&opt);
        if(opt==1)
        {
            long long k; 
            scanf("%d%lld",&x,&k);
            add(x,k);
        }
        if(opt==2)
        {
            scanf("%d%d",&xa,&xb);
            printf("%lld\n",range_ask(xa,xb));
        }
    }
}
```
# [树状数组2](https://www.luogu.org/problemnew/show/P3368)
区间修改，单点查询。
```cpp
//需要先
	range_add(i,i,a[i]);
```
```cpp
void add(int x,long long d)
{
    for(;x<=nx;x+=lowbit(x))
        tree[x]+=d;
}
```
```cpp
void range_add(int xa,int xb,long long d)
{
    add(xa,d);
    add(xb+1,-d);
}
	range_add(xa,xb,k);
```
```cpp
long long ask(int x)
{
    long long s;
    for(s=0;x;x-=lowbit(x))
        s+=tree[x];
    return s;
}
	printf("%lld\n",ask(x));
```
完整代码如下：
```cpp
#include<bits/stdc++.h>
using namespace std;
long long tree[500005];
int nx;
int lowbit(int x)
{
    return x&(-x);
}
void add(int x,long long d)
{
    for(;x<=nx;x+=lowbit(x))
        tree[x]+=d;
}
void range_add(int xa,int xb,long long d)
{
    add(xa,d);
    add(xb+1,-d);
}
long long ask(int x)
{
    long long s;
    for(s=0;x;x-=lowbit(x))
        s+=tree[x];
    return s;
}
long long range_ask(int xa,int xb)
{
    long long s=0;
    s+=ask(xb);
    s-=ask(xa-1);
    return s;
}
int q;
int main()
{
    scanf("%d%d",&nx,&q);
	for(int i=1,x;i<=nx;i++)
	{
		scanf("%d",&x);
		range_add(i,i,x);
	}
    for(int i=1,opt,x,xa,xb;i<=q;i++)
    {
        scanf("%d",&opt);
        if(opt==1)
        {
            long long k;
            scanf("%d%d%lld",&xa,&xb,&k);
            range_add(xa,xb,k);
        }
        if(opt==2)
        {
            scanf("%d",&x);
            printf("%lld\n",ask(x));
        }
    }
}
```