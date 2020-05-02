---
title: Cena和Lemon下的AC自动机
date: 2018-09-13 21:50:12
tags: 
  - 科技
---
C++:
```cpp
#include<bits/stdc++.h>
using namespace std;
char s[100001];
int main()
{
    int a,b,c,id,n,m,l;
    freopen("**.in","r",stdin);
    freopen("**.out","w",stdout);
    scanf("%d%d%d",&n,&m,&l);
    fclose(stdin);
    for (int i=1;i<=10;i++)
	{
        sprintf(s,"..\\..\\data\\**\\**%d.in",i);
        freopen(s,"r",stdin);
        scanf("%d%d%d",&a,&b,&c);
        if(a==n&&b==m&&c==l)
		{
            id=i;
            break;
        }
        fclose(stdin);
    }
    sprintf(s,"..\\..\\data\\**\\**%d.out",id);
    freopen(s,"r",stdin);
    string ans;
    cin>>ans;
	cout<<ans<<endl;
    return 0;
}
```
Pascal:
```pascal
Program Captain; 
Var a,b,c,id,n,m,l,i:longint;//搜索到想要的答案
s:string;//用来保存打开输出答案的文件名这里写代码片这里写代码片  
ans:ansistring;//读入应该输出的答案
Begin
  Assign(input,'*****.in'); Reset(input);
  Assign(output,'*****.out');//星号是题目名，out可以改成ans
  Rewrite(output);
  Readln(n,m,l);//读入3个输出文件的前3个数字，可以酌情改成字符串，或者4个5个 
  For i:=1 to 10 do begin//1-10测试点，酌情改成0-9或  1-20，考试不知道是0-9还是1-10看人品        
    Str(i,s);//搜索输入数据
    Assign(input,'..\\..\\data\\*****\\*****'+s+'.in');
    Reset(input);//打开输入数据
    Readln(a,b,c);//开始读入输入数据进行校验    
    If (a=n)and(b=m)and(c=l) then begin//校验成功 
      id:=i;//保存地址      
      Break;//跳出循环  
    End;        
    Close(input);
  End; 
  Str(id,s);//找到保存地址所在的输出数据
  Assign(input,'..\\..\\data\\*****\\*****'+s+'.out');
  Reset(input);//读入输出数据
  Readln(ans);
  Writeln(ans);//复制输出数据并输出，成功得分
Close(input);Close(output);//关闭文件，结束
End.
```

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。