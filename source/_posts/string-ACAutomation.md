---
title: AC自动机
date: 2019-01-14 09:32:46	
tags: 
  - 字符串
---

哇！AC自动机，莫非可以自动AC题目？不，这不可以，~~不过可以参考我的一篇自动AC机的文章尝试自动AC题目，~~叫AC自动机的原因是因为它的发明者名为Aho-Corasick，缩写为AC。。。

# AC自动机简单版
```cpp
#include<bits/stdc++.h>
using namespace std;
int n;
char s[1000005];
int fail[1000005],ch[1000005][26],end[1000005],cnt;
void build(char *s)
{
	int l=strlen(s+1);
	int now=0;
	for(int i=1;i<=l;i++)
	{
		if(!ch[now][s[i]-'a'])
			ch[now][s[i]-'a']=++cnt;
		now=ch[now][s[i]-'a'];
	}
	end[now]+=1;
}
void getfail()
{
	queue<int>q;
	for(int i=0;i<26;i++)
	{
		if(ch[0][i])
		{
			fail[ch[0][i]]=0;
			q.push(ch[0][i]);
		}
	}
	while(!q.empty())
	{
		int x=q.front();q.pop();
		for(int i=0;i<26;i++)
		{
			if(ch[x][i])
			{
				fail[ch[x][i]]=ch[fail[x]][i];
				q.push(ch[x][i]);
			}
			else
				ch[x][i]=ch[fail[x]][i];
		}
	}
}
int acquery(char *s)
{
	int l=strlen(s+1);
	int now=0,ans=0;
	for(int i=1;i<=l;i++)
	{
		now=ch[now][s[i]-'a'];
		for(int t=now;t&&(~end[t]);t=fail[t])
		{
			ans+=end[t];
			end[t]=-1;
		}
	}
	return ans;
}
int main()
{
	scanf("%d",&n);
	for(int i=1;i<=n;i++)
	{
		scanf("%s",s+1);
		build(s);
	}
	fail[0]=0;
	getfail();
	scanf("%s",s+1);
	printf("%d\n",acquery(s));
}
```

# AC自动机加强版
```cpp
#include<bits/stdc++.h>
using namespace std;
int n;
char s[155][75],t[1000005];
int fail[1000005],ch[1000005][26],end[1000005],cnt;
struct answer
{
	int num,pos;
}a[155];
bool operator<(answer a,answer b)
{
	return a.num==b.num?a.pos<b.pos:a.num>b.num;
}
void clean(int x)
{
	memset(ch[x],0,sizeof(ch[x]));
	fail[x]=0;
	end[x]=0;
}
void build(char *s,int num)
{
	int l=strlen(s+1);
	int now=0;
	for(int i=1;i<=l;i++)
	{
		if(!ch[now][s[i]-'a'])
			ch[now][s[i]-'a']=++cnt,clean(cnt);
		now=ch[now][s[i]-'a'];
	}
	end[now]=num;
}
void getfail()
{
	queue<int>q;
	for(int i=0;i<26;i++)
	{
		if(ch[0][i])
		{
			fail[ch[0][i]]=0;
			q.push(ch[0][i]);
		}
	}
	while(!q.empty())
	{
		int x=q.front();q.pop();
		for(int i=0;i<26;i++)
		{
			if(ch[x][i])
			{
				fail[ch[x][i]]=ch[fail[x]][i];
				q.push(ch[x][i]);
			}
			else
				ch[x][i]=ch[fail[x]][i];
		}
	}
}
int acquery(char *s)
{
	int l=strlen(s+1);
	int now=0,ans=0;
	for(int i=1;i<=l;i++)
	{
		now=ch[now][s[i]-'a'];
		for(int t=now;t;t=fail[t])
			a[end[t]].num++;
	}
	return ans;
}
int main()
{
	while(scanf("%d",&n)&&n)
	{
		cnt=0,clean(cnt);
		for(int i=1;i<=n;i++)
		{
			scanf("%s",s[i]+1);
			a[i].num=0;
			a[i].pos=i;
			build(s[i],i);
		}
		fail[0]=0;
		getfail();
		scanf("%s",t+1);
		acquery(t);
		sort(a+1,a+n+1);
		printf("%d\n%s\n",a[1].num,s[a[1].pos]+1);
		for(int i=2;i<=n;i++)
		{
			if(a[i].num==a[i-1].num)
				printf("%s\n",s[a[i].pos]+1);
			else
				break;
		}
	}
}
```

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。