---
title: crt和excrt
date: 2019-02-13 16:18:16
tags: 
  - 数学
---
# crt
```cpp
int exgcd(int a,int b,int &x,int &y)
{
	if(!b){x=1,y=0;return a;}
	int t=exgcd(b,a%b,y,x);y-=a/b*x;return t;
}
int crt()
{
	int ans=0,lcm=1,x,y;
	for(int i=1;i<=k;i++)
		lcm*=b[i];
	for(int i=1;i<=k;i++)
	{
		int tp=lcm/b[i];
		exgcd(tp,b[i],x,y);
		x=(x%b[i]+b[i])%b[i];
		ans=(ans+tp*x*a[i])%lcm;
	}
	return (ans+lcm)%lcm;
}
```
# excrt
```cpp
int exgcd(int a,int b,int &x,int &y)
{
	if(!b){x=1,y=0;return a;}
	int t=exgcd(b,a%b,y,x);y-=a/b*x;return t;
}
int excrt()
{
    int x,y,k;
    int M=b[1],ans=a[1];
    for(int i=2;i<=n;i++)
    {
        int c=(a[i]-ans%b[i]+b[i])%b[i];
        int gcd=exgcd(M,b[i],x,y),bg=b[i]/gcd;
        if(c%gcd)return -1;
        x=x*c/gcd%bg;
        ans+=x*M;
        M*=bg;
        ans=(ans%M+M)%M;
    }
    return (ans%M+M)%M;
}
```

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。