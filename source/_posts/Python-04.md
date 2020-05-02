---
title: 【Python入门】04 Python基础3——字符串和编码
date: 2019-02-15 08:24:00
tags: 
  - Python入门
---
在上一期的末尾我们说到一段代码
```python
# -*- coding: utf-8 -*-
n = 123
f = 456.789
s1 = 'Hello, world'
s2 = 'Hello, \'Adam\''
s3 = r'Hello, "Bart"'
s4 = r'''Hello,
Lisa!'''
```
我们看到有一行注释`# -*- coding: utf-8 -*-`

这个表示编码采用UTF-8编码，那么什么是字符编码呢？
# 字符编码
我们已经知道字符串也是一种数据类型，但是，字符串还存在一个特殊的编码问题。

计算机在设计时以8比特（bit）作为一个字节（byte），所以一个字节最大表示整数为255（$11111111_{(2)}=255_{(10)}$），如果要表示更大的整数，需要用更多的字节，比如两个字节最大为65535,4个字节最大为4294967295。  
由于计算机是美国人发明的，最早只有127个字符被编码写入计算机，也就是大小写英文字母、数字和一些符号，它们被称为`ASCII`编码，比如大写字母A是65，小写字母z是122。  
但是明显中文一个字节肯定不够用，起码也要两个字节，而且不能与ASCII编码冲突，于是中国便制定了`GB2312`编码，用来把中文编进去。  
你可以想到的是，全世界有上百种语言，日本把日文编到`Shift-JIS`编码里，韩国把韩文编到`EUC-KR`编码里，甚至中文的繁体字都采用另一种编码`Big5`。各国有各国的标准，就会不可避免的产生冲突，结果就是，在多语言的文本中，会出现乱码。

