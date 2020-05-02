---
title: 题解 U19727 【PP游戏#1 彩环游戏（Color Rings Game）】
date: 2018-10-17 20:42:08	
tags: 
  - 题解
---
[LuoguU19727](https://www.luogu.org/problemnew/show/U19727):

按题意模拟暴力即可。
```cpp
#include<bits/stdc++.h>
using namespace std;
int n,m;
int a[5][5][5];//x,y,sz;
int h[15][5];
int st[15][2];
void check()
{
    bool tag,t;
    queue<pair<pair<int,int>,int> >q;
    for(int c=1;c<=n;c++)
    {
        for(int i=1;i<=3;i++)
        {
            tag=1;
            for(int j=1;j<=3;j++)
            {
                t=0;
                for(int k=1;k<=3;k++)
                {
                    if(a[i][j][k]==c)
                    {
                        t=1;
                        break;
                    }
                }
                if(!t)
                {
                    tag=0;
                    break;
                }
            }
            if(tag)
            {
                for(int j=1;j<=3;j++)
                    for(int k=1;k<=3;k++)
                        if(a[i][j][k]==c)
                            q.push(make_pair(make_pair(i,j),k));
            }
        }
        for(int j=1;j<=3;j++)
        {
            tag=1;
            for(int i=1;i<=3;i++)
            {
                t=0;
                for(int k=1;k<=3;k++)
                {
                    if(a[i][j][k]==c)
                    {
                        t=1;
                        break;
                    }
                }
                if(!t)
                {
                    tag=0;
                    break;
                }
            }
            if(tag)
            {
                for(int i=1;i<=3;i++)
                    for(int k=1;k<=3;k++)
                        if(a[i][j][k]==c)
                            q.push(make_pair(make_pair(i,j),k));
            }
        }
        tag=1;
        for(int i=1;i<=3;i++)
        {
            t=0;
            for(int k=1;k<=3;k++)
            {
                if(a[i][i][k]==c)
                {
                    t=1;
                    break;
                }
            }
            if(!t)
            {
                tag=0;
                break;
            }
        }
        if(tag)
        {
            for(int i=1;i<=3;i++)
                for(int k=1;k<=3;k++)
                    if(a[i][i][k]==c)
                        q.push(make_pair(make_pair(i,i),k));
        }
        tag=1;
        for(int i=1;i<=3;i++)
        {
            t=0;
            for(int k=1;k<=3;k++)
            {
                if(a[i][4-i][k]==c)
                {
                    t=1;
                    break;
                }
            }
            if(!t)
            {
                tag=0;
                break;
            }
        }
        if(tag)
        {
            for(int i=1;i<=3;i++)
                for(int k=1;k<=3;k++)
                    if(a[i][4-i][k]==c)
                        q.push(make_pair(make_pair(i,4-i),k));
        }
    }
    for(int i=1;i<=3;i++)
    {
        for(int j=1;j<=3;j++)
        {
            if(a[i][j][1]==a[i][j][2]&&a[i][j][2]==a[i][j][3])
            {
                for(int k=1;k<=3;k++)
                    q.push(make_pair(make_pair(i,j),k));
            }
        }
    }
    while(!q.empty())
    {
        int x=q.front().first.first,y=q.front().first.second,c=q.front().second;q.pop();
        a[x][y][c]=0;
    }
}
void dfs(int d)
{
    if(d>m)
    {
        for(int k=1;k<=3;k++)
            for(int i=1;i<=3;i++)
                for(int j=1;j<=3;j++)
                    if(a[i][j][k])
                        return ;
        for(int i=1;i<=m;i++)
            printf("%d %d\n",st[i][0],st[i][1]);
        exit(0);
    }
    int t[5][5][5];memcpy(t,a,sizeof(t));
    for(int i=1;i<=3;i++)
        for(int j=1;j<=3;j++)
        {
            bool tag=1;
            for(int k=1;k<=3;k++)
            {
                if(h[d][k]&&a[i][j][k])
                {
                    tag=0;
                    break;
                }
            }
            if(tag)
            {
                for(int k=1;k<=3;k++)
                {
                    a[i][j][k]=(h[d][k]?h[d][k]:a[i][j][k]);
                }
                st[d][0]=i;st[d][1]=j;
                check();dfs(d+1);
                st[d][0]=0;st[d][1]=0;
                memcpy(a,t,sizeof(a));
            }
        }
}
int main()
{
    scanf("%d",&n);
    for(int k=1;k<=3;k++)
        for(int i=1;i<=3;i++)
            for(int j=1;j<=3;j++)
                scanf("%d",&a[i][j][k]);
    scanf("%d",&m);
    for(int i=1;i<=m;i++)
        for(int j=1;j<=3;j++)
            scanf("%d",&h[i][j]);
    dfs(1);
}
```