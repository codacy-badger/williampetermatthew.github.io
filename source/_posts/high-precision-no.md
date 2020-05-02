---
title: 高精度无符号整数算法
date: 2018-09-29 21:20:20
tags: 
  - 科技
---
# 高精度的写法
我们为了方便后来的操作，可以先使用一个struct封装内容物。
我们可以在声明一个struct的时候自动声明一个vector作为整体类型。
```cpp
struct Wint:vector<int>
{
};
```
我们可以写一个构造函数
```cpp
	Wint(int n=0)
    {
    	push_back(n);
    }
```
对于这个构造函数，如果我们不传n，那么默认n为0。
同时，我们为了方便写高精度之间的操作，我们可以先写一个check函数以帮助我们写下面的函数。我们检查下是否在非空的情况下最后一位为0，如果是我们就弹掉（如果是最后一位，额，我们在输出的时候会解决这个问题，可以提前看下怎么处理）。我们发现一位大于9，我们就进位，如果最后一位（即最高位）还大于9，我们就新建最后一位。
```cpp
    Wint& check()
    {
        while(!empty()&&!back())pop_back();
        if(empty())return *this;
        for(int i=1; i<size(); ++i)
        {
            (*this)[i]+=(*this)[i-1]/10;
            (*this)[i-1]%=10;
        }
        while(back()>=10)
        {
            push_back(back()/10);
            (*this)[size()-2]%=10;
        }
        return *this;
    }
```
那么整体的struct就长这样
```cpp
struct Wint:vector<int>
{
    Wint(int n=0)
    {
        push_back(n);
        check();
    }
    Wint& check()
    {
        while(!empty()&&!back())pop_back();
        if(empty())return *this;
        for(int i=1; i<size(); ++i)
        {
            (*this)[i]+=(*this)[i-1]/10;
            (*this)[i-1]%=10;
        }
        while(back()>=10)
        {
            push_back(back()/10);
            (*this)[size()-2]%=10;
        }
        return *this;
    }
};
```
***所有的运算符重载可以参见我的 重载运算符和重载函数 博客***
## 输入输出运算符
我们不要考虑scanf这种读入~~（因为这样的实现很复杂我不会）~~，既然高精度的时间复杂度很大~~（也有可能是我比较蒟蒻）~~，我们就可以考虑流式输入输出
```cpp
istream& operator>>(istream &is,Wint &n)
{
    string s;
    is>>s;
    n.clear();
    for(int i=s.size()-1;i>=0;--i)n.push_back(s[i]-'0');
    return is;
}
ostream& operator<<(ostream &os,const Wint &n)
{
    if(n.empty())os<<0;
    for(int i=n.size()-1;i>=0;--i)os<<n[i];
    return os;
}
```
至于流式输入输出的运算符写法，可以参考我的博客重载运算符。

对于输入流，我们先读入一串字符，然后从后向前加数（即高位在后，低位在前，这样方便我们进行加减等运算）。

对于输出流，我们如果发现为空（一般是只定义没有输入或计算，常用来卡高精）就输出0；然后从后往前输出就是高位到低位输出。

## 关系运算符
我们不要考虑太多类型与高精度相比较，我们可以先考虑高精度间的逻辑~~，如果我们要比较一个高精度和另一个整数，可以采用stringstream传（此条因太过毒瘤笔者被Diss到爆而去掉，感兴趣的读者可以自己了解下stringstream）~~。
```cpp
bool operator!=(const Wint &a,const Wint &b)
{
    if(a.size()!=b.size())return 1;
    for(int i=a.size()-1; i>=0; --i)
        if(a[i]!=b[i])return 1;
    return 0;
}
bool operator==(const Wint &a,const Wint &b)
{
    return !(a!=b);
}
bool operator<(const Wint &a,const Wint &b)
{
    if(a.size()!=b.size())return a.size()<b.size();
    for(int i=a.size()-1; i>=0; --i)
        if(a[i]!=b[i])return a[i]<b[i];
    return 0;
}
bool operator>(const Wint &a,const Wint &b)
{
    return b<a;
}
bool operator<=(const Wint &a,const Wint &b)
{
    return !(a>b);
}
bool operator>=(const Wint &a,const Wint &b)
{
    return !(a<b);
}
```
对于不等于，我们可以在判断完位数后挨个比较位数；对于等于，我们可以返回非不等于。

