---
title: NOIp2011-2017总结
date: 2018-10-16 16:21:26
tags: 
  - 不知道什么东西
---
# NOIp2011
Link to Francis_noco（孙启皓）
# NOIp2012
## D1T1[LuoguP1079](https://www.luogu.org/problemnew/show/P1079):Vigenère 密码
### 题目大意
给你一串加密后的字符串和一串加密钥匙，给你加密函数（以表的方式给出），求原字符串。
### 题目解析
这是一道很水的模拟题，我们可以建一张表大小52$\times$52（注意字母的大小写只与原串与加密串有关）暴力模拟。但是作为一个~~研究字符串很久的~~蒟蒻，我们利用这张表的性质（假设A-Z表示1-26，那么表上的第i行第j列的值为(i+j-2)%26+1），反推加密串与加密钥匙的表的性质（假设A-Z表示1-26，那么表上的第行第j列的值为(j-i+26)%26+1）。
### 题目代码
```cpp
#include<bits/stdc++.h>
using namespace std;
char m[1005],k[105],c[1005];
int dxx(char x)
{
	if(x>='a'&&x<='z')return 'a'-1;
	if(x>='A'&&x<='Z')return 'A'-1;
}
char R(int m_i,int k_i)
{
	return (m[m_i]-dxx(m[m_i])+k[k_i%strlen(k)]-dxx(k[k_i%strlen(k)])-2)%26+1+dxx(m[m_i]);
}
char R_1(int c_i,int k_i)
{
	return (c[c_i]-dxx(c[c_i])-k[k_i%strlen(k)]+dxx(k[k_i%strlen(k)])+26)%26+1+dxx(c[c_i]);
}
int main()
{
	scanf("%s",k);
	scanf("%s",c);
	for(int i=0;i<strlen(c);i++)
		printf("%c",R_1(i,i));
}
```
## D1T2[LuoguP1080](https://www.luogu.org/problemnew/show/P1080):国王游戏
### 题目大意
有1个国王n个大臣，每人左右手里都有数，每个大臣拿到的值是他前面人的左手值的积与他右手值的商，你需要将大臣排序然后把国王放在大臣前面，然后求大臣的值的最大值最小是多少。
### 题目解析
本题的唯一难点是排序大臣，如果i和j两个大臣相邻那么i排在j前面的必要条件是$total\times a[i]/b[j]<total\times a[j]/b[i]$，也就是说$a[i]\times b[i]<a[j]\times b[j]$。
其次就是注意高精度不要写流式输入输出，否则会TLE。
### 题目代码
```cpp
#include<bits/stdc++.h>
using namespace std;
int now[20005],sum[20005],ans[20005],add[20005];
struct ren
{
    long long a,b,c;
}r[1010];
char gc()
{
    static char buf[100000],*p1=buf,*p2=buf;
    return p1==p2&&(p2=(p1=buf)+fread(buf,1,100000,stdin),p1==p2)?EOF:*p1++;
}
template<typename __Type_of__scan>
void _scan(__Type_of__scan &x)
{
    __Type_of__scan f=1;x=0;char s=gc();
    while(s<'0'||s>'9'){if(s=='-')f=-1;s=gc();}
    while(s>='0'&&s<='9'){x=x*10+s-'0';s=gc();}
    x*=f;
}
char buf[100000],*pp=buf;
void pc(const char c)
{
    if(pp-buf==100000)fwrite(buf,1,100000,stdout),pp=buf;
    *pp++=c;
}
template<typename __Type_of__print>
void _print(__Type_of__print x)
{
    if(x<0){pc('-');x=-x;}
    if(x>9)_print(x/10);
    pc(x%10+'0');
}
void fsh(){fwrite(buf,1,pp-buf,stdout);pp=buf;}
void times(int x)
{
    memset(add,0,sizeof(add));
    for(int i=1;i<=ans[0];i++)
	{
        ans[i]=ans[i]*x;
        add[i+1]+=ans[i]/10;
        ans[i]%=10;
    }
    for(int i=1;i<=ans[0]+4;i++)
	{
        ans[i]+=add[i];
        if(ans[i]>=10)
		{
            ans[i+1]+=ans[i]/10;
            ans[i]%=10;
        }
        if(ans[i]!=0)
		{
            ans[0]=max(ans[0],i);
        } 
    }
    return ;
}
int divition(int x)
{
    memset(add,0,sizeof(add));
    int q=0;
    for(int i=ans[0];i>=1;i--)
	{
        q*=10;
        q+=ans[i];
        add[i]=q/x;
        if(add[0]==0 && add[i]!=0)
		{
            add[0]=i;
        }
        q%=x; 
    }
    return 0;
}
bool compare()
{
    if(sum[0]!=add[0])
    	return add[0]>sum[0];
    for(int i=add[0];i>=1;i--)
		if(add[i]!=sum[i])
			return add[i]>sum[i];
}
void cp()
{
    memset(sum,0,sizeof(sum));
    for(int i=add[0];i>=0;i--)
        sum[i]=add[i];
    return ;
}
bool cmp(ren a,ren b)
{
    return a.c<b.c;
}
int n;
int main()
{
    _scan(n);
    for(int i=0;i<=n;i++)
	{
        _scan(r[i].a),_scan(r[i].b);
        r[i].c=r[i].a*r[i].b;
    }
    sort(r+1,r+n+1,cmp);
    ans[0]=1,ans[1]=1;
    for(int i=1;i<=n;i++)
	{
        times(r[i-1].a);
        divition(r[i].b);
        if(compare())
            cp();
    }
    for(int i=sum[0];i>=1;i--)
        _print(sum[i]);
	fsh();
    return 0;
} 
```
## D1T3[LuoguP1081](https://www.luogu.org/problemnew/show/P1081):开车旅行
### 题目大意
小A和小B决定利用假期外出旅行，他们将想去的城市从1到N编号，且编号较小的城市在编号较大的城市的西边，已知各个城市的海拔高度互不相同，记城市i的海拔高度为Hi，城市i和城市j之间的距离d[i,j]恰好是这两个城市海拔高度之差的绝对值，即d[i,j]=|Hi-Hj|。

