---
title: 题解 U22412 【PP游戏#5 种树游戏（Tree Game）】
date: 2018-04-01 09:29:52	
tags: 
  - 题解
---
[LuoguU22412](https://www.luogu.org/problemnew/show/U22412):

这是我一次晚自习无聊翻数学必修五书时看到了一张图，我觉得很好玩就延伸了以下找到了一个规律。

![](https://cdn.luogu.org/upload/pic/16534.png)

首先，对于n==1时的情况输出$2^{n-1}$，因为每个枝干每年都会分成两个枝干，所以说输出$2^{n-1}$。
于是就有以下代码：
```cpp
	if(n==1)
    {
        printf("%d\n",(int)(pow(2,m-1)));
        return 0;
    }
```
我们再看样例，样例给的n=2时，列出的每年的枝干数$a$为

$\begin{matrix}1&1&2&3&5&\cdots \end{matrix}$

所以很明显，这是一个斐波那契数列，满足规律$a_i=\sum_{j=i-2}^{i+1}a_j$或者说是$a_i=a_{i-1}+a_{i-n}$。

据此，我们再画出n=3、n=4的图，得到两组数

$\begin{matrix}1&1&1&2&3&4&6&9&13&19&28&41&60&88&129&\cdots\\1&1&1&1&2&3&4&5&7&10&14&19&26&36&50&\cdots\end{matrix}$

仔细观察这些数，按规律1，n=3时，每隔1位，连取3位加和（例如19=4+6+9）；n=4时，每隔2位，连取4位加和（例如19=3+4+5+7）。

因此我们不难根据规律1得到以下递推式：

$a_i=\sum_{j=i-2\times n+2}^{i-n+1}a_j$

下面进行简单证明：每隔枝干长n年后会和之前n-2前的n的情况一模一样，在这n年间，情况相同，所以连取n个加和。

写成程序后代码如下：
```cpp
#include<bits/stdc++.h>
using namespace std;
const int maxm=1e6+50;
int n,m,p;
int a[maxm];
int main()
{
    scanf("%d%d%d",&n,&m,&p);
    if(n==1)
    {
        printf("%d\n",(int)(pow(2,m-1)));
        return 0;
    }//n==1
    for(int i=1;i<=n;i++)
    {
        a[i]=1;
    }
    if(m<=n)
    {
        printf("%d\n",a[m]);
        return 0;
    }//m<=n时只有一个主干
    for(int i=n+1;i<=2*n;i++)
    {
        a[i]=(i-n+1)%p;
    }//这里如果采用递推式下标会越界，所以要手动赋值，庆幸的是这里的数很有规律很好写。
    if(m<=2*n)
    {
        printf("%d\n",a[m]);
        return 0;
    }
    for(int i=2*n+1;i<=m;i++)
    {
        for(int j=i-2*n+2;j<=i-n+1;j++)
        {
            a[i]+=a[j];
            a[i]%=p;
        }
    }//递推式写法
    printf("%d\n",a[m]);
    return 0;
}
```
显然，这种规律算法的时间复杂度为$\mathit{O(nm)}$，如果我们还想更优，我们就需要利用规律2，n=3时，取前第1位和前第3位加和（例如19=6+13）；n=4时，取前第1位和前第4位加和（例如19=5+14）。

因此我们不难根据规律2得到以下递推式：

$a_i=a_{i-1}+a_{i-n}$

下面进行简单证明：上一年的枝干长一年后会相当于长出n年前的一个树，故为此。
- 我们也可以利用之前的那个公式用$a_i-a_{i-1}$即可得到这个公式

写成程序后代码如下：
```cpp
#include<bits/stdc++.h>
using namespace std;
const int maxm=1e6+50;
int n,m,p;
int a[maxm];
int main()
{
    scanf("%d%d%d",&n,&m,&p);
    if(n==1)
    {
        printf("%d\n",(int)(pow(2,m-1)));
        return 0;
    }//n==1
    for(int i=1;i<=n;i++)
    {
        a[i]=1;
    }
    if(m<=n)
    {
        printf("%d\n",a[m]);
        return 0;
    }//m<=n时只有一个主干
    for(int i=n+1;i<=2*n;i++)
    {
        a[i]=(i-n+1)%p;
    }//这里其实可以采用加和式，但是因为是前代码改的所以懒得再写了。
    if(m<=2*n)
    {
        printf("%d\n",a[m]);
        return 0;
    }
    for(int i=2*n+1;i<=m;i++)
    {
        a[i]=a[i-1]+a[i-n];
        a[i]%=p;
    }//加和式写法
    printf("%d\n",a[m]);
    return 0;
}
```
恩，对规律找规律，有趣的一道规律题，数学也是很好玩的嘛O(∩_∩)O~