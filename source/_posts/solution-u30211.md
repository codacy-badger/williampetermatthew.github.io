---
title: 题解 U30211 【BZ游戏#6 纸壳箱游戏（ZKX Game）】
date: 2018-07-20 19:05:35	
tags: 
  - 题解
---
[LuoguU30211](https://www.luogu.org/problemnew/show/U30211):

DP+博弈论

限制一下取数，从一边取DP即可。

用f[i][j]表示该取i，上一次取了j个的最大得分，则因为所有块的权值都>0，所以只要用2$\times $j和2$\times $j-1来更新答案就可以了，具体DP方程见代码

```cpp
#include<bits/stdc++.h>
using namespace std;
int n,ans,a[2005],f[2005][2005];
int main()
{
	scanf("%d",&n);
	for(int i=n;i>=1;i--)
		scanf("%d",&a[i]);
	for(int i=2;i<=n;i++)
		a[i]+=a[i-1],f[i][i]=a[i];
	for(int i=1;i<=n;i++)
		for(int j=1;j<=n;j++)
		{
			f[i][j]=f[i][j-1];
	 		if(i>=2*j)
			 	f[i][j]=max(f[i][j],a[i]-f[i-2*j][2*j]);
			if(i+1>=2*j)
				f[i][j]=max(f[i][j],a[i]-f[i-2*j+1][2*j-1]);
		}
	printf("%d\n",f[n][1]);
	return 0;
}
```