旅行过程中，小A和小B轮流开车，第一天小A开车，之后每天轮换一次。他们计划选择一个城市S作为起点，一直向东行驶，并且最多行驶X公里就结束旅行。小A和小B的驾驶风格不同，小B总是沿着前进方向选择一个最近的城市作为目的地，而小A总是沿着前进方向选择第二近的城市作为目的地（注意：本题中如果当前城市到两个城市的距离相同，则认为离海拔低的那个城市更近）。如果其中任何一人无法按照自己的原则选择目的城市，或者到达目的地会使行驶的总距离超出X公里，他们就会结束旅行。

在启程之前，小A想知道两个问题：
1. 对于一个给定的X=X0，从哪一个城市出发，小A开车行驶的路程总数与小B行驶的路程总数的比值最小（如果小B的行驶路程为0，此时的比值可视为无穷大，且两个无穷大视为相等）。如果从多个城市出发，小A开车行驶的路程总数与小B行驶的路程总数的比值都最小，则输出海拔最高的那个城市。
2. 对任意给定的X=Xi和出发城市Si，小A开车行驶的路程总数以及小B行驶的路程总数。

### 题目解析
这题建议先把暴力模拟的70分做出来而不是直接打正解，考场上如果想不到用倍增，暴力拿70分也是很可观的

那么先说说暴力思路:
1)预处理

显然我们可以做下面这些事情:

c1[i]表示b在i选择的下一个城市（也就是在i东边距离i最近的城市）  
c2[i]表示a在i选择的下一个城市（也就是在i东边距离i第二近的城市）  
dist1[i]表示b从i到c1[i]的路程  
dist2[i]表示a从i到c2[i]的路程  

我们可以从第n-1个城市枚举到第1个城市，在第i+1个城市到第n个城市之间

2)对于两个问题，分别模拟求解

由于a b是轮流驾驶的，我们可以用一个变量turn记录当前是谁在驾驶，turn为0时a驾驶，为1时b驾驶，然后根据题意记录a b分别走过的路程，判断总路程是否超过限制，模拟即可

得分：70暴力找最近和第二近，具体实现看代码

暴力打完了，接下来就是正解了

其实如果直接告诉你正解就是在暴力的基础上用倍增和双向链表优化一下，自己想想，慢慢调也能弄出来，所以还是建议先自己思考一下如何用倍增去优化，然后想想如何用双向链表去初始化

