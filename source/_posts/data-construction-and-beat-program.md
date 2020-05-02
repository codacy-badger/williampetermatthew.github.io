---
title: 数据构造与对拍程序
date: 2018-09-10 20:52:32
tags: 
  - 科技
---
# 数据构造
作为一个~~毒瘤~~出题人，我们要学着出~~毒瘤~~数据~~卡做题人~~；作为一个~~蒟蒻~~做题人，我们要学着出~~毒瘤~~数据尝试~~卡自己的程序并~~调试，下面简单介绍出数据的一些方法。
## 总程序的写法
```cpp
#include<bits/stdc++.h>
using namespace std;
const int T=10,L=200;
const char* problemname="";//your problem's name
char buf[L]={};
int randomint(int l,int r)
{
	return (rand()+rand()*32768)%(r-l+1)+l;
}
void make_data(int t)
{	
	//do something
	return ;
}
int main()
{
	srand(time(0));
	for(int t=1;t<=T;++t)
	{
		cerr<<"start time="<<clock()<<endl;
		sprintf(buf,"%s%d.in",problemname,t);
		freopen(buf,"w",stdout);
		
		make_data(t);
		
		fclose(stdout);
		cerr<<"t1="<<clock()<<endl;
		sprintf(buf,"copy %s%d.in %s.in >nul",problemname,t,problemname);
		system(buf);
		sprintf(buf,"\"%s.exe\"",problemname);
		system(buf);
		sprintf(buf,"copy %s.out %s%d.ans >nul",problemname,problemname,t);
		system(buf);
		sprintf(buf,"del %s.in",problemname);
		system(buf);
		sprintf(buf,"del %s.out",problemname);
		system(buf);
		cerr<<"data "<<t<<" has been made."<<endl;
		cerr<<"end time="<<clock()<<endl;
	}
}
```
下面对上面的程序做出详细解释：

**请在主程序中写上函数srand，否则会导致每次制造出来的数据都一样！**  
T为你要生成的数据组数，L是操作用字符串buf的长度。  
problemname即为你题目的名称。  
buf是操作用字符串。  
C++中的rand()函数可以随机生成一个$[-2^{15},2^{15})$以内的整数，利用组合我们便可以生成一个。randomint是生成一个int范围（$[-2^{31},2^{31})$）内的整数。如果你需要生成一个long long范围（$[-2^{63},2^{63})$）的整数，你可以使用下面的函数：
```cpp
long long randomlonglong(long long l,long long r)
{
	return (rand()*1ll+rand()*32768ll+rand()*2147483648ll+rand()*140737488355328ll)%(r-l+1ll)+l;
}
```
make_data函数是你需要修改的函数，你在这里输出数据，直接使用标准输出即可，如果你需要按数据点生成，可以使用传过来的t，对于一个随机数可以采用randomint函数实现。  
最后是主函数，这里面写的是操作，你可以在运行程序时看到程序的运行时间，可以简单检验std的时限。

这里讲解三个函数的使用方法：
1. system用途为执行传过来的参数的字符串内容。例如：system("pause")执行的结果为DOS命令中执行pause指令的结果，即“请按任意键继续. . . ”；
2. sprintf用法基本同printf，只不过printf是经过缓存区向标准输出输出内容，而sprintf是向第一参数传过来的字符串中写入内容。
3. cerr用法同cout，作用是不经过缓存区直接向标准输出输出内容，只会在显示区输出而不会向重定向文件中输出。

**生成时需要注意要把和题目名称相同的标程cpp（例如program.cpp）放在和datamaker同一个文件夹下，同时要注意标程需要写文件输入输出。**

如果在system函数行编译错误，请手动引用<windows.h>库。

## 简单数据生成

### 1~n的随机排列

方法一：  
利用STL库中的next_permutation函数。
```cpp
	int n=randomint(1,1e5);
	int a[100005];
	for(int i=1;i<=n;i++)a[i]=i;
	int T=randomint(1,n/*或者XJB写的一个数，反正要小于n!*/);
	while(T--)next_permutation(a+1,a+n+1);
	cout<<n<<endl;
	for(int i=1;i<=n;i++)cout<<a[i]<<" ";
```
- 优点：写着简单。
- 缺点：next_permutation返回的其实是一个bool，当数列排序到最后一组排列时便不可再次排列，一般是n比较小的时候会出现。

