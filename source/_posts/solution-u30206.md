---
title: 题解 U30206 【BZ游戏#1 草雉剑游戏（CZJ Game）】
date: 2018-07-21 15:52:55	
tags: 
  - 题解
---
[LuoguU30206](https://www.luogu.org/problemnew/show/U30206):

k=1：直接全部异或起来。

k=2：假设出现奇数次的是a和b，那么把所有数异或起来得到的就是s=a^b。

对每个二进制位维护一个数xw[i]，当读入一个x，x的第j位为1的时候，就把xw[j]^=x。 

对于s的最高位的1，那么一定是a和b之中，一个这一位为1，一个这一位为0。假设是第j位，那么xw[j]肯定就是其中一个数。

```cpp
#include<bits/stdc++.h>
using namespace std;
int n,k,xsum,xw[35];
int main()
{
    scanf("%d%d",&n,&k);
    for(int i=1,x;i<=n;i++)
    {
        scanf("%d",&x);
        xsum^=x;
        for(int j=0;j<=31;j++)
			if(x&(1<<j))
				xw[j]^=x;
    }
    if(k==1)
		printf("%d\n",xsum);
    else
    {
        for(int j=31,x,y;j>=0;j--)
			if(xsum&(1<<j))
	        {
	            x=xw[j],y=xsum^xw[j];
	            if(x>y)
					swap(x,y);
	            printf("%d %d\n",x,y);
	            break;
	        }
    }
    return 0;
}

```