1)这里先说一下为什么用倍增

显然，对于每一个点i，a的选择是唯一的，b的选择也是唯一的，所以不存在最优解，可以用倍增

2)怎么使用双向链表初始化

首先把所有城市的高度和编号存入一个结构体，然后排序，记录一下每个城市排序后的位置。然后这个结构体数组可以直接加一个last域和next域改成双向链表，预处理的时候从1到n在链表上找到对应位置pos[i]，不难想到第一近和第二近一定在pos[i]-2 pos[i]-1 pos[i]+1 pos[i]+2之间，在这四个位置之间和上面的暴力是一样的处理，然后在链表中把pos[i]删去，这样的话由于1到n是从西到东的，链表中除了pos[i]以外的所有城市都在pos[i]东边，那么就可以O(N)预处理出c1 c2 dist1 dist2

3)倍增初始化

下面说说如何初始化倍增以及注意事项

我们定义dist3[i][j]为a和b从i分别开了1<<j次的距离，
dist4[i][j]为a和b从i分别开了1<<j次以后a的总路程，dist5同理，为a和b从i分别开了1<<j次以后b的总路程
c3[i][j]为a和b从i分别开了1<<j次以后他们在什么位置

对于j=0的情况是显然的

注意一下c1和c2不要弄混了

然后是j>=1的情况

这里需要特别注意的是，j是在外层的，i是在内层的，千万不能搞反了（可能只有我这种蒟蒻会犯这种错），因为要先处理j-1的情形才能处理j，另外，如果c3[i][j]是0，说明不能走了，就不要去处理dist了

然后就初始化完了

4)对问题求解

我们先让a和b一起开，那么用c3和dist3就能得到一个近似最终的情况，最后由于是a先开的，要判断一下a是否可以继续开，因为之前是a开完b也开，可能再开dist3就超过限制而a再开一次并没有超过限制，所以这个判断是必要的，其余的与暴力相似

