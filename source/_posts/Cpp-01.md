---
title: 【C++入门】01 环境设置
date: 2019-01-13 10:25:00
tags: 
  - C++入门
---
C++入门主要方便的是OI选手，OI选手一般的系统环境为Windows而竞赛环境为Linux。所以我们对环境的介绍会以Windows为主，Linux为辅。

# 编译器
我们要将一份代码转化为计算机能读懂能运行的程序，必不可少的就是编译器。C++的编译器名字叫做g++，请注意gcc是C语言的编译器而不是C++语言的。

在Windows上，默认是没有安装g++的，所以我们需要手动安装g++。请通过下载安装[MinGW](http://www.mingw.org/)来安装g++。安装后，然后打开系统属性，选择“高级系统设置”（“高级”选项卡），找到“环境变量”并点击。
![](https://res.zhangkai.xin/pic/Cpp01-01.png)
然后找到**系统变量**中的`Path`并编辑
![](https://res.zhangkai.xin/pic/Cpp01-02.png)
在末尾以";"分割地址，并输入g++的目录。
![](https://res.zhangkai.xin/pic/Cpp01-03.png)
点击确定。在命令提示符输入g++并敲回车，如果提示
```
g++: fatal error: no input files
compilation terminated.
```
则证明设置正确，如果提示
```
'g++' 不是内部或外部命令，也不是可运行的程序或批处理文件。
```
则说明设置存在问题。

在Linux上，如果OI选手采用的是NOI Linux，那么是已经安装g++的。
如何查看其它版本Linux是否安装了g++？
打开终端，输入g++，如果提示
```
g++: fatal error: no input files
compilation terminated.
```
说明已经安装，若没有，可以在软件中心搜索g++安装，或者打开终端，输入
```
sudo apt-get install g++
```
即可安装。
# 编辑器
在Windows上，作为工程党的肯定要使用[Visual Studio](https://visualstudio.microsoft.com/zh-hans/)，但是作为OIer，入门级的装备肯定是Dev C++。
首先我们下载`Dev C++ 5.7.1`。为什么选择5.7.1版本？因为这个版本是支持MinGW的最稳定版本，而Dev C++最新的5.11版本是支持TDM-GCC的版本，由于存在bug导致调试不了，所以不推荐大家使用。
下载完以后我们安装，打开Dev C++便可以编写一份愉悦的代码。

同时，在Windows上，我也推荐使用Notepad++作为编辑器，Notepad++作为一款轻量化的编辑器但是支持许多插件，可以做到十分优秀的编辑效果。

在Linux上，大部分选手都推荐大家使用vim作为编辑器，这是因为vim有着极多的快捷键可以帮助你快速编辑甚至不需要用到鼠标。但是作为从Windows上Dev C++过渡到Linux的OIer，我的首选是系统自带的编辑器——gedit，它具有强大的插件后台，使得体验效果与Dev C++类似（除了不能使用方便的调试）。

同时，在Linux上，我也推荐使用Notepad++作为编辑器，但是Linux上没有Notepad++，我们只能采用一个替代版——Notepadqq。在终端执行以下命令，即可安装Notepadqq。
```
sudo apt-get update
sudo apt-get install snapd
sudo snap install --classic notepadqq
```
# 使用
请在编辑器里编辑一份代码（可从提前尝试部分复制），然后保存成`.cpp`文件（例如a.cpp）。

请将命令行/终端路径调整到保存文件的目录然后执行以下命令进行编译
```
g++ a.cpp -o a
```
其中a.cpp你是保存的文件名，-o表示不开启任何优化开关（以后会讲），a即你想要的可执行文件名字（在Windows的可执行文件扩展名必须为.exe而Linux则不是，所以执行此命令后Windows生成的程序会在末尾增加.exe而Linux不会）。

在Windows下，在命令行输入
```
a.exe
```
执行程序。

在Linux下，在终端输入
```
./a
```
执行程序，其中的.表示当前目录./a即为当前目录下的a程序。

如果是复制提前尝试部分的代码，应该可以在命令行或终端看到
```
Hello, world!
```
表示运行了该程序。

------------

# 提前尝试
请在编辑器中输入以下代码并编译执行
```cpp
#include<iostream>
using namespace std;
int main()
{
    cout<<"Hello, world!"<<endl;
    return 0;
}
```
观察显示的的结果并尝试完成以下任务：
1. 思考这段代码中每行都表示什么意思
2. 删除某些行测试出一个程序最短长度
3. 试着修改显示内容甚至输出多行内容

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。