---
title: 重载运算符和重载函数
date: 2018-10-09 15:48:06
tags: 
  - 科技
---
# 重载运算符
## 可重载运算符/不可重载运算符
下面是可重载的运算符列表：
![可重载运算符列表](https://res.zhangkai.xin/pic/QQ浏览器截图20181018105607.png)

下面是不可重载的运算符列表：
- .：成员访问运算符
- ```.*```, ```->*```：成员指针访问运算符
- ::：域运算符
- sizeof：长度运算符
- ?:：条件运算符
- ```#```：预处理符号

我们将以矩阵为例示例重载运算符。
## 双目算术运算符
```cpp
matrix operator*(matrix x,matrix y)
{
	matrix tmp;
	tmp.clear();
	for(int i=1;i<=x.n;i++)
		for(int j=1;j<=y.m;j++)
			for(int k=1;k<=x.m;k++)
				tmp.a[i][j]+=x.a[i][k]*y.a[k][j];
	return tmp;
}
```
## 关系运算符
```cpp
bool operator==(const matrix x,const matrix y)
{
	if(x.n!=y.n)return 0;
	if(x.m!=y.m)return 0;
	for(int i=1;i<=x.n;i++)
		for(int j=1;j<=x.m;j++)
			if(x.a[i][j]!=y.a[i][j])
				return 0;
	return 1;
}
```
## ~~逻辑运算符~~
~~由于重载逻辑运算符过于毒瘤而被隐藏~~
```cpp
//bool operator!(matrix x)
//{
//	for(int i=1;i<=x.n;i++)
//		for(int j=1;j<=x.m;j++)
//			if(!x.a[i][j])
//				return 1;
//	return 0;
//} 
```
## 单目运算符
```cpp
matrix operator-(matrix x)
{
	for(int i=1;i<=x.n;i++)
		for(int j=1;j<=x.m;j++)
			x.a[i][j]=-x.a[i][j];
	return x;
} 
```
## 自增自减运算符
```cpp
matrix& operator++(matrix &x)
{
	for(int i=1;i<=x.n;i++)
		for(int j=1;j<=x.m;j++)
			x.a[i][j]+=1;
	return x;
}//前置++ 
matrix operator++(matrix &x,int flag)
{
	matrix tmp=x;
	for(int i=1;i<=x.n;i++)
		for(int j=1;j<=x.m;j++)
			x.a[i][j]+=1;
	return tmp;
}//后置++ 
```
## 位运算符
```cpp
matrix operator~(matrix x)
{
	matrix tmp;
	tmp.clear();
	for(int i=1;i<=x.n;i++)
		for(int j=1;j<=x.m;j++)
			tmp.a[i][j]=~x.a[i][j];
	return tmp;
} 
```
## 赋值运算符
```cpp
matrix& operator*=(matrix &x,matrix y)
{
	return x=x*y;
}
```
# 重载函数
对于C++中预制的函数，有的函数由于固定了类型，比如pow在<math.h>中的返回值类型为double，传的两个参数类型也为double，那么如果我们使用int进行操作，在某些编译选项下（例如-lm），可能会CE；max和min函数在STL中的定义返回值类型为```const<typename _Tp>&```，传的两个参数类型也均为```const<typename _Tp>&```，这就意味着你不能把int和long long两个类型的数同时传到max或min里，否则就会CE。

遇到这种情况，我们就需要重载函数，有的人可能称这种为手写函数，但实际上我们写数据结构之类的函数是手写函数没问题，但max或min这种实际上是重载函数。

对于矩阵重载pow函数的例子：
```cpp
matrix pow(matrix a,int k)
{
	matrix ans;
	ans.init();
	while(k)
	{
		if(k&1)ans*=a;
		a*=a;
		k>>=1;
	}
	return ans;
}
```

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。