方法二：  
利用bool数组判断是否出现。
```cpp
	int n=randomint(1,1e5);
	int a[100005];
	bool b[100005];
	int l=1,r=n;
	for(int i=1,x;i<=n;i++)
	{
		while(b[l])l++;
		while(b[r])r--;
		do{x=randomint(l,r);}while(b[x]);b[x]=1;
		a[i]=x;
	}
	cout<<n<<endl;
	for(int i=1;i<=n;i++)cout<<a[i]<<" ";
```
- 优点：可以算是完全随机的排列。
- 缺点：写着没那么简单，刚开始这样写可能不会写l和r，这时就会使程序运行时间在某些时候极长（这时候建议srand(19260817)）。

### 字符（串）生成

![0-127](https://res.zhangkai.xin/pic/0-127.jpg)![128-255](https://res.zhangkai.xin/pic/128-255.jpg)

正如上表所示，我们要生成字符串的时候需要注意范围（0~127，128~255实际有问题），并且33~126的值才有实际意义。  
实际我们写的时候需要写
```cpp
	char c=randomint(33,126);
```
同时，如果你需要一个字符串，可以直接写
```cpp
	string s;
	for(int i=1;i<=len;i++)
		s+=(char)randomint(33,126);
```
## 图
### n个点m条边的图

方法是每次随机选择两个点，并连接一条边。
```cpp
	int n=randomint(1,1e5),m=randomint(1,1e5);
	int bian[100005][2];
	for(int i=1,x,y;i<=m;i++)
	{
		x=randomint(1,n);
		y=randomint(1,n);
		bian[i][0]=x;
		bian[i][1]=y;
	}
	printf("%d %d\n",n,m);
	for(int i=1;i<=m;i++)
	{
		printf("%d %d\n",bian[i][0],bian[i][1]);
	}
```

### 无自环？

只需将
```cpp
	y=randomint(1,n);
```
替换成
```cpp
	do{y=randomint(1,n);}while(x==y);
```
即可。

### 无重边？

增加map判重即可，但是要注意你的m限制。
```cpp
//无向无重边
	int n=randomint(1,1e5),m;
	int bian[100005][2];
	map<pair<int,int>,int>ma;
	do{m=randomint(1,1e5);}while(m*2>n*(n-1));
	for(int i=1,x,y;i<=m;i++)
	{
		do
		{
			x=randomint(1,n);
			do{y=randomint(1,n);}while(x==y);
		}while(ma.count(make_pair(min(x,y),max(x,y))));
		ma.insert(make_pair(make_pair(min(x,y),max(x,y)),i));
		bian[i][0]=x;
		bian[i][1]=y;
	}
	printf("%d %d\n",n,m);
	for(int i=1;i<=m;i++)
	{
		printf("%d %d\n",bian[i][0],bian[i][1]);
	}
//有向无重边
	int n=randomint(1,1e5),m;
	int bian[100005][2];
	map<pair<int,int>,int>ma;
	do{m=randomint(1,1e5);}while(m>n*(n-1));
	for(int i=1,x,y;i<=m;i++)
	{
		do
		{
			x=randomint(1,n);
			do{y=randomint(1,n);}while(x==y);
		}while(ma.count(make_pair(x,y));
		ma.insert(make_pair(make_pair(x,y),i));
		bian[i][0]=x;
		bian[i][1]=y;
	}
	printf("%d %d\n",n,m);
	for(int i=1;i<=m;i++)
	{
		printf("%d %d\n",bian[i][0],bian[i][1]);
	}
```

### 一棵树？

每次选择已经连接的一个节点和没连接的一个节点建边即可。
```cpp
	int n=randomint(1,1e5);
	int bian[100005][2];
	bool dian[100005];
	int x=randomint(1,n),y;
	do{y=randomint(1,n);}while(x==y);
	bian[1][0]=x;
	bian[1][1]=y;
	dian[x]=1;
	dian[y]=1;
	for(int i=2;i<n;i++)
	{
		do{x=randomint(1,n);}while(!dian[x]);
		do{y=randomint(1,n);}while(dian[y]);
		bian[i][0]=x;
		bian[i][1]=y;
		dian[x]=1;
		dian[y]=1;
	}
	printf("%d\n",n);
	for(int i=1;i<n;i++)
	{
		printf("%d %d\n",bian[i][0],bian[i][1]);
	}
```

### 一条链？

树的特殊情况，加上每个点访问次数即可。
```cpp
	int n=randomint(1,1e5);
	int bian[100005][2];
	int dian[100005];
	int x=randomint(1,n),y;
	do{y=randomint(1,n);}while(x==y);
	bian[1][0]=x;
	bian[1][1]=y;
	dian[x]=1;
	dian[y]=1;
	for(int i=2;i<n;i++)
	{
		do{x=randomint(1,n);}while(!dian[x]||dian[x]==2);
		do{y=randomint(1,n);}while(dian[y]);
		bian[i][0]=x;
		bian[i][1]=y;
		dian[x]=2;
		dian[y]=1;
	}
	printf("%d\n",n);
	for(int i=1;i<n;i++)
	{
		printf("%d %d\n",bian[i][0],bian[i][1]);
	}
```

### 菊花图？

树的特殊情况。
```cpp
	int n=randomint(1,1e5),s=randomint(1,n);
	int bian[100005][2];
	bool dian[100005];
	for(int i=1,x,y;i<n;i++)
	{
		x=s;
		do{y=randomint(1,n);}while(dian[y]);
		bian[i][0]=x;
		bian[i][1]=y;
		dian[x]=2;
		dian[y]=1;
	}
	printf("%d\n",n);
	for(int i=1;i<n;i++)
	{
		printf("%d %d\n",bian[i][0],bian[i][1]);
	}
```

### 蒲公英？
菊花图+一条链即可。

### 连通无向图？

在树的基础上连边即可
```cpp
	int n=randomint(1,5),m;
	int bian[100005][2];
	bool dian[100005];
	map<pair<int,int>,int>ma;
	do{m=randomint(1,1e5);}while(m*2>n*(n-1));
	int x=randomint(1,n),y;
	do{y=randomint(1,n);}while(x==y);
	bian[1][0]=x;
	bian[1][1]=y;
	dian[x]=1;
	dian[y]=1;
	ma.insert(make_pair(make_pair(min(x,y),max(x,y)),1));
	for(int i=2;i<n;i++)
	{
		do{x=randomint(1,n);}while(!dian[x]);
		do{y=randomint(1,n);}while(dian[y]);
		bian[i][0]=x;
		bian[i][1]=y;
		dian[x]=1;
		dian[y]=1;
		ma.insert(make_pair(make_pair(min(x,y),max(x,y)),i));
	}
	for(int i=n;i<=m;i++)
	{
		do
		{
			x=randomint(1,n);
			do{y=randomint(1,n);}while(x==y);
		}while(ma.count(make_pair(min(x,y),max(x,y))));
		ma.insert(make_pair(make_pair(min(x,y),max(x,y)),i));
		bian[i][0]=x;
		bian[i][1]=y;
	}
	printf("%d %d\n",n,m);
	for(int i=1;i<=m;i++)
	{
		printf("%d %d\n",bian[i][0],bian[i][1]);
	}
```



# 对拍程序
在考场上，一般会写一个正确的暴力和自己尝试写的正解，为了校验自己程序的正确性，我们可以利用上面所述的数据构造程序并写一个对拍程序来校验程序的正确性。
## 使用bat文件校验
我们知道Windows系统自带的DOS命令可以很方便的帮助我们完成校验任务，我们不妨学着写一个简单的bat文件方便快捷地进行我们的对拍。
```batch
@echo off
:again
data.exe > problemname.in
force.exe < problemname.in > problemname.ans
program.exe < problemname.in > problemname.out
fc problemname.ans problemname.out
if not errorlevel 1 goto again
pause
```
下面对上面的bat文件做出详细解释：

@echo off的作用是关闭无关显示，如果没有这行，你会看到一堆提示，这都是毫无必要的信息。  
:again这里是声明一个again名称的运行节点，使执行命令时能够跳转到这行。  
下面三行分别是 数据生成器 暴力程序 你的程序 三个**已经编译过**的程序。如果你的程序已经写了文件输入输出，请将这三行替换成下面的三行即可。
```batch
data.exe
force.exe
program.exe
```
fc是文件比较命令，如果文件比较不出差异会返回0，否则会返回1。  
if not errorlevel 1 goto again是指如果没有返回1则执行跳转至again运行节点。  
pause就是暂停程序。

如果我们的程序已经拍出了不同，我们就应该暂停并查看数据进行调试。

**需要注意的一点是，对拍的时候，需要把已经编译过的数据生成器、暴力程序、你的程序和对拍bat放到同一个文件夹下。**
## 使用C++写法校验
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
	while(1)
	{
		system("data.exe > problemname.in");
		system("force.exe < problemname.in > problemname.ans");
		system("program.exe < problemname.in > problemname.out");
		if(system("fc problemname.ans problemname.out"))break;
	}
	system("pause");
}
```
同bat的写法，其实就是用C++的system函数执行那些命令而已，但是与bat不同的是此处fc命令的返回值会直接给执行该条命令的system函数，所以不用再写if指令。这样的好处是节省了大脑记忆if命令的空间~~（当然我作为一个修计算机的肯定是会这条指令的）~~。

另外，同bat写法一样，如果你的程序已经写了文件输入输出，请将这三行替换成下面的三行即可。
```cpp
		system("data.exe");
		system("force.exe");
		system("program.exe");
