---
title: 线性基
date: 2019-3-19 21:56:08	
tags: 
  - 线性基
---

```cpp
void getlb(long long x)
{
	for(int i=62;i>=0;i--)
	{
		if(x&(1ll<<i))
		{
			if(!p[i])
			{
				p[i]=x;
				break;
			}
			else
				x^=p[i];
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
