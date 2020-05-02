---
title: 树链剖分
date: 2019-01-03 11:02:31
tags: 
  - 数据结构
---

树链剖分的一些操作：
- 查询子树范围
- 查询两点最短路径
- 求LCA ← 树剖LCA
- 最大子树
- 结合线段树可以做到：
	- 修改单点值/子树/两点间的值
	- 查询单点值/子树/两点间的和/最大值/最小值
    - ······
- ······

## 模板  
树剖+线段树（维护区间和）
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
void scan(tpos &x,Tpos &...X)
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
void print(tpop x,Tpop ...X)
{
    print(x),putchar(' '),print(X...);
}
int head[100005],cnt,n,m,root,size[100005],son[100005],top[100005],dep[100005],fa[100005],st[100005],ed[100005],dfx[100005];
int p,a[100005],add[400005],ans[400005];

struct edge
{
    int ne,fr,to;
}e[200005];
void adde(int u,int v)
{
    e[++cnt]=(edge){head[u],u,v};head[u]=cnt;
    e[++cnt]=(edge){head[v],v,u};head[v]=cnt;
}
void dfs1(int u)
{
    size[u]=1;
    for(int i=head[u],v;i;i=e[i].ne)
    {
        v=e[i].to;
        if(v!=fa[u])
        {
            fa[v]=u;
            dep[v]=dep[u]+1;
            dfs1(v);
            size[u]+=size[v];
            if(size[v]>size[son[u]])son[u]=v;
        }
    }
}
void dfs2(int u)
{
    static int dfn;
    st[u]=++dfn;
    dfx[dfn]=u;
    if(son[u])
    {
        top[son[u]]=top[u];
        dfs2(son[u]);
    }
    for(int i=head[u],v;i;i=e[i].ne)
    {
        v=e[i].to;
        if(v!=fa[u]&&v!=son[u])
        {
            top[v]=v;
            dfs2(v);
        }
    }
    ed[u]=dfn;
}
int ls(int x){return x<<1;}
int rs(int x){return x<<1|1;}
void push_up(int x){ans[x]=(ans[ls(x)]+ans[rs(x)])%p;ans[x]%=p;}
void build(int x,int l,int r)
{
    add[x]=0;
    if(l==r)
    {
        ans[x]=a[dfx[l]];
        return ;
    }
    int mid=(l+r)>>1;
    build(ls(x),l,mid);
    build(rs(x),mid+1,r);
    push_up(x);
}
void pushdown(int x,int l,int r)
{
    int mid=(l+r)>>1;
    add[ls(x)]=(add[ls(x)]+add[x])%p;
    add[rs(x)]=(add[rs(x)]+add[x])%p;
    ans[ls(x)]=(ans[ls(x)]+(add[x]*(mid-l+1))%p)%p;
    ans[rs(x)]=(ans[rs(x)]+(add[x]*(r-mid))%p)%p;
    add[x]=0;
}
void addup(int nl,int nr,int l,int r,int x,int k)
{
    if(nl<=l&&r<=nr)
    {
        ans[x]=(ans[x]+(k*(r-l+1))%p)%p;
        add[x]=(add[x]+k)%p;
        return ;
    }
    pushdown(x,l,r);
    int mid=(l+r)>>1;
    if(nl<=mid)addup(nl,nr,l,mid,ls(x),k);
    if(mid+1<=nr)addup(nl,nr,mid+1,r,rs(x),k);
    push_up(x);
}
int query(int ql,int qr,int l,int r,int x)
{
    int res=0;
    if(ql<=l&&r<=qr)return ans[x]%p;
    pushdown(x,l,r);
    int mid=(l+r)>>1;
    if(ql<=mid)res+=query(ql,qr,l,mid,ls(x));
    if(mid+1<=qr)res+=query(ql,qr,mid+1,r,rs(x));
    return res%p;
}
void addxy(int x,int y,int z)
{
    while(top[x]!=top[y])
    {
        if(dep[top[x]]<dep[top[y]])swap(x,y);
        addup(st[top[x]],st[x],1,n,1,z);
        x=fa[top[x]];
    }
    if(st[x]>st[y])swap(x,y);
    addup(st[x],st[y],1,n,1,z);
}
int queryxy(int x,int y)
{
    int res=0;
    while(top[x]!=top[y])
    {
        if(dep[top[x]]<dep[top[y]])swap(x,y);
        res+=query(st[top[x]],st[x],1,n,1),res%=p;
        x=fa[top[x]];
    }
    if(st[x]>st[y])swap(x,y);
    res+=query(st[x],st[y],1,n,1);
    return res%p;
}
int main()
{
    scan(n,m,root,p);
    for(int i=1;i<=n;i++)
    {
        scan(a[i]);a[i]%=p;
    }
    for(int i=1,u,v;i<n;i++)
    {
        scan(u,v);
        adde(u,v);
    }
    dfs1(root);dfs2(root);
    build(1,1,n);
    for(int i=1,opt,x,y,z;i<=m;i++)
    {
        scan(opt);
        if(opt==1)
        {
            scan(x,y,z);z%=p;
            addxy(x,y,z);
        }
        if(opt==2)
        {
            scan(x,y);
            print(queryxy(x,y)),putchar('\n');
        }
        if(opt==3)
        {
            scan(x,z);z%=p;
            addup(st[x],ed[x],1,n,1,z);
        }
        if(opt==4)
        {
            scan(x);
            print(query(st[x],ed[x],1,n,1)),putchar('\n');
        }
    }
}
```