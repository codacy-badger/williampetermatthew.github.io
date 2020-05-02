---
title: 【Python入门】01 环境设置
date: 2019-01-13 14:42:00
tags: 
  - Python入门
---
为什么我突然写这个坑贼大的系列？原因是我一个学长貌似他们要学Python于是找到了我，额，为了防止像我在机房教高一入门把他们全体说蒙的情况发生，我只好硬着头皮总结下这些东西，尽量照顾萌新，但是我还是要默认读者具有计算机基础知识，否则还是很难继续阅读的。

# 安装Python
相比我们介绍到的C++来说，Python的编辑器和编译器被官方融于一体，只需要下载[官网](https://www.python.org/downloads/release)上的Python安装即可。
![](https://res.zhangkai.xin/pic/Python01-01.png)
特别地，我们要在安装Python时选中`Add Python 3.7 to PATH`。
# 运行Python
我们打开命令行，输入
```
python
```

如果此时出现
```
Python 3.7.2 (tags/v3.7.2:9a3ffc0492, Dec 23 2018, 22:20:52) [MSC v.1916 32 bit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```
这样的文字，那么就说明已经安装成功。

如果提示
```
'python' 不是内部或外部命令，也不是可运行的程序或批处理文件。
```
则说明未安装或者在安装时没有选择`Add Python 3.7 to PATH`。

# Python交互模式
在命令行输入python后进入的样子称为`Python交互模式`。
```
┌────────────────────────────────────────────────────────┐
│管理员：C:\Windows\System32\cmd.exe - Python       - □ x │
├────────────────────────────────────────────────────────┤
│Microsoft Windows [版本 6.1.7601]                        │
│版权所有 (c) 2009 Microsoft Corporation。保留所有权利。     │
│                                                        │
│C:\Users\Administrator>python                           │
│Python 3.7.2 (tags/v3.7.2:9a3ffc0492, Dec 23 2018, 22:20│
│:52) [MSC v.1916 32 bit (Intel)] on win32               │
│Type "help", "copyright", "credits" or "license" for mor│
│e information.                                          │
│>>> _                                                   │
│                                                        │
│                                                        │
└────────────────────────────────────────────────────────┘
```
可以输入`exit()`退出此交互模式

您可以写一个代码（可以从提前尝试部分复制）保存为`.py`文件（例如a.py)
请将命令行路径调整到保存文件的目录然后在命令行模式执行
```
python a.py
```
如果是复制提前尝试部分的代码，应该可以在命令行看到
```
300
Hello, world!
```
或
```
Hello, world!
```
表示运行了该程序。

Python交互模式的代码是输入一行，执行一行，而命令行模式下直接运行.py文件是一次性执行该文件内的所有代码。可见，Python交互模式主要是为了调试Python代码用的，也便于初学者学习，它不是正式运行Python代码的环境！

# 系统菜单中的四个东西
- IDLE (Python 3.7 32-bit)
- Python 3.7 (32-bit)
- Python 3.7 Manuals (32-bit)
- Python 3.7 Module Docs (32-bit)

第一个东西点开是一个白框下的Python交互模式，我们可以点击菜单栏上的`File->New File`来写一份新代码，或者`File->Open`打开代码，然后在新弹出来的窗口中选择`Run->Run Module`执行它。
第二个东西点开发现就是Python交互模式，与命令行打开的不用的是，此时输入exit()不会进入命令行模式而是会直接关闭窗口
第三个东西是一个英文手册，是一个完全看不懂而且可以忽略的东西
第四个东西一点开发现弹出了一个网页，发现也是英文手册，也可以忽略。对了，如果要退出此状态要找到打开的一个小黑框，在里面输入q敲回车即可

------------

# 提前尝试
请在Python交互模式中分行输入以下代码并执行
```python
100+200
print('Hello, world!')
```

请创建两个.py文件输入以下代码并分别执行
```python
print(100+200)
print('Hello, world!')
```
```python
100+200
print('Hello, world!')
```
观察显示的的结果并尝试完成以下任务：
1. 思考这段代码中每行都表示什么意思
2. 试着修改显示内容并尝试减乘和除法
3. 为什么最后的代码执行时没有第一行

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。