```

如果在system函数行编译错误，请手动引用<windows.h>库。

## 次数的显示
### bat
```batch
@echo off
set t=0
:again
set /a t+=1
echo times: %t%
data.exe > problemname.in
force.exe < problemname.in > problemname.ans
program.exe < problemname.in > problemname.out
fc problemname.ans problemname.out
if not errorlevel 1 goto again
pause
```
### C++
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
	for(int t=1;t;t++)
	{
		printf("times: %d\n",t);
		system("data.exe > problemname.in");
		system("force.exe < problemname.in > problemname.ans");
		system("program.exe < problemname.in > problemname.out");
		if(system("fc problemname.ans problemname.out"))break;
	}
	system("pause");
}
```
## 时间的显示
### bat
```batch
@echo off
:again
set t1=%time:~0,2%%time:~3,2%%time:~6,2%
data.exe > problemname.in
set t2=%time:~0,2%%time:~3,2%%time:~6,2%
set /a t3=%t2%-%t1%
echo data	time=%t3% ms
set t1=%time:~0,2%%time:~3,2%%time:~6,2%
force.exe < problemname.in > problemname.ans
set t2=%time:~0,2%%time:~3,2%%time:~6,2%
set /a t3=%t2%-%t1%
echo force	time=%t3% ms
set t1=%time:~0,2%%time:~3,2%%time:~6,2%
program.exe < problemname.in > problemname.out
set t2=%time:~0,2%%time:~3,2%%time:~6,2%
set /a t3=%t2%-%t1%
echo program	time=%t3% ms
fc problemname.ans problemname.out
if not errorlevel 1 goto again
pause
```
### C++
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
	while(1)
	{
		unsigned int t1=clock(),t2,t3;
		system("data.exe > problemname.in");
		t2=clock();
		t3=t2-t1;
		printf("data	time=%u ms\n",t3);
		t1=clock();
		system("force.exe < problemname.in > problemname.ans");
		t2=clock();
		t3=t2-t1;
		printf("force   time=%u ms\n",t3);
		t1=clock();
		system("program.exe < problemname.in > problemname.out");
		t2=clock();
		t3=t2-t1;
		printf("program time=%u ms\n",t3);
		if(system("fc problemname.ans problemname.out"))break;
	}
	system("pause");
}
```
文件输入输出时替换模式参照上文。

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。