得分：100
### 题目代码
70分
```cpp
#include <bits/stdc++.h>
using namespace std;
long long c1[100005], c2[100005], n, m, h[100005] = { INT_MAX }, dist1[100005], dist2[100005];
template<typename __Type_of_scan>
void scan(__Type_of_scan &x)
{
    __Type_of_scan f=1;x=0;char s=getchar();
    while(s<'0'||s>'9'){if(s=='-')f=-1;s=getchar();}
    while(s>='0'&&s<='9'){x=x*10+s-'0';s=getchar();}
    x*=f;
}
template<typename __Type_of_print>
void print(__Type_of_print x)
{
    if(x<0){putchar('-');x=-x;}
    if(x>9)print(x/10);
    putchar(x%10+'0');
}
double minv=INT_MAX;
int main()
{
    scan(n);
    for(int i=1;i<=n;i++)
		scan(h[i]);
    for(int i=n-1;i>=1;i--)
    {
        long long minn=i+1,minn2=0;
        dist1[i]=abs(h[i]-h[i+1]);
        for(int j=i+2;j<=n;j++)
        {
            if(dist1[i]>abs(h[i]-h[j])||(dist1[i]==abs(h[i]-h[j])&&h[j]<h[minn]))
            {
                dist2[i]=dist1[i];
                dist1[i]=abs(h[i]-h[j]);
                minn2=minn;
                minn=j;
            }
            else if(dist2[i]==0||dist2[i]>abs(h[i]-h[j])||(dist2[i]==abs(h[i]-h[j])&&h[j]<h[minn2]))
            {
                dist2[i]=abs(h[i]-h[j]);
                minn2=j;
            }
        }
        c1[i]=minn;
        c2[i]=minn2;
    }
    long long x0,ans=0;
    scan(x0);
    for(int i=1;i<=n;i++)
    {
        long long a=0,b=0,loc=i,turn=0;
        while(1)
        {
            if(turn)
            {
                if(a+b+dist1[loc]>x0||!c1[loc])break;
                b+=dist1[loc];
                loc=c1[loc];
            }
            else
            {
                if(a+b+dist2[loc]>x0||!c2[loc])break;
                a+=dist2[loc];
                loc=c2[loc];
            }
            turn^=1;
        }
        if(!ans||1.0*a/b-minv<-0.00000001||(fabs(1.0*a/b-minv)<=0.00000001&&h[ans]<h[i]))
        {
            minv=1.0*a/b;
            ans=i;
        }
    }
    printf("%lld\n",ans);
    scan(m);
    while(m--)
    {
        long long s,x,a=0,b=0,turn=0;
        scan(s),scan(x);
        while(1)
        {
            if(turn)
            {
                if(a+b+dist1[s]>x||!c1[s])break;
                b+=dist1[s];
                s=c1[s];
            }
            else
            {
                if(a+b+dist2[s]>x||!c2[s])break;
                a+=dist2[s];
                s=c2[s];
            }
            turn^=1;
        }
        printf("%lld %lld\n",a,b);
    }
    return 0;
}
```
100分
```cpp
#include<bits/stdc++.h>
using namespace std;
long long c1[100005],c2[100005],c3[100005][21],n,m,pos[100005],dist1[100005],dist2[100005],dist3[100005][21],dist4[100005][21],dist5[100005][21];
template<typename __Type_of_scan>
void scan(__Type_of_scan &x)
{
    __Type_of_scan f=1;x=0;char s=getchar();
    while(s<'0'||s>'9'){if(s=='-')f=-1;s=getchar();}
    while(s>='0'&&s<='9'){x=x*10+s-'0';s=getchar();}
    x*=f;
}
template<typename __Type_of_print>
void print(__Type_of_print x)
{
    if(x<0){putchar('-');x=-x;}
    if(x>9)print(x/10);
    putchar(x%10+'0');
}
double minv=INT_MAX;
struct chengshi
{
    long long h;
    int bh,last,next;
}q[100005];
bool cmp(chengshi s,chengshi t){return s.h<t.h;}
void updata(int i,int loc,int x)
{
    if(x>=1&&x<=n)
    {
        if (!dist1[i]||dist1[i]>abs(q[loc].h-q[x].h)||(dist1[i]==abs(q[loc].h-q[x].h)&&q[x].h<q[pos[c1[i]]].h))
        {
            dist2[i]=dist1[i];
            dist1[i]=abs(q[loc].h-q[x].h);
            c2[i]=c1[i];
            c1[i]=q[x].bh;
        }
        else if(!dist2[i]||dist2[i]>abs(q[loc].h-q[x].h)||(dist2[i]==abs(q[loc].h-q[x].h)&&q[x].h<q[pos[c2[i]]].h))
        {
            dist2[i]=abs(q[loc].h-q[x].h);
            c2[i]=q[x].bh;
        }
    }
}
int main()
{
    scan(n);
    for(int i=1;i<=n;i++)scan(q[i].h),q[i].bh=i;
    sort(q+1,q+n+1,cmp);
    for(int i=1;i<=n;i++)
    {
        pos[q[i].bh]=i;
        if(i!=1)
            q[i].last=i-1;
        if(i!=n)
            q[i].next=i+1;
    }
    for(int i=1,loc;i<=n;i++)
    {
        loc=pos[i];
        updata(i,loc,q[q[loc].last].last);
        updata(i,loc,q[loc].last);
        updata(i,loc,q[loc].next);
        updata(i,loc,q[q[loc].next].next);
        if(q[loc].last)q[q[loc].last].next=q[loc].next;
        if(q[loc].next)q[q[loc].next].last=q[loc].last;
        q[loc].last=q[loc].next=0;
    }
    for(int i=1;i<=n;i++)
    {
        dist4[i][0]=dist2[i];
        dist5[i][0]=dist1[c2[i]];
        dist3[i][0]=dist2[i]+dist1[c2[i]];
        c3[i][0]=c1[c2[i]];
    }
    for(int j=1;j<=20;j++)
        for(int i=1;i<=n;i++)
        {
            c3[i][j]=c3[c3[i][j-1]][j-1];
            if(c3[i][j])
            {
                dist3[i][j]=dist3[i][j-1]+dist3[c3[i][j-1]][j-1];
                dist4[i][j]=dist4[i][j-1]+dist4[c3[i][j-1]][j-1];
                dist5[i][j]=dist5[i][j-1]+dist5[c3[i][j-1]][j-1];
            }
        }
    long long xx,ans=0;
    scan(xx);
    for(int i=1;i<=n;i++)
    {
        long long a=0,b=0,loc=i,x0=xx;
        for(int j=20;j>=0;j--)
        {
            if(dist3[loc][j]&&x0>=dist3[loc][j])
            {
                x0-=dist3[loc][j];
                a+=dist4[loc][j];
                b+=dist5[loc][j];
                loc=c3[loc][j];
            }
        }
        if(dist2[loc]<=x0)a+=dist2[loc];
        if(a<=0)continue;
        if(!ans||1.0*a/b-minv<-0.00000001||(fabs(1.0*a/b-minv)<=0.00000001&&q[pos[ans]].h<q[pos[i]].h))
        {
            minv=1.0*a/b;
            ans=i;
        }
    }
    printf("%lld\n",ans);
    scan(m);
    while(m--)
    {
        long long s,x,a=0,b=0;
        scan(x),scan(x);
        for(int j=20;j>=0;j--)
        {
            if(dist3[s][j]&&x>=dist3[s][j])
            {
                x-=dist3[s][j];
                a+=dist4[s][j];
                b+=dist5[s][j];
                s=c3[s][j];
            }
        }
        if(dist2[s]<=x)a+=dist2[s];
        printf("%lld %lld\n",a,b);
    }
    return 0;
}
```
## D2T1[LuoguP1082](https://www.luogu.org/problemnew/show/P1082):同余方程
### 题目大意
求关于x的同余方程ax≡1(mod b)的最小正整数解。
### 题目解析
裸的数论题。
要注意：
1. 此题要求的就是ax mod b=1的最小正整数解。
2. 题目要求最小正整数解，而我们可能求到负整数。由于负数取模是负数，而且绝对值小于模数所以我们加上模数再取模即可。
3. 选用扩展欧几里得算法，注意写的时候要么x,y传址调用，要么x,y为全局变量，其余会挂掉。