![字符编码的问题真是令人头疼！](https://res.zhangkai.xin/pic/Python04-01.png)

因此，`Unicode`应运而生，Unicode把所有语言统一到一套编码，这样就不会再出现乱码问题了。  
Unicode标准不断地在发展，最常用地是用两个字节表示一个字符（偏僻字符需要4个字节）。现代的操作系统和大多数编程语言都直接支持Unicode。  
现在，捋一捋ASCII和Unicode的编码区别：ASCII编码是一个字节，Unicode通常是两个字节。

例如：  
- 字母`A`用ASCII编码是十进制`65`，二进制`01000001`；  
- 字符`0`用ASCII编码是十进制`48`，二进制`00110000`，注意这里的`0`是字符`'0'`而不是整数`0`；  
- 汉字`中`超出ASCII编码范围，用Unicode编码是十进制的`20013`，二进制`01001110 00101101`。  

你可以猜测的是，如果把ASCII编码的`A`用Unicode编码，在前方补0，因此`A`的Unicode编码是`00000000 01000001`。

但是新的问题又出现了：如果统一成Unicode编码，乱码问题没了，但是如果你写的文本基本上全是英文的话，用Unicode编码比ASCII编码多一倍空间，所以储存和传输上十分不划算。  
所以本着节约的精神，又出现了把Unicode转化为“可变长编码”的`UTF-8`编码。UTF-8编码把一个Unicode字符根据不同的数字大小编码成1-6个字节，常用的英文字母被编码成1个字节，汉字通常是三个字节，只有生僻的字符才会被编码成4-6个字节。如果你要传输的文本包含大量英文字符，用UTF-8编码就能节省空间：

| 字符 | ASCII | Unicode | UTF-8 |
| -----------: | -----------: | -----------: | -----------: |
| A | 01000001 | 00000000 01000001 | 01000001 |
| 中 | X | 01001110 00101101 | 11100100 10111000 10101101 |

从上面的表格还可以发现，UTF-8有个额外的好处，就是ASCII编码实际上是UTF-8编码的一部分，所以，大量只支持ASCII编码的历史遗留软件可以在UTF-8编码下继续工作。  
搞清楚了ASCII、Unicode和UTF-8的关系，我们现在总结下计算机系统通用的字符编码工作方式：  
在计算机内存中，统一使用Unicode编码，当需要保存到硬盘或者传输的时候，就转换为UTF-8编码。  
用记事本编辑的时候，从文件读取的UTF-8字符被转换为Unicode字符到内存里，编辑完成后，保存的时候再把Unicode转换为UTF-8保存到文件：

![](https://res.zhangkai.xin/pic/Python04-02.png)

浏览网页时，服务器会把动态生成的Unicode内容转换为UTF-8再传输到浏览器：

![](https://res.zhangkai.xin/pic/Python04-03.png)

所以你看到很多网页的源码上会有类似`<meta charset="UTF-8" />`的信息，表示该网页正是用的UTF-8编码。

# Python的字符串
搞清楚了字符编码问题后，我们再来研究Python的字符串。

在最新版Python 3版本中，字符串是以Unicode编码的，也就是说，Python的字符串支持多语言，例如：
```python
>>> print('包含中文的str')
包含中文的str
```
对于单个字符的编码，Python提供了`ord()`函数获取字符的整数表示，`chr()`函数把编码转换成对应的字符：
```python
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'
```

如果知道字符的整数编码，还可以用十六进制这么写str：
```python
>>> '\u4e2d\u6587'
'中文'
```
两种写法完全等价。

由于Python的字符串类型是`str`，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把`str`变为以字节为单位的`bytes`。

Python对`bytes`类型的数据用`b`前缀表示：
```python
x=b'ABC'
```
注意区分`'ABC'`和`b'ABC'`，前者是str，后者虽然显示和前者一样，但bytes的每个字符都只占用一个字节。

以Unicode表示的str通过`encode()`方法可以编码为直径的bytes，例如：
```python
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
>>> '中文'.encode('ascii')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
```
纯英文的str可以用ASCII编码为bytes，内容一致，含有中文的str可以用UTF-8编码为bytes。含有中文的str无法用ASCII编码，因为中文编码范围超过了ASCII，Python会报错。

在bytes中，无法显示为ASCII自负的字节，用`\x##`显示。

反过来，如果我们从网络或磁盘上读取了字节流，那么读取到的数据就是bytes。腰包bytes变为str，需要`decode()`：
```python
>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
```

如果bytes中包含无法解码的字节，decode()会报错：
```python
>>> b'\xe4\xb8\xad\xff'.decode('utf-8')
Traceback (most recent call last):
  ...
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 3: invalid start byte
```

如果bytes中只有一小部分无效的字节，可以传入`errors='ignore'`忽略错误的字节：
```python
>>> b'\xe4\xb8\xad\xff'.decode('utf-8', errors='ignore')
'中'
```

要计算str包含多少个字符，可以用`len()`函数：
```python
>>> len('ABC')
3
>>> len('中文')
2
```

len()函数计算的是str的字符数，如果换成bytes，len()就计算字节数：
```python
>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('utf-8'))
6
```

可见，1个中文字符经过UTF-8编码后通常会占用3个字节，而1个英文字符只占用1个字节。

在操作字符串时，我们经常遇到str和bytes的互相转换。为了避免乱码问题，应当始终坚持使用UTF-8编码对str和bytes进行转换。

由于Python源代码也是一个文本文件，所以，当你的源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为UTF-8编码。当Python解释器读取源代码时，为了让它按UTF-8编码读取，我们通常在文件开头写上这两行：

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```
第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释；  
第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。  

申明了UTF-8编码并不意味着你的.py文件就是UTF-8编码的，必须并且要确保文本编辑器正在使用UTF-8 without BOM编码：

![](https://res.zhangkai.xin/pic/Python04-04.png)

如果.py文件本身使用UTF-8编码，并且也申明了`# -*- coding: utf-8 -*-`，打开命令提示符测试就可以正常显示中文：

![](https://res.zhangkai.xin/pic/Python04-05.png)

## 格式化
最后一个常见的问题是如何输出格式化的字符串。我们经常会输出类似`'亲爱的xxx你好！你xx月的话费是xx，余额是xx'`之类的字符串，而xxx的内容都是根据变量变化的，所以，需要一种简便的格式化字符串的方式。

在Python中，采用的格式化方式和C语言是一致的，用`%`实现，举例如下：
```python
>>> 'Hello, %s!'%'world'
'Hello, world!'
>>> 'Hi, %s, you have $%d.'%('Michael',1000000)
'Hi, Michael, you have $1000000.'
```

你可能猜到了，`%`运算符就是用来格式化字符串的。在字符串内部，`%s`表示用字符串替换，`%d`表示用整数替换，有几个`%?`占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个`%?`，括号可以省略。

| 占位符 | 替换内容 |
| :----------- | :----------- |
| %d | 整数 |
| %f | 浮点数 |
| %s | 字符串 |
| %x | 十六进制整数 |

其中，格式化整数和浮点数还可以指定是否补0和整数与小数的位数：
```python
>>> print('%2d-%02d'%(3,1))
 3-01
>>> print('%.2f'%3.1415926)
3.14
```

如果你不太确定应该用什么，`%s`永远起作用，它会把任何数据类型转换为字符串：
```python
>>> 'Age: %s. Gender: %s'%(25,True)
'Age: 25. Gender: True'
```

有些时候，字符串里面的`%`是一个普通字符怎么办？这个时候就需要转义，用`%%`来表示一个`%`：
```python
>>> 'growth rate: %d %%'%7
'growth rate: 7 %'
```

## format()
另一种格式化字符串的方法是使用`format()`，它会用传入的参数一次替换字符串中的占位符`{0}`、`{1}`……，不过这明显比%麻烦得多：
```python
>>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明',17.125)
'Hello, 小明, 成绩提升了 17.1%'
```

# 小结
Python 3的字符串使用Unicode，直接支持多语言。

当str和bytes互相转换时，需要指定编码。最常用的编码是UTF-8。Python当然也支持其他编码方式，比如把Unicode编码成GB2312：
```python
>>> '中文'.encode('gb2312')
b'\xd6\xd0\xce\xc4'
```
但这种方式纯属自找麻烦，如果没有特殊业务要求，请牢记仅使用UTF-8编码。

格式化字符串的时候，可以用Python的交互式环境测试，方便快捷。

------------

# 提前尝试

复习知识点并尝试练习此题目：
> T04-01: 小明的成绩从去年的72分提升到了今年的85分，请计算小明成绩提升的百分点，并用字符串格式化显示出`'xx.x%'`，只保留小数点后1位：
> 
> 提示：
> ```python
# -*- coding: utf-8 -*-
s1 = 72
s2 = 85
r = ???
print('???' % r)
```

请执行此代码
```
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