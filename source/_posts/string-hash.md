---
title: 字符串哈希
date: 2019-01-14 09:26:53
tags: 
  - 字符串
---

字符串万能解法

模板题P3370
# 单哈
```cpp
#include<bits/stdc++.h>
using namespace std;
const long long base=63ll;
const long long mod=1e9+7;
int n,ans;
char s[1505];
int v[10005];
int hash(char *s)
{
	long long tmp=0ll;
	for(int i=0;i<strlen(s);i++)
	{
		tmp=(tmp*base+s[i])%mod;
	}
	return tmp;
}
int main()
{
	scanf("%d",&n);
	for(int i=1;i<=n;i++)
	{
		scanf("%s",s);
		v[i]=hash(s);
	}
	sort(v+1,v+n+1);
	int ans=0;
	for(int i=1;i<=n;i++)
		if(v[i]!=v[i-1])
			ans++;
	printf("%d",ans);
}
```
# 双哈
```cpp
#include<bits/stdc++.h>
using namespace std;
const long long base=63ll;
const long long mod1=1e9+7;
const long long mod2=1e9+9;
int n,ans;
char s[1505];
struct hashta
{
	int v1,v2;
	friend bool operator<(hashta a,hashta b)
	{
		return a.v1==b.v1?a.v2<b.v2:a.v1<b.v1;
	}
	friend bool operator!=(hashta a,hashta b)
	{
		return a.v1!=b.v1||a.v2!=b.v2;
	}
}v[10005];
int hash1(char *s)
{
	long long tmp=0ll;
	for(int i=0;i<strlen(s);i++)
	{
		tmp=(tmp*base+s[i])%mod1;
	}
	return tmp;
}
int hash2(char *s)
{
	long long tmp=0ll;
	for(int i=0;i<strlen(s);i++)
	{
		tmp=(tmp*base+s[i])%mod2;
	}
	return tmp;
}
int main()
{
	scanf("%d",&n);
	for(int i=1;i<=n;i++)
	{
		scanf("%s",s);
		v[i]=(hashta){hash1(s),hash2(s)};
	}
	sort(v+1,v+n+1);
	int ans=0;
	for(int i=1;i<=n;i++)
		if(v[i]!=v[i-1])
			ans++;
	printf("%d",ans);
}
```
# $O(n)$处理一维串，$O(1)$提取子串哈希
## 单哈
```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
int n,m,q;
const int mod=1e7+7,base=2;
int qb[1005];
int sa[1005];
int ht[1005];
bool h[10000010];
char s[1005];
void hashta()
{
	for(int i=1;i<=n;i++)
		ht[i]=((ht[i-1]*base)%mod+sa[i])%mod;
	for(int i=m;i<=n;i++)
	{
		int tmp=((ht[i]-(ht[i-m]*qb[m])%mod)%mod+mod)%mod;
		h[tmp]=1;
		// cerr<<tmp<<endl;
	}
}
int hashon()
{
	for(int i=1;i<=m;i++)
		ht[i]=((ht[i-1]*base)%mod+sa[i])%mod;
	// cerr<<ht[m]<<endl;
	return h[ht[m]];
}
void pre()
{
	qb[0]=1;
	for(int i=1;i<1005;i++)
		qb[i]=(qb[i-1]*base)%mod;
}
signed main()
{
	pre();
	scanf("%d%d",&n,&m);
	scanf("%s",s+1);
	for(int i=1;i<=n;i++)
		sa[i]=s[i]-'0';
	hashta();
	scanf("%d",&q);
	for(int t=1;t<=q;t++)
	{
		scanf("%s",s+1);
		for(int i=1;i<=n;i++)
			sa[i]=s[i]-'0';
		printf("%d\n",hashon());
	}
}
```
## 双哈
```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
int n,m,q;
const int moda=1e7+7,basea=2;
const int modb=1e7+9,baseb=7;
int qba[1005];
int qbb[1005];
int sa[1005];
int ht[1005];
bool hasha[10000010];
bool hashb[10000010];
char s[1005];
void hashta()
{
	for(int i=1;i<=n;i++)
		ht[i]=((ht[i-1]*basea)%moda+sa[i])%moda;
	for(int i=m;i<=n;i++)
	{
		int tmp=((ht[i]-(ht[i-m]*qba[m])%moda)%moda+moda)%moda;
		hasha[tmp]=1;
		// cerr<<tmp<<endl;
	}
}
void hashtb()
{
	for(int i=1;i<=n;i++)
		ht[i]=((ht[i-1]*baseb)%modb+sa[i])%modb;
	for(int i=m;i<=n;i++)
	{
		int tmp=((ht[i]-(ht[i-m]*qbb[m])%modb)%modb+modb)%modb;
		hashb[tmp]=1;
		// cerr<<tmp<<endl;
	}
}
int hashon()
{
	for(int i=1;i<=m;i++)
		ht[i]=((ht[i-1]*basea)%moda+sa[i])%moda;
	// cerr<<ht[m]<<endl;
	bool ta=hasha[ht[m]];
	for(int i=1;i<=m;i++)
		ht[i]=((ht[i-1]*baseb)%modb+sa[i])%modb;
	// cerr<<ht[m]<<endl;
	bool tb=hashb[ht[m]];
	return ta&&tb;
}
void pre()
{
	qba[0]=1;
	for(int i=1;i<1005;i++)
		qba[i]=(qba[i-1]*basea)%moda;
	qbb[0]=1;
	for(int i=1;i<1005;i++)
		qbb[i]=(qbb[i-1]*baseb)%modb;
}
signed main()
{
	pre();
	scanf("%d%d",&n,&m);
	scanf("%s",s+1);
	for(int i=1;i<=n;i++)
		sa[i]=s[i]-'0';
	hashta();
	hashtb();
	scanf("%d",&q);
	for(int t=1;t<=q;t++)
	{
		scanf("%s",s+1);
		for(int i=1;i<=n;i++)
			sa[i]=s[i]-'0';
		printf("%d\n",hashon());
	}
}
```
# $O(n^2)$处理二维串，$O(1)$提取子矩阵哈希
[Contest Hunter 1806](http://contest-hunter.org:83/contest/0x18%E3%80%8C%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E3%80%8D%E7%BB%83%E4%B9%A0/1806%20Matrix)
## 单哈
pts:90（其实模数好的话可以AC）
```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
int n,m,a,b,q;
const int mod=1e7+7,base1=2,base2=9191891;
int qb1[1005],qb2[1005];
int sa[1005][1005];
int ht[1005][1005];
bool h[10000010];
char s[1005][1005];
void hashta()
{
	for(int i=1;i<=n;i++)
		for(int j=1;j<=m;j++)
			ht[i][j]=((ht[i][j-1]*base1)%mod+sa[i][j])%mod;
	for(int i=1;i<=n;i++)
		for(int j=1;j<=m;j++)
			ht[i][j]=((ht[i-1][j]*base2)%mod+ht[i][j])%mod;
	for(int i=a;i<=n;i++)
		for(int j=b;j<=m;j++)
		{
			int tmp=((ht[i][j]-(ht[i][j-b]*qb1[b])%mod-(ht[i-a][j]*qb2[a])%mod+((ht[i-a][j-b]*qb1[b])%mod*qb2[a])%mod)%mod+mod)%mod;
			h[tmp]=1;
			// cerr<<tmp<<endl;
		}
}
int hashon()
{
	for(int i=1;i<=a;i++)
		for(int j=1;j<=b;j++)
			ht[i][j]=((ht[i][j-1]*base1)%mod+sa[i][j])%mod;
	for(int i=1;i<=a;i++)
		for(int j=1;j<=b;j++)
			ht[i][j]=((ht[i-1][j]*base2)%mod+ht[i][j])%mod;
	// cerr<<ht[a][b]<<endl;
	return h[ht[a][b]];
}
void pre()
{
	qb1[0]=1;
	qb2[0]=1;
	for(int i=1;i<1005;i++)
		qb1[i]=(qb1[i-1]*base1)%mod;
	for(int i=1;i<1005;i++)
		qb2[i]=(qb2[i-1]*base2)%mod;
}
signed main()
{
	pre();
	scanf("%d%d%d%d",&n,&m,&a,&b);
	for(int i=1;i<=n;i++)
		scanf("%s",s[i]+1);
	for(int i=1;i<=n;i++)
		for(int j=1;j<=m;j++)
			sa[i][j]=s[i][j]-'0';
	hashta();
	memset(ht,0,sizeof(ht));
	scanf("%d",&q);
	for(int t=1;t<=q;t++)
	{
		for(int i=1;i<=a;i++)
			scanf("%s",s[i]+1);
		for(int i=1;i<=a;i++)
			for(int j=1;j<=b;j++)
				sa[i][j]=s[i][j]-'0';
		printf("%d\n",hashon());
	}
}
```
## 双哈
pts:100
```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
int n,m,a,b,q;
const int moda=1e7+7,basea1=2,basea2=9191891;
const int modb=1e7+9,baseb1=7,baseb2=6498497;
int qba1[1005],qba2[1005];
int qbb1[1005],qbb2[1005];
int sa[1005][1005];
int ht[1005][1005];
bool hasha[10000010];
bool hashb[10000010];
char s[1005][1005];
void hashta()
{
	for(int i=1;i<=n;i++)
		for(int j=1;j<=m;j++)
			ht[i][j]=((ht[i][j-1]*basea1)%moda+sa[i][j])%moda;
	for(int i=1;i<=n;i++)
		for(int j=1;j<=m;j++)
			ht[i][j]=((ht[i-1][j]*basea2)%moda+ht[i][j])%moda;
	for(int i=a;i<=n;i++)
		for(int j=b;j<=m;j++)
		{
			int tmp=((ht[i][j]-(ht[i][j-b]*qba1[b])%moda-(ht[i-a][j]*qba2[a])%moda+((ht[i-a][j-b]*qba1[b])%moda*qba2[a])%moda)%moda+moda)%moda;
			hasha[tmp]=1;
			// cerr<<tmp<<endl;
		}
}
void hashtb()
{
	for(int i=1;i<=n;i++)
		for(int j=1;j<=m;j++)
			ht[i][j]=((ht[i][j-1]*baseb1)%modb+sa[i][j])%modb;
	for(int i=1;i<=n;i++)
		for(int j=1;j<=m;j++)
			ht[i][j]=((ht[i-1][j]*baseb2)%modb+ht[i][j])%modb;
	for(int i=a;i<=n;i++)
		for(int j=b;j<=m;j++)
		{
			int tmp=((ht[i][j]-(ht[i][j-b]*qbb1[b])%modb-(ht[i-a][j]*qbb2[a])%modb+((ht[i-a][j-b]*qbb1[b])%modb*qbb2[a])%modb)%modb+modb)%modb;
			hashb[tmp]=1;
			// cerr<<tmp<<endl;
		}
}
int hashon()
{
	for(int i=1;i<=a;i++)
		for(int j=1;j<=b;j++)
			ht[i][j]=((ht[i][j-1]*basea1)%moda+sa[i][j])%moda;
	for(int i=1;i<=a;i++)
		for(int j=1;j<=b;j++)
			ht[i][j]=((ht[i-1][j]*basea2)%moda+ht[i][j])%moda;
	// cerr<<ht[a][b]<<endl;
	bool ta=hasha[ht[a][b]];
	for(int i=1;i<=a;i++)
		for(int j=1;j<=b;j++)
			ht[i][j]=((ht[i][j-1]*baseb1)%modb+sa[i][j])%modb;
	for(int i=1;i<=a;i++)
		for(int j=1;j<=b;j++)
			ht[i][j]=((ht[i-1][j]*baseb2)%modb+ht[i][j])%modb;
	// cerr<<ht[a][b]<<endl;
	bool tb=hashb[ht[a][b]];
	return ta&&tb;
}
void pre()
{
	qba1[0]=1;
	qba2[0]=1;
	for(int i=1;i<1005;i++)
		qba1[i]=(qba1[i-1]*basea1)%moda;
	for(int i=1;i<1005;i++)
		qba2[i]=(qba2[i-1]*basea2)%moda;
	qbb1[0]=1;
	qbb2[0]=1;
	for(int i=1;i<1005;i++)
		qbb1[i]=(qbb1[i-1]*baseb1)%modb;
	for(int i=1;i<1005;i++)
		qbb2[i]=(qbb2[i-1]*baseb2)%modb;
}
signed main()
{
	pre();
	scanf("%d%d%d%d",&n,&m,&a,&b);
	for(int i=1;i<=n;i++)
		scanf("%s",s[i]+1);
	for(int i=1;i<=n;i++)
		for(int j=1;j<=m;j++)
			sa[i][j]=s[i][j]-'0';
	hashta();
	hashtb();
	scanf("%d",&q);
	for(int t=1;t<=q;t++)
	{
		for(int i=1;i<=a;i++)
			scanf("%s",s[i]+1);
		for(int i=1;i<=a;i++)
			for(int j=1;j<=b;j++)
				sa[i][j]=s[i][j]-'0';
		printf("%d\n",hashon());
	}
}
```

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。