### 题目代码
```cpp
#include<bits/stdc++.h>
using namespace std;
int a,b,x,y,z;
void exgcd(int a,int b)
{
    if(b==0)
    {
        x=1;y=0;
        return ;
    }
    exgcd(b,a%b);
    z=x;x=y;
    y=z-a/b*y;
    return ;
}
int main()
{
    scanf("%d%d",&a,&b);
    exgcd(a,b);
    printf("%d\n",(x+b)%b);
}
```
## D2T2[LuoguP1083](https://www.luogu.org/problemnew/show/P1083):借教室
### 题目大意
在大学期间，经常需要租借教室。大到院系举办活动，小到学习小组自习讨论，都需要向学校申请借教室。教室的大小功能不同，借教室人的身份不同，借教室的手续也不一样。

面对海量租借教室的信息，我们自然希望编程解决这个问题。

我们需要处理接下来n天的借教室信息，其中第i天学校有ri个教室可供租借。共有m份订单，每份订单用三个正整数描述，分别为dj,sj,tj，表示某租借者需要从第sj天到第tj天租借教室（包括第sj天和第tj天），每天需要租借dj个教室。

我们假定，租借者对教室的大小、地点没有要求。即对于每份订单，我们只需要每天提供dj个教室，而它们具体是哪些教室，每天是否是相同的教室则不用考虑。

借教室的原则是先到先得，也就是说我们要按照订单的先后顺序依次为每份订单分配教室。如果在分配的过程中遇到一份订单无法完全满足，则需要停止教室的分配，通知当前申请人修改订单。这里的无法满足指从第sj天到第tj天中有至少一天剩余的教室数量不足dj个。

现在我们需要知道，是否会有订单无法完全满足。如果有，需要通知哪一个申请人修改订单。
### 题目解析
二分订单号很容易想到，但是如何check？

线段树是可以的（但是此时你已显然不用再写二分）

然后我们可以用前缀和，用c[]作为前缀和数组，对于一个d[i],s[i],t[i],则将c[s[i]]+=d[i],c[t[i]+1]-=d[i]，
这是在一个订单的起始天+要借的房间数量，在结束天的下一天减去要借的房间数量。

