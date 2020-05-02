---
title: 后缀自动机
date: 2019-01-19 14:43:38
tags: 
  - 字符串
---

普通的后缀自动机

代码如下：  
洛谷P3975 [TJOI2015]弦论
```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=2010000;
char s[N];
int fail[N],son[N][26],len[N],siz[N],sum[N];
int rt=1,lst=rt,tot=1,n,t[N],A[N],tp,k;
void insert(int c)
{
    int fa=lst,now=++tot;
	lst=now;
    len[now]=len[fa]+1;
	siz[now]=1;
    while(fa&&!son[fa][c])
		son[fa][c]=now,fa=fail[fa];
    if(!fa)
	{
		fail[now]=rt;
		return;
	}
    int x=son[fa][c],y=++tot;
    if(len[fa]+1==len[x])
	{
		fail[now]=x;
		tot--;
		return;
	}
    len[y]=len[fa]+1;
	fail[y]=fail[x];
	fail[x]=fail[now]=y;
    memcpy(son[y],son[x],sizeof(son[y]));
    while(fa&&son[fa][c]==x)
		son[fa][c]=y,fa=fail[fa];
}
void work()
{
	for(int i=1;i<=tot;i++)
		t[len[i]]++;
    for(int i=1;i<=tot;i++)
		t[i]+=t[i-1];
    for(int i=1;i<=tot;i++)
		A[t[len[i]]--]=i;
    for(int i=tot;i>=1;i--)
    {
        int now=A[i];
		if(tp)siz[fail[now]]+=siz[now];
		else siz[now]=1;
    }
	siz[rt]=0;
	for(int i=tot;i;i--)
	{
		sum[A[i]]=siz[A[i]];
		for(int j=0;j<26;j++)
			if(son[A[i]][j])
				sum[A[i]]+=sum[son[A[i]][j]];
	}
}
main()
{
    scanf("%s%d%d",s+1,&tp,&k);
	n=strlen(s+1);
    for(int i=1;i<=n;i++)
		insert(s[i]-'a');
    work();
	if(k>sum[rt])
	{
		printf("-1\n");
		return 0;
	}
	int now=rt;
	while((k-=siz[now])>0)
	{
		int p=0;
		while(k>sum[son[now][p]])k-=sum[son[now][p++]];
		now=son[now][p];
		putchar('a'+p);
	}
    return 0;
}
```

广义后缀自动机

代码如下：  
洛谷P2336 [SCOI2012]喵星球上的点名
```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=600005;
char s[N];
int fail[N],len[N],siz[N],sum[N];
int rt=1,lst,tot=1,n,m,t[N],A[N],len1[N],len2[N],las[N];
int str[N],marked[N],ans[N];
map<int,int>son[N];
void insert(int c)
{
    int fa=lst,now=++tot;
    lst=now;
    len[now]=len[fa]+1;
    while(fa&&!son[fa][c])
        son[fa][c]=now,fa=fail[fa];
    if(!fa)
    {
        fail[now]=rt;
        return;
    }
    int x=son[fa][c],y=++tot;
    if(len[fa]+1==len[x])
    {
        fail[now]=x;
        tot--;
        return;
    }
    len[y]=len[fa]+1;
    fail[y]=fail[x];
    fail[x]=fail[now]=y;
    son[y]=son[x];
    while(fa&&son[fa][c]==x)
        son[fa][c]=y,fa=fail[fa];
}
void updata1(int x,int y)
{
    for(;x&&las[x]!=y;x=fail[x])
    {
        siz[x]++;
        las[x]=y;
    }
}
void updata2(int x,int y)
{
    for(;x&&las[x]!=y;x=fail[x])
    {
        ans[y]+=marked[x];
        las[x]=y;
    }
}
main()
{
    scanf("%d%d",&n,&m);
    int tmp=0;
    for(int i=1;i<=n;i++)
    {
        scanf("%d",&len1[i]);
        lst=rt;
        for(int j=1;j<=len1[i];j++)
        {
            scanf("%d",&str[++tmp]);
            insert(str[tmp]);
        }
        scanf("%d",&len2[i]);
        lst=rt;
        for(int j=1;j<=len2[i];j++)
        {
            scanf("%d",&str[++tmp]);
            insert(str[tmp]);
        }
    }
    tmp=0;
    memset(las,0,sizeof(las));
    for(int i=1;i<=n;i++)
    {
        for(int j=1,x=rt;j<=len1[i];j++)updata1(x=son[x][str[++tmp]],i);
        for(int j=1,x=rt;j<=len2[i];j++)updata1(x=son[x][str[++tmp]],i);
    }
    for(int i=1,le,x;i<=m;i++)
    {
        scanf("%d",&le);
        bool tag=0;x=rt;
        for(int j=1,d;j<=le;j++)
        {
            scanf("%d",&d);
            if(!tag)
            {
                if(son[x][d])
                    x=son[x][d];
                else
                    tag=1;
            }
        }
        if(!tag)
        {
            marked[x]++;
            printf("%d\n",siz[x]);
        }
        else
            printf("0\n");
    }
    tmp=0;
    memset(las,0,sizeof(las));
    for(int i=1;i<=n;i++)
    {
        for(int j=1,x=rt;j<=len1[i];j++)updata2(x=son[x][str[++tmp]],i);
        for(int j=1,x=rt;j<=len2[i];j++)updata2(x=son[x][str[++tmp]],i);
    }
    for(int i=1;i<=n;i++)
        printf("%d ",ans[i]);
    return 0;
}
```



------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。