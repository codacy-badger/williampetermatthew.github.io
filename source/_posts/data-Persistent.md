---
title: 可持久化数据结构
date: 2019-01-07 17:51:20
tags: 
  - 数据结构
---

# 主席树（可持久化线段树）
## 权值线段树
普通的线段树维护的是单点的值，比方说一个数组是{1,1,2,4,2,4,3,4}，开成普通线段树长这样

![](https://res.zhangkai.xin/pic/2019-01-09_07-51-59-621000.png)

而权值线段树维护的是这个数出现了几次，就比方说上面的数组维护成了这样

![](https://res.zhangkai.xin/pic/2019-01-09_07-55-44-679000.png)
## 主席树
现在我们在树中插入一个数2

![](https://res.zhangkai.xin/pic/2019-01-09_08-10-02-152000.png)

观察修改过后的权值线段树，发现只有红色的链有更改，所以我们有一个大胆的想法：可不可以每次只建一个链，由于根节点肯定不一样所以我们保存根节点的信息的数组就是保存版本信息的数组，这样就可以回到之前的版本，就十分的方便了。

![](https://res.zhangkai.xin/pic/2019-01-09_08-12-58-370000.png)

这就是所谓的主席树即可持久化线段树。

板子如下
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
int n,q,m,tot=0,rt[200005],a[200005],p[200005];
//n是原数个数，q是询问数，m是去重个数（即权值线段树叶子节点数），tot是节点总个数，rt是根版本号，a是原数组，p是去重数组 
int son[200000<<6][2],size[200000<<6];
//son和size是线段树动态开点保存的信息，左右儿子和子树大小 
void build(int &rt,int l,int r)
{
    rt=++tot;
    int mid=(l+r)>>1;
    if(l==r)return;
    build(son[rt][0],l,mid);
	build(son[rt][1],mid+1,r);
}//建立原树 
void newpoint(int &rt,int old,int l,int r,int val)
{
	rt=++tot;
    son[rt][0]=son[old][0];
    son[rt][1]=son[old][1];
    size[rt]=size[old]+1;
    int mid=(l+r)>>1;
    if(l==r)return;
    if(val<=mid)newpoint(son[rt][0],son[old][0],l,mid,val);
    else newpoint(son[rt][1],son[old][1],mid+1,r,val);
}//建立新版本 
int query(int rt1,int rt2,int l,int r,int k)
{
    int mid=(l+r)>>1;
    if(l==r) return l;
    if(size[son[rt2][0]]-size[son[rt1][0]]>=k)return query(son[rt1][0],son[rt2][0],l,mid,k);
	else return query(son[rt1][1],son[rt2][1],mid+1,r,k-size[son[rt2][0]]+size[son[rt1][0]]);
}//询问

int main()
{
	scan(n,q);
    for(int i=1;i<=n;i++)
		scan(a[i]),p[i]=a[i];
	sort(p+1,p+n+1);
	m=unique(p+1,p+n+1)-p-1;
	build(rt[0],1,m);
	//去重建权值线段树 
    for(int i=1,pos;i<=n;i++)
	{
		pos=lower_bound(p+1,p+m+1,a[i])-p;
		newpoint(rt[i],rt[i-1],1,m,pos);
	}//主席树的每一个版本都是一个权值线段树 
    for(int i=1,x,y,k;i<=q;i++)
    {
    	scan(x,y,k);
        print(p[query(rt[x-1],rt[y],1,m,k)]),putchar('\n');
    }//类似前缀和思想，求y-(x-1)==k排名的数即在[x,y]上排名第k的数 
    return 0;
}
```
# 可持久化数组
可持久化数组其实就是记录数组的历史版本，随时调用，随时修改。  
所以我们可以利用主席树来维护，只不过主席树这次维护的不是第k名而是单点的值。
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
int n,q,tot=0,rt[1000005],a[1000005];
//n是原数个数，q是操作数，tot是节点总个数，rt是根版本号，a是原数组
int son[1000005*20][2],val[1000005*20];
//son和val是线段树动态开点保存的信息，左右儿子和单点的值 
void build(int &rt,int l,int r)
{
    rt=++tot;
    int mid=(l+r)>>1;
    if(l==r)
	{
		val[rt]=a[l];
		return;
	}
    build(son[rt][0],l,mid);
	build(son[rt][1],mid+1,r);
}//建立原树 
void newpoint(int &rt,int old,int l,int r,int x,int w)
{
	rt=++tot;
	son[rt][0]=son[old][0];
	son[rt][1]=son[old][1];
	val[rt]=val[old];
    int mid=(l+r)>>1;
    if(l==r)
	{
		val[rt]=w;
		return;
	}
    if(x<=mid)newpoint(son[rt][0],son[old][0],l,mid,x,w);
    else newpoint(son[rt][1],son[old][1],mid+1,r,x,w);
}//建立新版本 
int query(int rt,int l,int r,int x)
{
    int mid=(l+r)>>1;
    if(l==r)return val[rt];
    if(x<=mid)return query(son[rt][0],l,mid,x);
 	else return query(son[rt][1],mid+1,r,x);
}//询问
int main()
{
	scan(n,q);
    for(int i=1;i<=n;i++)
		scan(a[i]);
	build(rt[0],1,n);
    for(int i=1,v,op,x,w;i<=q;i++)
    {
        scan(v,op,x);
  		if(op==1)scan(w),newpoint(rt[i],rt[v],1,n,x,w);
  		if(op==2)print(query(rt[i]=rt[v],1,n,x)),putchar('\n');
    }
    return 0;
}
```
# 可持久化并查集
可持久化并查集建立与可持久化数组的基础上，但是由于我们要建立可持久化，所以不能路径压缩。

所以我们考虑基本不常用的按秩合并，我们利用可持久化数组（基于主席树），维护并查集信息

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
int n,q,tot=0,rt[200005],a[200005];
//n是原数个数，q是操作数，tot是节点总个数，rt是根版本号，a是原数组
int son[200000*20][2],fa[200000*20],level[200000*20];
//son、fa、level是按秩合并的 
void build(int &rt,int l,int r)
{
	rt=++tot;
	int mid=(l+r)>>1;
	if(l==r)
	{
		fa[rt]=l;
		return;
	}
	build(son[rt][0],l,mid);
	build(son[rt][1],mid+1,r);
}//建立原树 
void newpoint(int &rt,int old,int l,int r,int x,int father)
{
	rt=++tot;
	int mid=(l+r)>>1;
	if(l==r)
	{
		fa[rt]=father;
		level[rt]=level[old];
		return;
	}
	son[rt][0]=son[old][0];
	son[rt][1]=son[old][1];
	if(x<=mid)newpoint(son[rt][0],son[old][0],l,mid,x,father);
	else newpoint(son[rt][1],son[old][1],mid+1,r,x,father);
}//建立新版本 
void addlevel(int rt,int l,int r,int x)
{
	int mid=(l+r)>>1;
	if(l==r)
	{
		++level[rt];
		return;
	}
	if(x<=mid)addlevel(son[rt][0],l,mid,x);
	else addlevel(son[rt][1],mid+1,r,x);
}//增加优先级 
int query(int rt,int l,int r,int x)
{
	int mid=(l+r)>>1;
	if(l==r)return rt;
	if(x<=mid)return query(son[rt][0],l,mid,x);
	else return query(son[rt][1],mid+1,r,x);
}//询问节点在版本上的位置 
int getfa(int rt,int x)
{
	int father=query(rt,1,n,x);
	return fa[father]==x?father:getfa(rt,fa[father]);
}//询问节点在版本上的祖先 
void connect(int v,int x,int y)
{
	int fx=getfa(rt[v],x),fy=getfa(rt[v],y);
	if(fx==fy)return;
	if(level[fx]<level[fy])swap(fx,fy);
	newpoint(rt[v],rt[v-1],1,n,fa[fy],fa[fx]);
	if(level[fx]==level[fy])
		addlevel(rt[v],1,n,fa[fx]);
}//在版本上连接连个集合 
int main()
{
    scan(n,q);
    build(rt[0],1,n);
    for(int i=1,op,x,y;i<=q;i++)
    {
    	scan(op);
    	switch(op)
    	{
    		case 1:scan(x,y);rt[i]=rt[i-1];connect(i,x,y);break;
    		case 2:scan(x);rt[i]=rt[x];break;
    		case 3:scan(x,y);rt[i]=rt[i-1];print(getfa(rt[i],x)==getfa(rt[i],y)),putchar('\n');break;
		}
    }
    return 0;
}
```

# 可持久化Trie树


# 可持久化平衡树
## 可持久化平衡树
## 可持久化文艺平衡树


# 可持久化状链表


------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。