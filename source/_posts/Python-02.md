---
title: 【Python入门】02 Python基础1——输入输出
date: 2019-01-19 07:46:00
tags: 
  - Python入门
---
在上一期的末尾我们说到三段大差不差的代码

代码的第一行都有计算`100+200`的意思，有的也得到了`300`，第二行都有显示`Hello, world!`的意思也都输出了

第一段代码在Python交互模式下输出了
```
300
Hello, world!
```

第二段代码正常运行输出了
```
300
Hello, world!
```

第三段代码正常运行却只输出了
```
Hello, world!
```

这是为什么呢？  
原因在于Python交互模式会将你执行的代码结果输出，而正常的程序结果只会在写了输出语句时才会输出

**我们在下文的代码中，如果开头有">>> "即表示在Python交互模式下执行**
# 输入和输出
输入和输出需要利用`input()`和`print()`，我们把这种具有一定固定作用(function)的语句称为**函数(function)**。
## 输出
在print()的括号里加上字符串即可输出，就比如
```python
>>> print('Hello, world!')
```

print()里也可以接受多个字符串，用","隔开，会输出一个空格，例如
```python
>>> print('Hello','The','World')
```

输出结果为
```
Hello The World
```

当然print()也支持输出整数或者输出计算结果
```python
>>> print(300)
>>> print(100+200)
```

输出结果均为
```
300
```

当然我们也可以结合字符串把结果变得更好看一些，例如
```python
>>> print('100+200=',100+200)
```

这样就会输出
```
100+200=300
```

现在你已经掌握了基本的输出。
## 输入
现在我们可以通过print()输出结果了，但是我们想要用一个程序计算很多种情况而不想每次都去改程序怎么办？

我们这就要利用input()了，input()可以读取一个字符串  
我们如果要利用这个字符串，就需要一个**变量**去保存  
我们使用name变量（name只是一个名称，不同于print和input这种函数的名字，可以在不冲突的前提下任意起）保存这样的字符串
```python
>>> name=input()
Peter_Matthew
```
当我们输入第一行后，Python交互模式就在等待我们输入了，我们可以任意输入一些东西，比如输入第二行的Peter_Matthew。

输入完后不会有任何提示而是又回到了">>> "的提示状态，我们刚才输入的内容保存到了变量name里  
我们可以直接输入`name`查看变量内容：
```python
>>> name
```

结果是
```
'Peter_Matthew'
```

# 变量
请注意，我们初中设正方形的边长为$a$,则正方形的面积就为$a^2$，我们把a看做一个变量，用于计算不同的a的不同的面积，比如：
- a=1,S=a*a=1*1=1
- a=1.5,S=a*a=1.5*1.5=2.25

计算机中的变量不仅可以为整数或浮点数（带小数的数），还可以是字符串，上文的name变量就是一个字符串

我们可以通过print函数输出变量内容
```python
>>> print(name)
```

这样的结果是
```
Peter_Matthew
```
# 输入输出的一些小点
这样，我们就可以魔改hello world程序了
```python
name=input()
print('hello,',name)
```

运行后输入Peter_Matthew，我们便可以看到
```
hello, Peter_Matthew
```

我们发现没有任何提示就让你输入东西，十分地不方便，我们就想让程序先输出一行提示，这样我们的用户就可以根据提示输入了
```python
print('Please enter your name here: ')
name=input()
print('hello,',name)
```

这样运行的结果是
```
Please enter your name here: 
```

此时我们键入Peter_Matthew，即可得到
```
hello, Peter_Matthew
```

有没有更简单的方法呢？
当然有的，input()函数可以让你显示一个字符串提示用户，于是我们就把代码改成了
```python
name=input('Please enter your name here: ')
print('hello,',name)
```

这样运行的结果和上文的程序类似，只不过上面的程序输入时换行了，下面的程序输入时没有换行（在同一行输入的）
# 小结
任何程序，都是为了执行一定特殊的操作。  
有了输入，用户才能告诉程序所需的信息。  
有了输出，程序运行后才能告诉用户结果。  

输入的英文为Input，输出的英文为Output，因此我们把输入输出统称为Input/Output，简称I/O，有时也写作IO。

------------

# 提前尝试
请执行此代码
```python
a=100
if a>=0:
    print(a)
else:
    print(-a)
```
观察显示的的结果并尝试完成以下任务：
1. 思考这段代码中每行都表示什么意思
2. 修改a的值或将a的值变为输入时赋值

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。