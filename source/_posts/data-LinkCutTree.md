---
title: 动态树
date: 2019-01-04 20:55:54
tags: 
  - 数据结构
---

动态树的一些操作：  
_据说树剖能做的动态树都能做，但目前使（wo）用（zhi）最（xue）多（hui）的LCT不擅长处理子树操作~~（也有可能是我太菜了）~~_
- 单点修改/询问
- 路径询问
- 连边/删边
- ······

## 模板
```cpp
#include<bits/stdc++.h>
using namespace std;
int n,m,key[300005];
int top,ch[300005][2],fa[300005],xsm[300005],que[300005],rev[300005];
void pushup(int x){xsm[x]=xsm[ch[x][0]]^xsm[ch[x][1]]^key[x];}
void pushdown(int x)
{
    if(rev[x])
	{
        rev[ch[x][0]]^=1;rev[ch[x][1]]^=1;rev[x]^=1;
        swap(ch[x][0],ch[x][1]);
    }
}
bool isroot(int x){return ch[fa[x]][0]!=x&&ch[fa[x]][1]!=x;}
void rotate(int x)
{
    int y=fa[x],z=fa[y],l=(ch[y][1]==x),r=l^1;
    if(!isroot(y))
	{
		if(ch[z][0]==y)
			ch[z][0]=x;
		else
			ch[z][1]=x;
	}
    fa[x]=z;
	fa[y]=x;
	fa[ch[x][r]]=y;
    ch[y][l]=ch[x][r];
	ch[x][r]=y;
    pushup(y);
	pushup(x);
}
void splay(int x)
{
    top=0;
	que[++top]=x;
    for(int i=x;!isroot(i);i=fa[i])
		que[++top]=fa[i];
    for(int i=top;i;i--)
		pushdown(que[i]);
    while(!isroot(x))
	{
        int y=fa[x],z=fa[y];
        if(!isroot(y))
		{
            if((ch[y][0]==x)^(ch[z][0]==y))rotate(x);
            else rotate(y);
        }
		rotate(x);
    }
}
void access(int x)
{
	for(int t=0;x;t=x,x=fa[x])
	{
		splay(x);
		ch[x][1]=t;
		pushup(x);
	}
}
void makeroot(int x)
{
	access(x);
	splay(x);
	rev[x]^=1;
}
int find(int x)
{
	access(x);
	splay(x);
	while(ch[x][0])
		x=ch[x][0];
	return x;
}
void split(int x,int y)
{
	makeroot(x);
	access(y);
	splay(y);
}
void cut(int x,int y)
{
	split(x,y);
	if(ch[y][0]==x&&ch[x][1]==0)
		ch[y][0]=fa[x]=0;
}
void link(int x,int y)
{
	makeroot(x);
	fa[x]=y;
}
int main()
{
	scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++)
		scanf("%d",&key[i]),xsm[i]=key[i];
    for(int i=1,opt,x,y;i<=m;i++)
	{
		scanf("%d%d%d",&opt,&x,&y);
        if(opt==0)split(x,y),printf("%d\n",xsm[y]);
        if(opt==1)if(find(x)!=find(y))link(x,y);
        if(opt==2)if(find(x)==find(y))cut(x,y);
        if(opt==3)access(x),splay(x),key[x]=y,pushup(x);
    }
    return 0;
}
```

------------

> [![知识共享许可协议](https://t1.picb.cc/uploads/2018/09/06/JW1a8i.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://t1.picb.cc/uploads/2018/09/06/JW1bXL.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。