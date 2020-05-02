---
title: Manacher
date: 2019-01-14 09:35:26	
tags: 
  - 字符串
---

代码如下：
```cpp
#include<bits/stdc++.h>
using namespace std;
int n,p[51000100],ans=1;
char a[51000100],s[102000200];
void manacher()
{
    int maxright=0,mid=0;
    for(int i=0;i<n;i++)
    {
        if(i<maxright)
            p[i]=min(p[2*mid-i],maxright-i);
        else
            p[i]=1;
        for(;i-p[i]>=0&&s[i+p[i]]==s[i-p[i]];p[i]++);
        if(p[i]+i>maxright)
            maxright=p[i]+i,mid=i;
    }
}
void change()
{
    s[0]=s[1]='#';
    for(int i=0;i<n;i++)
    {
        s[i*2+2]=a[i];
        s[i*2+3]='#';
    }
    n=n*2+2;
    s[n]=0;
}
int main()
{
    scanf("%s",a);
    n=strlen(a);
    change();
    manacher();
    for(int i=0;i<n;i++)
    {
        ans=max(ans,p[i]);
    }
    printf("%d\n",ans-1);
    return 0;
} 
```

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。