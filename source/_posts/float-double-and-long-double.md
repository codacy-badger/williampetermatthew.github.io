---
title: float类型、double类型和long double类型的二进制存储
date: 2018-10-11 16:44:13
tags: 
  - 科技
---
# 环境
在32位环境下， float占用32位，double占用64位，long double占用80位

目前C/C++编译器标准都遵照IEEE制定的浮点数表示法来进行float,double,long double运算。这种结构是一种科学计数法，用符号、指数和尾数来表示，底数定为2——即把一个浮点数表示为尾数乘以2的指数次方再添上符号。

|  | 符号位阶码 | 阶码 | 整数 | 尾数 | 长度 |
| -----------: | -----------: | -----------: | -----------: | -----------: | -----------: |
| float | 1 | 8 | 0 | 23 | 32 |
| double | 1 | 11 | 0 | 52 | 64 |
| long double | 1 | 15 | 1 | 63 | 80 |

![](https://res.zhangkai.xin/pic/590px-Float_example.svg.png)

![](https://res.zhangkai.xin/pic/618px-IEEE_754_Double_Floating_Point_Format.svg.png)

![](https://res.zhangkai.xin/pic/762px-X86_Extended_Floating_Point_Format.svg.png)

# 举例说明
将100分别转化为float型,double型和long double型的二进制表达。

$100=(1+\frac{1}{2}+\frac{1}{16})\times2^6$

## float

转为float型为

100为正数，符号位为0，

阶码，一共8位，因为指数可以为负，为了便于计算，规定都先加上127，在这里6+127=133转为二进制为10000101

尾数转为1.1001，因为最高位的1 **不写入内存**，则尾数转为23位二进制为10010000000000000000000

合在一起就是01000010110010000000000000000000

## double

转为double型为

100为正数，符号位为0，

阶码，一共11位，因为指数可以为负，为了便于计算，规定都先加上1023，在这里6+1023=1029转为二进制为10000000101

尾数转为1.1001，因为最高位的1 **不写入内存**，则尾数转为52位二进制为1001000000000000000000000000000000000000000000000000

合在一起就是0100000001011001000000000000000000000000000000000000000000000000

## long double

转为long double型为

100为正数，符号位为0，

阶码，一共15位，因为指数可以为负，为了便于计算，规定都先加上16383，在这里6+16383=16389转为二进制为100000000000101

尾数转为1.1001，将最高位的1 **写入内存**，则尾数转为64位二进制为1100100000000000000000000000000000000000000000000000000000000000

合在一起就是01000000000001011100100000000000000000000000000000000000000000000000000000000000

# 转换到二进制

## float

将float转为二进制字符串
```cpp
//str should have at least 32 byte.
void floattostr(float* a,char* str)
{
	unsigned int c;
	c=((unsigned int*)a)[0]; 
	for(int i=0;i<32;i++)
    {
		str[31-i]=(char)(c&1)+'0';
		c>>=1;
	}
	str[32]='\0';
}
```

## double

将double转为二进制字符串
```cpp
//str should have at least 64 byte.
void doubletostr(double* a,char* str)
{
	long long c;
	c=((long long*)a)[0]; 
	for(int i=0;i<64;i++)
    {
		str[63-i]=(char)(c&1)+'0';
		c>>=1;
	}
	str[64]='\0';
}
```

## long double

将long double转为二进制字符串
```cpp
//str should have at least 80 byte.
void doubletostr(double* a,char* str)
{
	__int128 c;
	c=((__int128*)a)[0]; 
	for(int i=0;i<80;i++)
    {
		str[79-i]=(char)(c&1)+'0';
		c>>=1;
	}
	str[80]='\0';
}
```
# 从二进制转换

## float

将32位二进制字符串转为float
```cpp
float strtofloat(char* str)
{
	unsigned int flt=0;
	for(int i=0;i<31;i++)
    {
		flt+=(str[i]-'0');
		flt<<=1;
	}
	dbl+=(str[31]-'0');
	float* ret=(float*)&flt;
	return *ret;
}
```

## double

将64位二进制字符串转为double
```cpp
double strtodbl(char* str)
{
	long long dbl=0;
	for(int i=0;i<63;i++)
    {
		dbl+=(str[i]-'0');
		dbl<<=1;
	}
	dbl+=(str[63]-'0');
	double* db=(double*)&dbl;
	return *db;
}
```

## long double

将80位二进制字符串转为long double
```cpp
long double strtolongdbl(char* str)
{
	__int128 ldb=0;
	for(int i=0;i<79;i++)
    {
		ldb+=(str[i]-'0');
		ldb<<=1;
	}
	ldb+=(str[79]-'0');
	long double* ld=(long double*)&ldb;
	return *ld;
}
```

# 其他

1. 在内存中占有的字节数不同

	单精度浮点数在机内存占4个字节

	双精度浮点数在机内存占8个字节
    
    长双精度浮点数在机内存占10个字节

2. 有效数字位数不同

	单精度浮点数有效数字8位

	双精度浮点数有效数字16位
    
    长双精度浮点数有效数字19位

3. 数值取值范围

	单精度浮点数的表示范围：-3.40E+38~3.40E+38

	双精度浮点数的表示范围：-1.79E+308~1.79E+308
    
    长双精度浮点数的表示范围：-1.2E+4932~1.2E+4932 

4. 在程序中处理速度不同

	一般来说，CPU处理单精度浮点数的速度比处理双精度浮点数快

	如果不声明，默认小数为double类型，所以如果要用float的话，必须进行强转

	例如：float  a=1.3; 会编译报错，正确的写法 float a = (float)1.3;或者float a = 1.3f;（f或F都可以不区分大小写）

注意：float是8位有效数字，第7位数字将会四舍五入

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。