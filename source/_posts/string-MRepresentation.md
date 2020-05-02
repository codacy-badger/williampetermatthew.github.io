---
title: 最小表示法/最大表示法
date: 2019-02-15 15:43:43
tags: 
  - 字符串
---

我们可以求出最小/最大开始的位置然后输出。

代码如下：
```cpp
int getMin(char *s)  
{  
	int i=1,j=2,k=0; 
	int len=strlen(s+1);
	while(i<=len&&j<=len&&k<len)
	{  
		int t=s[(i+k-1)%len+1]-s[(j+k-1)%len+1];  
		if(!t)k++;
		else
		{  
			if(t>0)i=i+k+1;  
			else j=j+k+1;  
			if(i==j)j++;  
			k=0;  
		}  
	}  
	return min(i,j);  
}
int getMax(char *s)  
{  
	int i=1,j=2,k=0; 
	int len=strlen(s+1);
	while(i<=len&&j<=len&&k<len)
	{  
		int t=s[(i+k-1)%len+1]-s[(j+k-1)%len+1];  
		if(!t)k++;  
		else
		{  
			if(t>0)j=j+k+1;
			else i=i+k+1;
			if(i==j)j++;
			k=0;
		}  
	}  
	return min(i,j);
}
```

下面是一份0下标开始的版本
```cpp
int getMin(char *s)  
{  
	int i=0,j=1,k=0; 
	int len=strlen(s);
	while(i<len&&j<len&&k<len)
	{  
		int t=s[(i+k)%len]-s[(j+k)%len];  
		if(!t)k++;  
		else
		{  
			if(t>0)i=i+k+1;  
			else j=j+k+1;  
			if(i==j)j++;  
			k=0;  
		}  
	}  
	return min(i,j);  
}
int getMax(char *s)  
{  
	int i=0,j=1,k=0; 
	int len=strlen(s);
	while(i<len&&j<len&&k<len)
	{  
		int t=s[(i+k)%len]-s[(j+k)%len];  
		if(!t)k++;  
		else
		{  
			if(t>0)j=j+k+1;
			else i=i+k+1;
			if(i==j)j++;
			k=0;
		}  
	}  
	return min(i,j);  
}
```

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。