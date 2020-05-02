---
title: 题解 U23374 【PP游戏#6 矩阵游戏（Matrix Game）】
date: 2018-04-15 17:25:16	
tags: 
  - 题解
---
[LuoguU23374](https://www.luogu.org/problemnew/show/U23374):

我在小学时订阅了一本杂志叫《趣味数学》（免费打个广告），这杂志里面就是讲了样例里面的这么个矩阵。他所讲的故事背景是一个人要吃火鸡，但是火鸡不愿意，于是他和火鸡打赌，要是火鸡按照规则选择数后加和为一个数，就让火鸡被吃，火鸡同意了，然后。。。就没有然后了，因为之后火鸡就被吃了。

下面进入正题，如何生成一个这样的矩阵呢？显然有多解，所以我写了个SPJ，为了方便我的SPJ，我出题时，限定了0的所在位置。

![](https://cdn.luogu.org/upload/pic/17539.png)

然后我们要做的是在该行该列其他位置填数使其和为所要值，就好像是样例#1中的51。当然数是随意的了。

![](https://cdn.luogu.org/upload/pic/17540.png)

然后其他格子用横行数列填好的数之和填就行，例如左上角18=13+5。

![](https://cdn.luogu.org/upload/pic/17541.png)

同理可按步生成样例#2的数。

![](https://cdn.luogu.org/upload/pic/17542.png)

![](https://cdn.luogu.org/upload/pic/17543.png)

![](https://cdn.luogu.org/upload/pic/17544.png)

然后就是愉快的贴代码了~
```cpp
#include<bits/stdc++.h>
using namespace std;
int n,m,x,y,bas,mor;
int a[105][105];
int randomint(int l,int r)
{
	return (rand()*32768+rand())%(r-l)+l;
}
int main()
{
// 	freopen("matrix.in","r",stdin);
// 	freopen("matrix.out","w",stdout);
	srand(time(0));
	scanf("%d%d%d%d",&n,&m,&x,&y);
	bas=m;
	for(int i=1;i<=n;i++)
	{
		if(i!=x)a[i][y]=randomint(1,bas/(n-i+1)),bas-=a[i][y];
		if(i!=y)a[x][i]=randomint(1,bas/(n-i+1)),bas-=a[x][i];
	}
	
	if(x!=1)a[1][y]+=bas;
	else a[n][y]+=bas;
	for(int i=1;i<=n;i++)
	{
		if(i!=x)
		{
			for(int j=1;j<=n;j++)
			{
				if(j!=y)
				{
					a[i][j]=a[i][y]+a[x][j];
				}
			}
		}
	}
	a[x][y]=0;
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=n;j++)
		{
			cout<<a[i][j]<<" ";
		}
		cout<<endl;
	}
	return 0;
}
```
哈哈，下面放彩蛋版（SPJ的返回话不同）
```cpp
#include<bits/stdc++.h>
using namespace std;
int n,m,x,y,bas,mor;
int a[105][105];
int main()
{
// 	freopen("matrix.in","r",stdin);
// 	freopen("matrix.out","w",stdout);
    scanf("%d%d%d%d",&n,&m,&x,&y);
    a[x][y]=0;
    bas=m/(n+n-2);
    if((n+n-2)*bas!=m)mor=m-(n+n-2)*bas;
    for(int i=1;i<=n;i++)
    {
        if(i!=x)a[i][y]=bas;
        if(i!=y)a[x][i]=bas;
    }
    if(x!=1)a[1][y]+=mor;
    else a[n][y]+=mor;
    for(int i=1;i<=n;i++)
    {
        if(i!=x)
        {
            for(int j=1;j<=n;j++)
            {
                if(j!=y)
                {
                    a[i][j]=a[i][y]+a[x][j];
                }
            }
        }
    }
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=n;j++)
        {
            cout<<a[i][j]<<" ";
        }
        cout<<endl;
    }
    return 0;
}
```