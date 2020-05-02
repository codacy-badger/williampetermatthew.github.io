---
title: 高精度带符号整数算法
date: 2018-10-06 08:14:48
tags: 
  - 科技
---
本博客是 高精度无符号整数算法 的延伸版本，请读者在食用前序文章后食用本篇效果更佳。
# 高精度的写法
同无符号的一样，我们仍采用struct封装，但与之前不同的是，我们新定义一个f表示正负。
```cpp
struct Wint:vector<int>
{
	int f=1;
};
```
然后改下构造函数
```cpp
    Wint(int n=0)
    {
    	if(n<0)n*=f=-1;
        push_back(n);
    }
```
不再过多解释构造函数写法，只是这里要判断收到的n的正负。
然后是check函数。
```cpp
    Wint& check()
    {
        while(!empty()&&!back())pop_back();
        if(empty()){f=1;return *this;}
        if(back()<0)
        {
    		f*=-1;
    		for(int i=0; i<size(); ++i)
    			(*this)[i]*=-1;
        }
        for(int i=1; i<size(); ++i)
        {
        	while((*this)[i-1]<0)
	        {
	            int j=i;
	            while((*this)[j]<=0&&j<size())++j;
	            while(j>=i)
	            {
	                --(*this)[j];
	                (*this)[--j]+=10;
	            }
	        }
            (*this)[i]+=(*this)[i-1]/10;
            (*this)[i-1]%=10;
            while(!empty()&&!back())pop_back();
        	if(empty()){f=1;return *this;}
        }
        while(back()>=10)
        {
            push_back(back()/10);
            (*this)[size()-2]%=10;
        }
        if(back()<0)f*=-1,(*this)[size()-1]*=-1;
        return *this;
    }
```
那么整体的struct就长这样
```cpp
struct Wint:vector<int>
{
	int f=1;
    Wint(int n=0)
    {
    	if(n<0)n*=f=-1;
        push_back(n);
    }
    Wint& check()
    {
        while(!empty()&&!back())pop_back();
        if(empty()){f=1;return *this;}
        if(back()<0)
        {
    		f*=-1;
    		for(int i=0; i<size(); ++i)
    			(*this)[i]*=-1;
        }
        for(int i=1; i<size(); ++i)
        {
        	while((*this)[i-1]<0)
	        {
	            int j=i;
	            while((*this)[j]<=0&&j<size())++j;
	            while(j>=i)
	            {
	                --(*this)[j];
	                (*this)[--j]+=10;
	            }
	        }
            (*this)[i]+=(*this)[i-1]/10;
            (*this)[i-1]%=10;
            while(!empty()&&!back())pop_back();
        	if(empty()){f=1;return *this;}
        }
        while(back()>=10)
        {
            push_back(back()/10);
            (*this)[size()-2]%=10;
        }
        if(back()<0)f*=-1,(*this)[size()-1]*=-1;
        return *this;
    }
};
```
***所有的运算符重载可以参见我的 重载运算符和重载函数 博客***
## 输入输出运算符
```cpp
istream& operator>>(istream &is,Wint &n)
{
    string s;
    is>>s;
    n.clear();
    for(int i=s.size()-1; i>=0; --i)
    {
    	if(s[i]=='-')
    	{
    		n.f=-1;
    		continue;
    	}
    	n.push_back(s[i]-'0');
    }
    return is;
}
ostream& operator<<(ostream &os,const Wint &n)
{
    if(n.empty()){os<<0;return os;}
	if(n.f==-1)os<<"-";
    for(int i=n.size()-1; i>=0; --i)os<<n[i];
    return os;
}
```
直接增加"-"的判断即可。
## 关系运算符
正如之前的，我们只写高精度之间的比较~~，如果我们要比较一个高精度和另一个整数，可以同之前所述采用stringstream传（此条因太过毒瘤笔者被Diss到爆而去掉，感兴趣的读者可以自己了解下stringstream）~~。
```cpp
bool operator!=(const Wint &a,const Wint &b)
{
	if(a.f!=b.f)return 1;
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
	if(a.f!=b.f)return a.f<b.f;
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
加上判断f即可。
## 单目运算符与abs函数
```cpp
Wint operator-(Wint a)
{
	if(~a.f)a.f=-1;
	else a.f=1;
	return a.check();
} 
Wint abs(Wint a)
{
	if(a<0)return -a;
	else return a;
} 
```
对于-运算符，我们改变符号即可。
对于abs函数，我们分段返回即可。
## ~~逻辑运算符~~
~~此内容已因笔者过于蒟蒻或毒瘤而被Diss到爆炸，现已划线。~~
```cpp
//bool operator!(Wint &n)
//{
//	n.check();
//	return n==0;
//}
//bool operator~(Wint &n)
//{
//	n.check();
//	return n!=-1;
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
~~即使加上符号也十分的简单对不对。。。~~
## 双目算术、赋值和自增自减运算符
```cpp
Wint& operator+=(Wint &a,const Wint &b)
{
	Wint c=b;
    if(a.size()<c.size())a.resize(c.size());
    if(a<0&&c>0)
    {
    	if(abs(a)<abs(c))
    	{
    		swap(a,c);
    	}
    }
    else if(a>0&&c<0)
    {
    	if(abs(a)<abs(c))
    	{
    		swap(a,c);
    	}
    }
    for(int i=0; i!=b.size();++i)a[i]+=c[i]*((a.f==c.f)?1:-1);
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
Wint& operator-=(Wint &a,const Wint b)
{
	return a+=-b;
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
有了符号，加减就可以一起处理了。
```cpp
Wint operator*(const Wint &a,const Wint &b)
{
    Wint n;
    if(a.f!=b.f)n.f=-1;
    else n.f=1;
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
    Wint ans,tmp=b;
    int af=a.f,tmpf=tmp.f;
    a.f=tmp.f=1;
    for(int t=a.size()-tmp.size(); a>=tmp; --t)
    {
        Wint d;
        d.assign(t+1,0);
        d.back()=1;
        Wint c=tmp*d;
        while(a>=c)
        {
            a-=c;
            ans+=d;
        }
    }
    if(af!=tmpf)ans.f=-1;
    else ans.f=1;
    a.f=af,tmp.f=tmpf;
    return ans.check();
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
```
乘除只用判断同或异号。
这里的取模运算，对于正数，取正或负模均为正数；对于负数，取正或负模均为负数。
## 常用函数
abs函数已经在上文提过。
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
这是快速幂函数。
## 回顾
那么整体的写法如下
```cpp
struct Wint:vector<int>
{
	int f=1;
    Wint(int n=0)
    {
    	if(n<0)n*=f=-1;
        push_back(n);
    }
    Wint& check()
    {
        while(!empty()&&!back())pop_back();
        if(empty()){f=1;return *this;}
        if(back()<0)
        {
    		f*=-1;
    		for(int i=0; i<size(); ++i)
    			(*this)[i]*=-1;
        }
        for(int i=1; i<size(); ++i)
        {
        	while((*this)[i-1]<0)
	        {
	            int j=i;
	            while((*this)[j]<=0&&j<size())++j;
	            while(j>=i)
	            {
	                --(*this)[j];
	                (*this)[--j]+=10;
	            }
	        }
            (*this)[i]+=(*this)[i-1]/10;
            (*this)[i-1]%=10;
            while(!empty()&&!back())pop_back();
        	if(empty()){f=1;return *this;}
        }
        while(back()>=10)
        {
            push_back(back()/10);
            (*this)[size()-2]%=10;
        }
        if(back()<0)f*=-1,(*this)[size()-1]*=-1;
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
    	if(s[i]=='-')
    	{
    		n.f=-1;
    		continue;
    	}
    	n.push_back(s[i]-'0');
    }
    return is;
}
ostream& operator<<(ostream &os,const Wint &n)
{
    if(n.empty()){os<<0;return os;}
	if(n.f==-1)os<<"-";
    for(int i=n.size()-1; i>=0; --i)os<<n[i];
    return os;
}
bool operator!=(const Wint &a,const Wint &b)
{
	if(a.f!=b.f)return 1;
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
	if(a.f!=b.f)return a.f<b.f;
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
Wint operator-(Wint a)
{
	if(~a.f)a.f=-1;
	else a.f=1;
	return a.check();
} 
Wint abs(Wint a)
{
	if(a<0)return -a;
	else return a;
} 
Wint& operator+=(Wint &a,const Wint &b)
{
	Wint c=b;
    if(a.size()<c.size())a.resize(c.size());
    if(a<0&&c>0)
    {
    	if(abs(a)<abs(c))
    	{
    		swap(a,c);
    	}
    }
    else if(a>0&&c<0)
    {
    	if(abs(a)<abs(c))
    	{
    		swap(a,c);
    	}
    }
    for(int i=0; i!=b.size();++i)a[i]+=c[i]*((a.f==c.f)?1:-1);
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
Wint& operator-=(Wint &a,const Wint b)
{
	return a+=-b;
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
    if(a.f!=b.f)n.f=-1;
    else n.f=1;
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
    Wint ans,tmp=b;
    int af=a.f,tmpf=tmp.f;
    a.f=tmp.f=1;
    for(int t=a.size()-tmp.size(); a>=tmp; --t)
    {
        Wint d;
        d.assign(t+1,0);
        d.back()=1;
        Wint c=tmp*d;
        while(a>=c)
        {
            a-=c;
            ans+=d;
        }
    }
    if(af!=tmpf)ans.f=-1;
    else ans.f=1;
    a.f=af,tmp.f=tmpf;
    return ans.check();
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
还是举洛谷高精度算法试炼场中的几道题
我们在main函数中这么写
```cpp
int main()
{
    Wint a,b;
    //可以把b改成int型，仍能正常使用（题切不了）
    cin>>a>>b;
    //LuoguP1601 cout<<a+b<<endl;
    //LuoguP2142 cout<<a-b<<endl;
    //LuoguP1303 cout<<a*b<<endl;
    return 0;
}
```

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。