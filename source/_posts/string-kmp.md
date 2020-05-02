---
title: kmp
date: 2018-11-08 15:11:41	
tags: 
  - 字符串
---
字符串的经典类型有一项为单串匹配问题，这是指一个模式串在一个文本串中出现了多少次并求出出现的位置的一个问题。  
我们称模式串长为$n$，文本串长为$m$，那么有
- 理论复杂度下界$O(n+m)$。

# 考虑暴力
每次在文本串中的每一个位置从前往后匹配尝试是否正确。
- 对于随机数据来说十分优秀，甚至有时可以达到近$O(m)$的复杂度，但是会被 "aaaaaaaa"文本串和"aaa"模式串 这样的数据卡到$O(nm)$。

# kmp算法
我们考虑，我们希望找到一个方便的转移，使得我们模式串的当前一位匹配失败了可以及时地转到另一个地方匹配，这样就减少了不必要的匹配次数。  
![]()  
![]()  
![]()  
![]()  
![]()  
![]()  

代码如下：
我们从下标1开始存储（读入方式为scanf("%s",s+1);）
```cpp
//文本串为s1，模式串为s2
void getnext()
{
	int n=strlen(s2+1);
	next[1]=0;
	for(int i=2,j;i<=n;i++)
	{
		j=next[i-1];
		while(s2[i]!=s2[j+1]&&j)
			j=next[j];
		if(s2[i]==s2[j+1])
			next[i]=j+1;
		else
			next[i]=0;
	}
}
void kmp()
{
	getnext();
	int n=strlen(s2+1),m=strlen(s1+1);
	for(int i=1,j=0;i<=m;i++)
	{
		while(s1[i]!=s2[j+1]&&j)
			j=next[j];
		if(s1[i]==s2[j+1])
			j++;
		if(j==n)
			printf("%d\n",i-n+1);
	}
}
```



------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。
