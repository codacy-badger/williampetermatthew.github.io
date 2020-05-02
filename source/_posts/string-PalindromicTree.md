---
title: 回文自动机
date: 2019-02-15 15:39:19
tags: 
  - 字符串
---

类似AC自动机的一种回文串匹配自动机，也就是一棵字符树。准确的说，是两颗字符树，0号表示回文串长度为偶数的树，1号表示回文串长度为奇数的树。

洛谷上的模板题（[P3649 【APIO2014】回文串](https://www.luogu.org/problemnew/show/P3649)）：  
求的其实就是$max(cnt[i] \times len[i])$
```cpp
#include<bits/stdc++.h>
using namespace std;
char s[300005];
int fail[300005],ch[300005][26],len[300005],num[300005],cnt[300005],tot,last;
int newpoint(int l)
{
	++tot;
	memset(ch[tot],0,sizeof(ch[tot]));
	len[tot]=l;
	cnt[tot]=0;
	num[tot]=0;
	return tot;
}
void init()
{
	tot=-1;
	last=0;
	newpoint(0);
	newpoint(-1);
	s[0]=-1;
	fail[0]=1;
}
int getfail(int x,int l)
{
	while(s[l-len[x]-1]!=s[l])
		x=fail[x];
	return x;
}
void count()
{
	for(int i=tot;i>=0;i--)
		cnt[fail[i]]+=cnt[i];
}
void build(char *s)
{
	init();
	int l=strlen(s+1);
	for(int i=1;i<=l;i++)
	{
		int cur=getfail(last,i);
		if(!ch[cur][s[i]-'a'])
		{
			int now=newpoint(len[cur]+2);
			fail[now]=ch[getfail(fail[cur],i)][s[i]-'a'];
			ch[cur][s[i]-'a']=now;
			num[now]=num[fail[now]]+1;
		}
		last=ch[cur][s[i]-'a'];
		cnt[last]++;
	}
	count();
}
int main()
{
	scanf("%s",s+1);
	build(s);
	long long ans=0;
	for(int i=0;i<=tot;i++)
		ans=max(ans,(long long)cnt[i]*len[i]);
	printf("%lld\n",ans);
}
```

求本质不同的回文子串个数（HihoCoder-1602、[HDU3948](http://acm.hdu.edu.cn/showproblem.php?pid=3948)）：  
回文自动机上的每个点都是一个本质不同的回文子串，所以就是求回文自动机总点的个数，同时注意刚开始有两个点0和1不是串上的点，所以统计答案时去掉这两个点的影响。HDU的题要注意从头再来时的清空。
```cpp
// #include<bits/stdc++.h>
#include<cstdio>
#include<cstring>
using namespace std;
char s[300005];
int fail[300005],ch[300005][26],len[300005],num[300005],cnt[300005],tot,last;
int newpoint(int l)
{
	++tot;
	memset(ch[tot],0,sizeof(ch[tot]));
	len[tot]=l;
	cnt[tot]=0;
	num[tot]=0;
	return tot;
}
void init()
{
	tot=-1;
	last=0;
	newpoint(0);
	newpoint(-1);
	s[0]=-1;
	fail[0]=1;
}
int getfail(int x,int l)
{
	while(s[l-len[x]-1]!=s[l])
		x=fail[x];
	return x;
}
void count()
{
	for(int i=tot;i>=0;i--)
		cnt[fail[i]]+=cnt[i];
}
void build(char *s)
{
	init();
	int l=strlen(s+1);
	for(int i=1;i<=l;i++)
	{
		int cur=getfail(last,i);
		if(!ch[cur][s[i]-'a'])
		{
			int now=newpoint(len[cur]+2);
			fail[now]=ch[getfail(fail[cur],i)][s[i]-'a'];
			ch[cur][s[i]-'a']=now;
			num[now]=num[fail[now]]+1;
		}
		last=ch[cur][s[i]-'a'];
		cnt[last]++;
	}
	count();
}
int main()
{
	int T;
	scanf("%d",&T);
	for(int i=1;i<=T;i++)
	{
		scanf("%s",s+1);
		build(s);
		printf("Case #%d: %d\n",i,tot-1);
	}
}
```

统计所有回文串的个数（HihoCoder-1589）：  
即cnt之和
```cpp
#include<bits/stdc++.h>
using namespace std;
char s[800005];
int fail[800005],ch[800005][26],len[800005],num[800005],cnt[800005],tot,last;
int newpoint(int l)
{
	++tot;
	memset(ch[tot],0,sizeof(ch[tot]));
	len[tot]=l;
	cnt[tot]=0;
	num[tot]=0;
	return tot;
}
void init()
{
	tot=-1;
	last=0;
	newpoint(0);
	newpoint(-1);
	s[0]=-1;
	fail[0]=1;
}
int getfail(int x,int l)
{
	while(s[l-len[x]-1]!=s[l])
		x=fail[x];
	return x;
}
void count()
{
	for(int i=tot;i>=0;i--)
		cnt[fail[i]]+=cnt[i];
}
void build(char *s)
{
	init();
	int l=strlen(s+1);
	for(int i=1;i<=l;i++)
	{
		int cur=getfail(last,i);
		if(!ch[cur][s[i]-'a'])
		{
			int now=newpoint(len[cur]+2);
			fail[now]=ch[getfail(fail[cur],i)][s[i]-'a'];
			ch[cur][s[i]-'a']=now;
			num[now]=num[fail[now]]+1;
		}
		last=ch[cur][s[i]-'a'];
		cnt[last]++;
	}
	count();
}
int main()
{
	scanf("%s",s+1);
	build(s);
	long long ans=0;
	for(int i=2;i<=tot;i++)
		ans+=cnt[i];
	printf("%lld\n",ans);
}
```

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。