当我们读入d,s,t，则操作c[s]=c[s]+d;c[t+1]=c[t+1]-d;
那么如果第i天在s和t之间，那么前i天的sum{c[i]}中有c[s],相当于已经记下第i天的订单数量了。如果第i天在t之后，前i天的sum{c[i]}中有c[s]和c[t],因为c[s]+d+c[t+1]-d=c[s]+c[t]，所以这个订单只对s和t中间天数起作用。
### 题目代码
```cpp
#include<bits/stdc++.h>
using namespace std;
int n,m,ans=20020420;
int r[1000005],c[1000005],now;
int d[1000005],s[1000005],t[1000005];
bool check(int x)
{
    int sum=0;
    if(now>x) 
        for(int i=x+1;i<=now;i++)
        {
            c[s[i]]-=d[i];
            c[t[i]+1]+=d[i];
        }
    else
        for(int i=now+1;i<=x;i++)
        {
            c[s[i]]+=d[i];
            c[t[i]+1]-=d[i];
        }
    now=x;
    for(int i=1;i<=n;i++)
    {
        sum+=c[i];
        if(sum>r[i])
        	return 1;
    }
    return 0;
}
int main()
{
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++)
        scanf("%d",&r[i]);
    for(int i=1;i<=m;i++)
        scanf("%d%d%d",&d[i],&s[i],&t[i]);
    int lt=1,rt=m;
    while(lt<=rt)
    {
        int mid=(lt+rt)/2;
        if(check(mid))
        {
            ans=min(ans,mid);
            rt=mid-1;
        }
        else 
            lt=mid+1;
    }
    if(ans!=20020420) 
        printf("-1\n%d\n",ans);
    else
        printf("0\n");
    return 0;
}
```
## D2T3[LuoguP1084](https://www.luogu.org/problemnew/show/P1084):疫情控制
### 题目大意
H国有n个城市，这n个城市用n−1条双向道路相互连通构成一棵树，1号城市是首都，也是树中的根节点。

H国的首都爆发了一种危害性极高的传染病。当局为了控制疫情，不让疫情扩散到边境城市（叶子节点所表示的城市），决定动用军队在一些城市建立检查点，使得从首都到边境城市的每一条路径上都至少有一个检查点，边境城市也可以建立检查点。但特别要注意的是，首都是不能建立检查点的。

现在，在H国的一些城市中已经驻扎有军队，且一个城市可以驻扎多个军队。一支军队可以在有道路连接的城市间移动，并在除首都以外的任意一个城市建立检查点，且只能在一个城市建立检查点。一支军队经过一条道路从一个城市移动到另一个城市所需要的时间等于道路的长度（单位：小时）。

请问最少需要多少个小时才能控制疫情。注意：不同的军队可以同时移动。
### 题目解析
先说输出-1的情况，也就是军队数小于根节点的子节点数时无法完成控制，输出-1

接下来，对于求最小时间就想到了可以二分答案。二分限制的时间t，看是否所有军队是否能在t时间内完成移动。

那么时候涉及到树上移动，为了不超时，我们显然需要倍增来爬树。

剩下的就是怎么判断能否在t时间内完成移动了，这道题主要就是这里烦人

很显然的可以想到，所有军队都要尽量往上爬，因为越往上，能控制的节点就越多。

主要问题在于，要不要跨子树。

那么对于所有能爬到根节点的军队，我们记下他剩下的时间和原先爬上来的子树。如果这个军队所在的子树并没有被控制，并且他剩余时间不够他回到原来的子树，那他就根本不用爬到根节点，让他回去控制原来的子树就可以了。因为如果他不回去，必然需要另一个到达根节点并且剩余时间大于从根节点到他原来的子树距离的军队来代替他控制那颗子树，这样显然是亏的，因为当前军队的剩余时间小于子树到根节点距离，也就是小于能代替他控制子树的军队的剩余时间。让一个剩余时间大于当前军队的军队代替当前军队控制当前子树显然不合理。

上述情况是要回去滴

怎么写呢？把爬到根节点的军队按照剩余时间从小到大排序，然后对于满足上述情况的点，把他原先所在的子树做标记，并把剩余时间赋为-1即可。至于为什么要从小到大排序：当两个点原先的子树为同一棵的情况下，当然让剩余时间小的那个去控制原来那棵子树啦，大的那个后扫到，此时原来的子树已经被小的军队控制了，他就不用回去了。

