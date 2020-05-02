---
title: 多项式
date: 2019-02-11 21:22:15
tags: 
  - 数学
---
# FFT
## 递归版
```cpp
#include<bits/stdc++.h>
#ifndef M_PI
#define M_PI		3.14159265358979323846
#endif
using namespace std;
int n,m;
struct fushu
{
	double x,y;
}f[4000005],g[4000005];
fushu operator+(fushu a,fushu b){return (fushu){a.x+b.x,a.y+b.y};}
fushu operator-(fushu a,fushu b){return (fushu){a.x-b.x,a.y-b.y};}
fushu operator*(fushu a,fushu b){return (fushu){a.x*b.x-a.y*b.y,a.x*b.y+a.y*b.x};}
void fft(int lim,fushu *a,int tp)
{
	if(lim==1)return ;
	fushu a1[lim>>1],a2[lim>>1];
	for(int i=0;i<=lim;i+=2)
		a1[i>>1]=a[i],a2[i>>1]=a[i+1];
	fft(lim>>1,a1,tp);
	fft(lim>>1,a2,tp);
	fushu Wn=(fushu){cos(2.0*M_PI/lim),tp*sin(2.0*M_PI/lim)},w=(fushu){1.0};
	for(int i=0;i<(lim>>1);i++,w=w*Wn)
	{
		a[i]=a1[i]+w*a2[i];
		a[i+(lim>>1)]=a1[i]-w*a2[i];
	}
}
int main()
{
	scanf("%d%d",&n,&m);
	for(int i=0;i<=n;i++)
		scanf("%lf",&f[i].x);
	for(int i=0;i<=m;i++)
		scanf("%lf",&g[i].x);
	int lim=1;
	while(lim<=n+m)
		lim<<=1;
	fft(lim,f,1);
	fft(lim,g,1);
	for(int i=0;i<=lim;i++)
		f[i]=f[i]*g[i];
	fft(lim,f,-1);
	for(int i=0;i<=n+m;i++)
		printf("%.0lf ",floor(f[i].x/lim));
}
```
其中可以小优化一下
```cpp
    for(int i=0;i<(lim>>1);i++,w=w*Wn)
    {
        complex t=w*a2[i];
        a[i]=a1[i]+t,
        a[i+(lim>>1)]=a1[i]-t;  
    }
```
## 迭代版
```cpp
#include<bits/stdc++.h>
#ifndef M_PI
#define M_PI		3.14159265358979323846
#endif
using namespace std;
int n,m;
struct fushu
{
	double x,y;
}f[10000005],g[10000005];
fushu operator+(fushu a,fushu b){return (fushu){a.x+b.x,a.y+b.y};}
fushu operator-(fushu a,fushu b){return (fushu){a.x-b.x,a.y-b.y};}
fushu operator*(fushu a,fushu b){return (fushu){a.x*b.x-a.y*b.y,a.x*b.y+a.y*b.x};}
int l,r[10000005];
void fft(int lim,fushu *a,int tp)
{
	for(int i=0;i<lim;i++)
		if(i<r[i])
			swap(a[i],a[r[i]]);
	for(int mid=1;mid<lim;mid<<=1)
	{
		fushu Wn=(fushu){cos(M_PI/mid),tp*sin(M_PI/mid)};
		for(int R=mid<<1,j=0;j<lim;j+=R)
		{
			fushu w=(fushu){1,0};
			for(int k=0;k<mid;k++,w=w*Wn)
			{
				fushu x=a[j+k],y=w*a[j+mid+k];
				a[j+k]=x+y;
				a[j+mid+k]=x-y;
			}
		}
	}
}
int main()
{
	scanf("%d%d",&n,&m);
	for(int i=0;i<=n;i++)
		scanf("%lf",&f[i].x);
	for(int i=0;i<=m;i++)
		scanf("%lf",&g[i].x);
	int lim=1;
	while(lim<=n+m)lim<<=1,l++;
	for(int i=0;i<lim;i++)
		r[i]=(r[i>>1]>>1)|((i&1)<<(l-1));
	fft(lim,f,1);
	fft(lim,g,1);
	for(int i=0;i<=lim;i++)
		f[i]=f[i]*g[i];
	fft(lim,f,-1);
	for(int i=0;i<=n+m;i++)
		printf("%.0lf ",floor(f[i].x/lim+0.5));
}
```
# NTT
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long
const long long P=998244353,G=3,Gi=332748118;
int n,m,l,r[3000005];
long long f[3000005],g[3000005];
long long pow(long long a,long long k)
{
	long long ans=1;
	while(k)
	{
		if(k&1)ans=(ans*a)%P;
		a=(a*a)%P;
		k>>=1;
	}
	return ans%P;
}
void ntt(int lim,long long *a,int tp)
{
	for(int i=0;i<lim;i++)
		if(i<r[i])
			swap(a[i],a[r[i]]);
	for(int mid=1;mid<lim;mid<<=1)
	{
		long long Wn=pow(tp==1?G:Gi,(P-1)/(mid<<1));
		for(int R=mid<<1,j=0;j<lim;j+=R)
		{
			long long w=1;
			for(int k=0;k<mid;k++,w=(w*Wn)%P)
			{
				int x=a[j+k],y=w*a[j+mid+k]%P;
				a[j+k]=(x+y)%P;
				a[j+mid+k]=(x-y+P)%P;
			}
		}
	}
}
signed main()
{
	scanf("%lld%lld",&n,&m);
	for(int i=0;i<=n;i++)
		scanf("%lld",&f[i]),f[i]=(f[i]+P)%P;
	for(int i=0;i<=m;i++)
		scanf("%lld",&g[i]),g[i]=(g[i]+P)%P;
	int lim=1;
	while(lim<=n+m)lim<<=1,l++;
	for(int i=0;i<lim;i++)
		r[i]=(r[i>>1]>>1)|((i&1)<<(l-1));
	ntt(lim,f,1);
	ntt(lim,g,1);
	for(int i=0;i<=lim;i++)
		f[i]=(f[i]*g[i])%P;
	ntt(lim,f,-1);
	long long inv=pow(lim,P-2);
	for(int i=0;i<=n+m;i++)
		printf("%lld ",(f[i]*inv)%P);
}
```