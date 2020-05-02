---
title: Trie树，字典树
date: 2019-01-14 09:25:29
tags: 
  - 字符串
---

代码如下：  
（Luogu P2580 于是他错误的点名开始了）
```cpp
int son[N][26],n,m,len,tot;
char s[N];
bool tag[N],vis[N];
void insert()
{
    int now=0;
    for(int i=1;i<=len;i++)
    {
        if(!son[now][s[i]-'a'])
            son[now][s[i]-'a']=++tot;
        now=son[now][s[i]-'a'];
    }
    tag[now]=1;
}
int find()
{
    int x=0;
    for(int i=1;i<=len;i++)
    {
        if(!son[now][s[i]-'a'])
            return 0;
        now=son[now][s[i]-'a'];
    }
    if(!tag[now])
        return 0;
    if(vis[now])
        return 1;
    vis[now]=1;
    return 2;
}
```

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。