那么对于仍然还呆在根节点等你安排的军队，把他们按照剩余时间从大到小排序。然后记录一下根节点到所有没有被控制的子树的距离，也从大到小排序。然后，贪心比较，也就是第一个和第一个比，第二个和第二个比……比到一个剩余时间小于当前记录下的距离，也就是过不去了，那么当前的限制时间t偏小，失败。如果全部比完了，都能够走到，那么当前t可以成功控制疫情。
### 题目代码
```cpp
#include<bits/stdc++.h>
using namespace std;
int n,m,cnt;
int d[50005],head[100005];
long long a[50005],f[25][50005],dis[25][50005];
long long tag[50005],rst[50005],q[50005],top[50005];
struct edge
{
	int ne,to,w;
}e[100005];
struct army
{
    int rst,bac;
}arr[50005];
bool cmp(int x,int y)
{
    return x>y;
}
bool cmp1(army x,army y)
{
    return x.rst<y.rst;
}
bool cmp2(army x,army y)
{
    return x.rst>y.rst;
}
void addedge(int x,int y,int z)
{
	e[++cnt]=(edge){head[x],y,z};head[x]=cnt;
}
void dfs(int u,int dep,int t)
{
    d[u]=dep,top[u]=t;
    for(int i=head[u];i;i=e[i].ne)
    {
        int v=e[i].to;
        if(u==1) t=v;
        if(!d[v])
        {
        	dfs(v,dep+1,t);
			f[0][v]=u;
			dis[0][v]=e[i].w;
        }
    }
}
bool can(int x,int fa)
{
    if(tag[x])return 1;
    int res=0;
    for(int i=head[x];i;i=e[i].ne)
    {
        if(e[i].to==fa)continue;
        res++;
        if(!can(e[i].to,x))return 0;
    }
    if(!res)return 0;
    return 1;
}
bool check(long long t)
{
    int num=0,k=0;
    memset(tag,0,sizeof(tag));
    for(int i=1;i<=m;i++)
    { 
        long long x=a[i],tmp=t;
        for(int j=20;j>=0;j--)
        {
            if(!f[j][x])continue;
            if(dis[j][x]>tmp)continue;
            tmp-=dis[j][x];
            if(f[j][x]==1)
            {
                arr[++num].rst=tmp,arr[num].bac=top[x];
                x=f[j][x];
                break;
            }
            x=f[j][x];
        }
        tag[x]=1;
    }
    tag[1]=0;
    if(can(1,1)) return 1;
    sort(arr+1,arr+num+1,cmp1);
    for(int i=1;i<=num;i++)
        if(arr[i].rst<dis[0][arr[i].bac])
            if(!can(arr[i].bac,1)) arr[i].rst=-1,tag[arr[i].bac]=1;
    sort(arr+1,arr+num+1,cmp2);
    for(int i=head[1];i;i=e[i].to)
        if(!can(e[i].to,1))q[++k]=e[i].w;
    sort(q+1,q+k+1,cmp);
    arr[num+1].rst=-0x7fffffff;
    for(int i=1;i<=k;i++)
        if(q[i]>arr[i].rst) return 0;
    return 1;
}
int main()
{
    long long l=0,r=0;
    scanf("%d",&n);
    int tmp=0;
    for(int i=1,x,y,z;i<n;i++)
    {
        scanf("%d%d%d",&x,&y,&z);
        addedge(x,y,z);
        addedge(y,x,z);
        r+=z;
        if(x==1||y==1) tmp++;
    }
    f[0][1]=0,dfs(1,1,0);
    for(int i=1;i<=20;i++)
        for(int j=1;j<=n;j++)
        {
            f[i][j]=f[i-1][f[i-1][j]];
            dis[i][j]=dis[i-1][j]+dis[i-1][f[i-1][j]];
        }
    scanf("%d",&m);
    for(int i=1;i<=m;i++)
        scanf("%d",&a[i]);
    if(tmp>m)
    {
        printf("-1");
        return 0;
    }
    while(l<r)
    {
        long long mid=(l+r)/2;
        if(check(mid)) r=mid;
        else l=mid+1;
    }
    printf("%lld",l);
    return 0;
}
```
# NOIp2013
Link to Curry0420（李明达）
# NOIp2014
Link to czj666（陈子骏）
# NOIp2015
Link to cs18（孙锦洋）
# NOIp2016
Link to Bei_S（王子骏）
# NOIp2017
Link to Steven7（尚元睿）