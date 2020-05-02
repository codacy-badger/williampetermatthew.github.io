---
title: 线段树
date: 2018-11-08 17:10:29
tags: 
  - 数据结构
---


```cpp
int ls(int x){return x<<1;}
int rs(int x){return x<<1|1;}
```
# [线段树1](https://www.luogu.org/problemnew/show/P3372)
区间加减，区间查询
```cpp
void push_up(int x){ans[x]=ans[ls(x)]+ans[rs(x)];}
```
```cpp
void build(int x,int l,int r)
{
    add[x]=0;
    if(l==r)
    {
        ans[x]=a[l];
        return ;
    }
    int mid=(l+r)>>1;
    build(ls(x),l,mid);
    build(rs(x),mid+1,r);
    push_up(x);
}
	build(1,1,n);
```
```cpp
void push_down(int x,int l,int r)
{
    int mid=(l+r)>>1;
    add[ls(x)]=add[ls(x)]+add[x];
    add[rs(x)]=add[rs(x)]+add[x];
    ans[ls(x)]=ans[ls(x)]+add[x]*(mid-l+1);
    ans[rs(x)]=ans[rs(x)]+add[x]*(r-mid);
    add[x]=0;
}
```
```cpp
void addup(int nl,int nr,int l,int r,int x,long long k)
{
    if(nl<=l&&r<=nr)
    {
        ans[x]=ans[x]+k*(r-l+1);
        add[x]=add[x]+k;
        return ;
    }
    push_down(x,l,r);
    int mid=(l+r)>>1;
    if(nl<=mid)addup(nl,nr,l,mid,ls(x),k);
    if(mid+1<=nr)addup(nl,nr,mid+1,r,rs(x),k);
    push_up(x);
}
	addup(l,r,1,n,1,k);
```
```cpp
long long query(int ql,int qr,int l,int r,int x)
{
    long long res=0;
    if(ql<=l&&r<=qr)return ans[x];
    int mid=(l+r)>>1;
    push_down(x,l,r);
    if(ql<=mid)res+=query(ql,qr,l,mid,ls(x));
    if(mid+1<=qr)res+=query(ql,qr,mid+1,r,rs(x));
    return res;
}
	print(query(l,r,1,n,1));
```
完整代码如下：
```cpp
#include<bits/stdc++.h>
using namespace std;
template<typename __Type_of_scan>
void scan(__Type_of_scan &x)
{
    __Type_of_scan f=1;x=0;char s=getchar();
    while(s<'0'||s>'9'){if(s=='-')f=-1;s=getchar();}
    while(s>='0'&&s<='9'){x=x*10+s-'0';s=getchar();}
    x*=f;
}
template<typename __Type_of_print>
void print(__Type_of_print x)
{
    if(x<0){putchar('-');x=-x;}
    if(x>9)print(x/10);
    putchar(x%10+'0');
}
int n,m;
long long a[100005],ans[400005],add[400005];
int ls(int x){return x<<1;}
int rs(int x){return x<<1|1;}
void push_up(int x){ans[x]=ans[ls(x)]+ans[rs(x)];}
void build(int x,int l,int r)
{
    add[x]=0;
    if(l==r)
    {
        ans[x]=a[l];
        return ;
    }
    int mid=(l+r)>>1;
    build(ls(x),l,mid);
    build(rs(x),mid+1,r);
    push_up(x);
}
void push_down(int x,int l,int r)
{
    int mid=(l+r)>>1;
    add[ls(x)]=add[ls(x)]+add[x];
    add[rs(x)]=add[rs(x)]+add[x];
    ans[ls(x)]=ans[ls(x)]+add[x]*(mid-l+1);
    ans[rs(x)]=ans[rs(x)]+add[x]*(r-mid);
    add[x]=0;
}
void addup(int nl,int nr,int l,int r,int x,long long k)
{
    if(nl<=l&&r<=nr)
    {
        ans[x]=ans[x]+k*(r-l+1);
        add[x]=add[x]+k;
        return ;
    }
    push_down(x,l,r);
    int mid=(l+r)>>1;
    if(nl<=mid)addup(nl,nr,l,mid,ls(x),k);
    if(mid+1<=nr)addup(nl,nr,mid+1,r,rs(x),k);
    push_up(x);
}
long long query(int ql,int qr,int l,int r,int x)
{
    long long res=0;
    if(ql<=l&&r<=qr)return ans[x];
    int mid=(l+r)>>1;
    push_down(x,l,r);
    if(ql<=mid)res+=query(ql,qr,l,mid,ls(x));
    if(mid+1<=qr)res+=query(ql,qr,mid+1,r,rs(x));
    return res;
}
int main()
{
    scan(n),scan(m);
    for(int i=1;i<=n;i++)
        scan(a[i]);
    build(1,1,n);
    for(int i=1,opt,x,y;i<=m;i++)
    {
        scan(opt),scan(x),scan(y);
        if(opt==1)
        {
            long long k;
            scan(k);
            addup(x,y,1,n,1,k);
        }
        if(opt==2)
        {
            print(query(x,y,1,n,1)),putchar('\n');
        }
    }
}
```
# [线段树2](https://www.luogu.org/problemnew/show/P3373)
模意义下：区间乘，区间加减，区间查询
```cpp
void push_up(int x){ans[x]=(ans[ls(x)]+ans[rs(x)])%p;}
```
```cpp
void build(int x,int l,int r)
{
    add[x]=0;mul[x]=1;
    if(l==r)
    {
        ans[x]=a[l];
        return ;
    }
    int mid=(l+r)>>1;
    build(ls(x),l,mid);
    build(rs(x),mid+1,r);
    push_up(x);
}
	build(1,1,n);
```
```cpp
void push_down(int x,int l,int r)
{
    int mid=(l+r)>>1;
    mul[ls(x)]=(mul[ls(x)]*mul[x])%p;
    mul[rs(x)]=(mul[rs(x)]*mul[x])%p;
    add[ls(x)]=(add[ls(x)]*mul[x])%p;
    add[rs(x)]=(add[rs(x)]*mul[x])%p;
    ans[ls(x)]=(ans[ls(x)]*mul[x])%p;
    ans[rs(x)]=(ans[rs(x)]*mul[x])%p;
    mul[x]=1;
    add[ls(x)]=(add[ls(x)]+add[x])%p;
    add[rs(x)]=(add[rs(x)]+add[x])%p;
    ans[ls(x)]=(ans[ls(x)]+add[x]*(mid-l+1))%p;
    ans[rs(x)]=(ans[rs(x)]+add[x]*(r-mid))%p;
    add[x]=0;
}
```
```cpp
void addup(int nl,int nr,int l,int r,int x,long long k)
{
    if(nl<=l&&r<=nr)
    {
        ans[x]=(ans[x]+k*(r-l+1))%p;
        add[x]=(add[x]+k)%p;
        return ;
    }
    push_down(x,l,r);
    int mid=(l+r)>>1;
    if(nl<=mid)addup(nl,nr,l,mid,ls(x),k);
    if(mid+1<=nr)addup(nl,nr,mid+1,r,rs(x),k);
    push_up(x);
}
	addup(l,r,1,n,1,k);
```
```cpp
void mulup(int nl,int nr,int l,int r,int x,long long k)
{
    if(nl<=l&&r<=nr)
    {
        ans[x]=(ans[x]*k)%p;
        mul[x]=(mul[x]*k)%p;
        add[x]=(add[x]*k)%p;
        return ;
    }
    push_down(x,l,r);
    int mid=(l+r)>>1;
    if(nl<=mid)mulup(nl,nr,l,mid,ls(x),k);
    if(mid+1<=nr)mulup(nl,nr,mid+1,r,rs(x),k);
    push_up(x);
}
	mulup(l,r,1,n,1,k);
```
```cpp
long long query(int ql,int qr,int l,int r,int x)
{
    long long res=0;
    if(ql<=l&&r<=qr)return ans[x]%p;
    int mid=(l+r)>>1;
    push_down(x,l,r);
    if(ql<=mid)res+=query(ql,qr,l,mid,ls(x));
    if(mid+1<=qr)res+=query(ql,qr,mid+1,r,rs(x));
    return res%p;
}
	print(query(l,r,1,n,1));
```
完整代码如下：
```cpp
#include<bits/stdc++.h>
using namespace std;
template<typename __Type_of_scan>
void scan(__Type_of_scan &x)
{
    __Type_of_scan f=1;x=0;char s=getchar();
    while(s<'0'||s>'9'){if(s=='-')f=-1;s=getchar();}
    while(s>='0'&&s<='9'){x=x*10+s-'0';s=getchar();}
    x*=f;
}
template<typename __Type_of_print>
void print(__Type_of_print x)
{
    if(x<0){putchar('-');x=-x;}
    if(x>9)print(x/10);
    putchar(x%10+'0');
}
int n,m;
long long p;
long long a[100005],ans[400005],add[400005],mul[400005];
int ls(int x){return x<<1;}
int rs(int x){return x<<1|1;}
void push_up(int x){ans[x]=(ans[ls(x)]+ans[rs(x)])%p;}
void build(int x,int l,int r)
{
    add[x]=0;mul[x]=1;
    if(l==r)
    {
        ans[x]=a[l];
        return ;
    }
    int mid=(l+r)>>1;
    build(ls(x),l,mid);
    build(rs(x),mid+1,r);
    push_up(x);
}
void push_down(int x,int l,int r)
{
    int mid=(l+r)>>1;
    mul[ls(x)]=(mul[ls(x)]*mul[x])%p;
    mul[rs(x)]=(mul[rs(x)]*mul[x])%p;
    add[ls(x)]=(add[ls(x)]*mul[x])%p;
    add[rs(x)]=(add[rs(x)]*mul[x])%p;
    ans[ls(x)]=(ans[ls(x)]*mul[x])%p;
    ans[rs(x)]=(ans[rs(x)]*mul[x])%p;
    mul[x]=1;
    add[ls(x)]=(add[ls(x)]+add[x])%p;
    add[rs(x)]=(add[rs(x)]+add[x])%p;
    ans[ls(x)]=(ans[ls(x)]+add[x]*(mid-l+1))%p;
    ans[rs(x)]=(ans[rs(x)]+add[x]*(r-mid))%p;
    add[x]=0;
}
void addup(int nl,int nr,int l,int r,int x,long long k)
{
    if(nl<=l&&r<=nr)
    {
        ans[x]=(ans[x]+k*(r-l+1))%p;
        add[x]=(add[x]+k)%p;
        return ;
    }
    push_down(x,l,r);
    int mid=(l+r)>>1;
    if(nl<=mid)addup(nl,nr,l,mid,ls(x),k);
    if(mid+1<=nr)addup(nl,nr,mid+1,r,rs(x),k);
    push_up(x);
}
void mulup(int nl,int nr,int l,int r,int x,long long k)
{
    if(nl<=l&&r<=nr)
    {
        ans[x]=(ans[x]*k)%p;
        mul[x]=(mul[x]*k)%p;
        add[x]=(add[x]*k)%p;
        return ;
    }
    push_down(x,l,r);
    int mid=(l+r)>>1;
    if(nl<=mid)mulup(nl,nr,l,mid,ls(x),k);
    if(mid+1<=nr)mulup(nl,nr,mid+1,r,rs(x),k);
    push_up(x);
}
long long query(int ql,int qr,int l,int r,int x)
{
    long long res=0;
    if(ql<=l&&r<=qr)return ans[x]%p;
    int mid=(l+r)>>1;
    push_down(x,l,r);
    if(ql<=mid)res+=query(ql,qr,l,mid,ls(x));
    if(mid+1<=qr)res+=query(ql,qr,mid+1,r,rs(x));
    return res%p;
}
int main()
{
    scan(n),scan(m),scan(p);
    for(int i=1;i<=n;i++)
        scan(a[i]);
    build(1,1,n);
    for(int i=1,opt,x,y;i<=m;i++)
    {
        scan(opt),scan(x),scan(y);
        if(opt==1)
        {
            long long k;
            scan(k);
            mulup(x,y,1,n,1,k);
        }
        if(opt==2)
        {
            long long k;
            scan(k);
            addup(x,y,1,n,1,k);
        }
        if(opt==3)
        {
            print(query(x,y,1,n,1)),putchar('\n');
        }
    }
}
```