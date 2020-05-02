---
title: 二维树状数组
date: 2019-01-26 14:50:25
tags: 
  - 数据结构
---

```cpp
int lowbit(int x)
{
    return x&(-x);
}
```
# [二维树状数组1](https://www.luogu.org/problemnew/show/U44574)
单点修改，矩阵查询。
```cpp
void add(int x,int y,long long d)
{
	int sy=y;
    for(;x<=nx;x+=lowbit(x))
        for(y=sy;y<=ny;y+=lowbit(y))
            tree[x][y]+=d;
}
   add(x,y,k);
```
```cpp
long long ask(int x,int y)
{
    long long s;
	int sy=y;
    for(s=0;x;x-=lowbit(x))
        for(y=sy;y;y-=lowbit(y))
            s+=tree[x][y];
    return s;
}
```
```cpp
long long range_ask(int xa,int ya,int xb,int yb)
{
    long long s=0;
    s+=ask(xb,yb);
    s-=ask(xb,ya-1);
    s-=ask(xa-1,yb);
    s+=ask(xa-1,ya-1);
    return s;
}
	printf("%lld\n",range_ask(xa,ya,xb,yb));
```
完整代码如下：
```cpp
#include<bits/stdc++.h>
using namespace std;
long long tree[1005][1005];
int nx,ny;
int lowbit(int x)
{
    return x&(-x);
}
void add(int x,int y,long long d)
{
	int sy=y;
    for(;x<=nx;x+=lowbit(x))
        for(y=sy;y<=ny;y+=lowbit(y))
            tree[x][y]+=d;
}
void range_add(int xa,int ya,int xb,int yb,long long d)
{
    add(xa,ya,d);
    add(xa,yb+1,-d);
    add(xb+1,ya,-d);
    add(xb+1,yb+1,d);
}
long long ask(int x,int y)
{
    long long s;
	int sy=y;
    for(s=0;x;x-=lowbit(x))
        for(y=sy;y;y-=lowbit(y))
            s+=tree[x][y];
    return s;
}
long long range_ask(int xa,int ya,int xb,int yb)
{
    long long s=0;
    s+=ask(xb,yb);
    s-=ask(xb,ya-1);
    s-=ask(xa-1,yb);
    s+=ask(xa-1,ya-1);
    return s;
}
int q;
int main()
{
    scanf("%d%d%d",&nx,&ny,&q);
    for(int i=1,opt,x,y,xa,xb,ya,yb;i<=q;i++)
    {
        scanf("%d",&opt);
        if(opt==1)
        {
            long long k; 
            scanf("%d%d%lld",&x,&y,&k);
            add(x,y,k);
        }
        if(opt==2)
        {
            scanf("%d%d%d%d",&xa,&ya,&xb,&yb);
            printf("%lld\n",range_ask(xa,ya,xb,yb));
        }
    }
}
```
# [二维树状数组2](https://www.luogu.org/problemnew/show/U44587)
矩阵修改，单点查询。
```cpp
void add(int x,int y,long long d)
{
	int sy=y;
    for(;x<=nx;x+=lowbit(x))
        for(y=sy;y<=ny;y+=lowbit(y))
            tree[x][y]+=d;
}
```
```cpp
void range_add(int xa,int ya,int xb,int yb,long long d)
{
    add(xa,ya,d);
    add(xa,yb+1,-d);
    add(xb+1,ya,-d);
    add(xb+1,yb+1,d);
}
	range_add(xa,ya,xb,yb,k);
```
```cpp
long long ask(int x,int y)
{
    long long s;
	int sy=y;
    for(s=0;x;x-=lowbit(x))
        for(y=sy;y;y-=lowbit(y))
            s+=tree[x][y];
    return s;
}
	printf("%lld\n",ask(x,y));
```
完整代码如下：
```cpp
#include<bits/stdc++.h>
using namespace std;
long long tree[1005][1005];
int nx,ny;
int lowbit(int x)
{
    return x&(-x);
}
void add(int x,int y,long long d)
{
	int sy=y;
    for(;x<=nx;x+=lowbit(x))
        for(y=sy;y<=ny;y+=lowbit(y))
            tree[x][y]+=d;
}
void range_add(int xa,int ya,int xb,int yb,long long d)
{
    add(xa,ya,d);
    add(xa,yb+1,-d);
    add(xb+1,ya,-d);
    add(xb+1,yb+1,d);
}
long long ask(int x,int y)
{
    long long s;
	int sy=y;
    for(s=0;x;x-=lowbit(x))
        for(y=sy;y;y-=lowbit(y))
            s+=tree[x][y];
    return s;
}
long long range_ask(int xa,int ya,int xb,int yb)
{
    long long s=0;
    s+=ask(xb,yb);
    s-=ask(xb,ya-1);
    s-=ask(xa-1,yb);
    s+=ask(xa-1,ya-1);
    return s;
}
int q;
int main()
{
    scanf("%d%d%d",&nx,&ny,&q);
    for(int i=1,opt,x,y,xa,xb,ya,yb;i<=q;i++)
    {
        scanf("%d",&opt);
        if(opt==1)
        {
            long long k;
            scanf("%d%d%d%d%lld",&xa,&ya,&xb,&yb,&k);
            range_add(xa,ya,xb,yb,k);
        }
        if(opt==2)
        {
            scanf("%d%d",&x,&y);
            printf("%lld\n",ask(x,y));
        }
    }
}
```