对于小于，我们可以比较位数，然后从高位向低位比较大小；对于大于，我们可以返回反比较的小于；对于小于等于，我们可以返回非大于；对于大于等于，我们可以返回非小于。
## ~~逻辑运算符~~
~~此内容已因笔者过于蒟蒻或毒瘤而被Diss到爆炸，现已划线。~~
```cpp
//bool operator!(Wint &n)
//{
//	n.check();
//	return n==0;
//}
//bool operator&&(Wint &a,Wint &b)
//{
//	a.check();b.check();
//	if(!a)return 0;
//	if(!b)return 0;
//	return 1;
//}
//bool operator||(Wint &a,Wint &b)
//{
//	a.check();b.check();
//	if(a>0)return 1;
//	if(b>0)return 1;
//	return 0;
//}
```
~~十分的简单对不对。。。~~
## 双目算术、赋值和自增自减运算符
```cpp
Wint& operator+=(Wint &a,const Wint &b)
{
    if(a.size()<b.size())a.resize(b.size());
    for(int i=0;i!=b.size();++i)a[i]+=b[i];
    return a.check();
}
Wint operator+(Wint a,const Wint &b)
{
    return a+=b;
}
Wint& operator++(Wint &n)
{
	n+=1;
	return n.check();
}
Wint operator++(Wint &n,int flag)
{
	Wint tmp=n;
	n+=1;
	n.check();
	return tmp;
}
```
对于加法，我们先实现+=，如果a的size小，我们就要开空间，然后依次相加最后check下即可。
对于+可以返回a+=b。
```cpp
Wint& operator-=(Wint &a,Wint b)
{
    if(a<b)swap(a,b);
    for(int i=0; i!=b.size(); a[i]-=b[i],++i)
        if(a[i]<b[i])
        {
            int j=i+1;
            while(!a[j])++j;
            while(j>i)
            {
                --a[j];
                a[--j]+=10;
            }
        }
    return a.check();
}
Wint operator-(Wint a,const Wint &b)
{
    return a-=b;
}
Wint& operator--(Wint &n)
{
	n-=1;
	return n.check();
}
Wint operator--(Wint &n,int flag)
{
	Wint tmp=n;
	n-=1;
	n.check();
	return tmp;
}
```
对于减法，我们先实现-=，如果a小我们就要交换a和b（我们写的是高精度无符号整数），然后后依次相加最后check下即可。
对于-可以返回a-=b。
```cpp
Wint operator*(const Wint &a,const Wint &b)
{
    Wint n;
    n.assign(a.size()+b.size()-1,0);
    for(int i=0; i!=a.size(); ++i)
        for(int j=0; j!=b.size(); ++j)
            n[i+j]+=a[i]*b[j];
    return n.check();
}
Wint& operator*=(Wint &a,const Wint &b)
{
    return a=a*b;
}
```
对于乘法，我们需要先实现```*```，在新建的返回高精中，第i+j位加上$a_i * b_j$。然后对于```*=```返回```a=a*b```即可
```cpp
Wint divmod(Wint &a,const Wint &b)
{
    Wint ans;
    for(int t=a.size()-b.size(); a>=b; --t)
    {
        Wint d;
        d.assign(t+1,0);
        d.back()=1;
        Wint c=b*d;
        while(a>=c)
        {
            a-=c;
            ans+=d;
        }
    }
    return ans;
}
Wint operator/(Wint a,const Wint &b)
{
    return divmod(a,b);
}
Wint& operator/=(Wint &a,const Wint &b)
{
    return a=a/b;
}
```
学过Python的读者应该知道在Python中有一个divmod函数（在Python 2.3之前不允许处理复数），这个函数在Python中把除数和余数运算结果结合起来，返回一个包含商和余数的元组(a//b,a%b)。
而在C++中我们写的这个函数就是除法。assign是C++string类的成员函数，用于拷贝、赋值操作，它们允许我们顺次地把一个string对象的部分内容拷贝到另一个string对象上。
然后写/，返回divmod(a,b)；/=返回a=a/b。
```cpp
Wint& operator%=(Wint &a,const Wint &b)
{
    divmod(a,b);
    return a;
}
Wint operator%(Wint a,const Wint &b)
{
    return a%=b;
}
```
## 常用函数
```cpp
Wint pow(const Wint &n,const Wint &k)
{
    if(k.empty())return 1;
    if(k==2)return n*n;
    if(k.back()%2)return n*pow(n,k-1);
    return pow(pow(n,k/2),2);
}
//下面是非递归写法
Wint pow(Wint n,Wint k)
{
	Wint ans(1);
	while(k>0)
	{
		if(k%2>0)
			ans*=n;
		n*=n;
		k/=2;
	}
	return ans;
}
```
其实快速幂实现的思想非常好想，所以就这么写了出来。
## 回顾
那么整体的写法如下
```cpp
struct Wint:vector<int>
{
    Wint(int n=0)
    {
        push_back(n);
        check();
    }
    Wint& check()
    {
        while(!empty()&&!back())pop_back();
        if(empty())return *this;
        for(int i=1; i<size(); ++i)
        {
            (*this)[i]+=(*this)[i-1]/10;
            (*this)[i-1]%=10;
        }
        while(back()>=10)
        {
            push_back(back()/10);
            (*this)[size()-2]%=10;
        }
        return *this;
    }
};
istream& operator>>(istream &is,Wint &n)
{
    string s;
    is>>s;
    n.clear();
    for(int i=s.size()-1; i>=0; --i)n.push_back(s[i]-'0');
    return is;
}
ostream& operator<<(ostream &os,const Wint &n)
{
    if(n.empty())os<<0;
    for(int i=n.size()-1; i>=0; --i)os<<n[i];
    return os;
}
bool operator!=(const Wint &a,const Wint &b)
{
    if(a.size()!=b.size())return 1;
    for(int i=a.size()-1; i>=0; --i)
        if(a[i]!=b[i])return 1;
    return 0;
}
bool operator==(const Wint &a,const Wint &b)
{
    return !(a!=b);
}
bool operator<(const Wint &a,const Wint &b)
{
    if(a.size()!=b.size())return a.size()<b.size();
    for(int i=a.size()-1; i>=0; --i)
        if(a[i]!=b[i])return a[i]<b[i];
    return 0;
}
bool operator>(const Wint &a,const Wint &b)
{
    return b<a;
}
bool operator<=(const Wint &a,const Wint &b)
{
    return !(a>b);
}
bool operator>=(const Wint &a,const Wint &b)
{
    return !(a<b);
}
Wint& operator+=(Wint &a,const Wint &b)
{
    if(a.size()<b.size())a.resize(b.size());
    for(int i=0; i!=b.size(); ++i)a[i]+=b[i];
    return a.check();
}
Wint operator+(Wint a,const Wint &b)
{
    return a+=b;
}
Wint& operator++(Wint &n)
{
	n+=1;
	return n.check();
}
Wint operator++(Wint &n,int flag)
{
	Wint tmp=n;
	n+=1;
	n.check();
	return tmp;
}
Wint& operator-=(Wint &a,Wint b)
{
    if(a<b)swap(a,b);
    for(int i=0; i!=b.size(); a[i]-=b[i],++i)
        if(a[i]<b[i])
        {
            int j=i+1;
            while(!a[j])++j;
            while(j>i)
            {
                --a[j];
                a[--j]+=10;
            }
        }
    return a.check();
}
Wint operator-(Wint a,const Wint &b)
{
    return a-=b;
}
Wint& operator--(Wint &n)
{
	n-=1;
	return n.check();
}
Wint operator--(Wint &n,int flag)
{
	Wint tmp=n;
	n-=1;
	n.check();
	return tmp;
}
Wint operator*(const Wint &a,const Wint &b)
{
    Wint n;
    n.assign(a.size()+b.size()-1,0);
    for(int i=0; i!=a.size(); ++i)
        for(int j=0; j!=b.size(); ++j)
            n[i+j]+=a[i]*b[j];
    return n.check();
}
Wint& operator*=(Wint &a,const Wint &b)
{
    return a=a*b;
}
Wint divmod(Wint &a,const Wint &b)
{
    Wint ans;
    for(int t=a.size()-b.size(); a>=b; --t)
    {
        Wint d;
        d.assign(t+1,0);
        d.back()=1;
        Wint c=b*d;
        while(a>=c)
        {
            a-=c;
            ans+=d;
        }
    }
    return ans;
}
Wint operator/(Wint a,const Wint &b)
{
    return divmod(a,b);
}
Wint& operator/=(Wint &a,const Wint &b)
{
    return a=a/b;
}
Wint& operator%=(Wint &a,const Wint &b)
{
    divmod(a,b);
    return a;
}
Wint operator%(Wint a,const Wint &b)
{
    return a%=b;
}
Wint pow(const Wint &n,const Wint &k)
{
    if(k.empty())return 1;
    if(k==2)return n*n;
    if(k.back()%2)return n*pow(n,k-1);
    return pow(pow(n,k/2),2);
}
////下面是非递归写法
//Wint pow(Wint n,Wint k)
//{
//	Wint ans(1);
//	while(k>0)
//	{
//		if(k%2>0)
//			ans*=n;
//		n*=n;
//		k/=2;
//	}
//	return ans;
//}
```
# 高精度的用法
我们在之前的定义写法中写的方式可以方便的帮助我们写。
例如洛谷高精度算法试炼场中的几道题
我们在main函数中这么写
```cpp
int main()
{
    Wint a,b;
    //可以把b改成int型，仍能正常使用（题切不了）
    cin>>a>>b;
    //LuoguP1601 cout<<a+b<<endl;
    //LuoguP2142 if(a-b<0)cout<<"-";cout<<a-b<<endl;
    //LuoguP1303 cout<<a*b<<endl;
    return 0;
}
//LuoguP1255
int main()
{
	int n;
	Wint f[3];
    cin>>n;
    if(n==0)
    {
        printf("0\n");
        return 0;
    }
    f[0]=(Wint)1;
    f[1]=(Wint)1;
    for(int i=2;i<=n;i++)
    {
        f[i%3]=f[(i-1)%3]+f[(i-2)%3];
    }
    cout<<f[n%3]<<endl;
}
//LuoguP1604
//我们先定义一个进制，然后修改check和输入输出即可
int jz;
struct Wint:vector<int>
{
    ...
    Wint& check()
    {
        while(!empty()&&!back())pop_back();
        if(empty())return *this;
        for(int i=1; i<size(); ++i)
        {
            (*this)[i]+=(*this)[i-1]/jz;
            (*this)[i-1]%=jz;
        }
        while(back()>=jz)
        {
            push_back(back()/jz);
            (*this)[size()-2]%=jz;
        }
        return *this;
    }
};
istream& operator>>(istream &is,Wint &n)
{
    string s;
    is>>s;
    n.clear();
    for(int i=s.size()-1; i>=0; --i)
    {
    	if(s[i]>='A')n.push_back(s[i]-'A'+10);
    	else n.push_back(s[i]-'0');
    }
    return is;
}
ostream& operator<<(ostream &os,const Wint &n)
{
    if(n.empty())os<<0;
    for(int i=n.size()-1; i>=0; --i)
    {
    	char tmp;
    	if(n[i]>9)tmp='A'+n[i]-10;
        else tmp=n[i]+'0';
        os<<tmp;
    }
    return os;
}
//最后main函数里
int main()
{
    Wint a,b;
    cin>>jz;
    cin>>a>>b;
    cout<<a+b<<endl;
    return 0;
}
```

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。