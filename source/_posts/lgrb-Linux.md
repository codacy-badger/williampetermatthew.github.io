---
title: 【转载】OI中可以用到的Linux基础教程
date: 2018-08-27 09:25:38
tags: 
  - 洛谷日报
---


------------
# OI中可以用到的Linux基础教程
------------

> 声明：本文为转载文章，转载自[洛谷日报#29](https://www.luogu.org/blog/Chanis/Linux)，原作者为Chanis。

------------
## Linux基础教程

●前置系统：任意Linux(不用NOI Linux也没问题，我用的是deepin），如果没有装，而且你用的是win10，请看往期的洛谷日报：[练习Linux？其实你的Win10自带一个Ubuntu](https://www.luogu.org/blog/asfr/Run-Ubuntu-On-Windows10)。如果你用的不是win10，你可以装双系统或是虚拟机，这里直接给地址，不再赘述（mac os的操作类似linux，仅提供win教程）:[虚拟机](https://jingyan.baidu.com/article/86112f135e584a273697876b.html) [双系统](https://jingyan.baidu.com/article/60ccbceb18624464cab197ea.html) [NOI Linux下载地址](http://www.noi.cn/newsview.html?id=267&hash=BDBDBE&type=1)

●QQ826755370

### 引言

众所周知，NOIP复赛使用的操作系统将逐渐变为NOI Linux，湖南已经取消了windows机房，我所在的江苏今年似乎也要将win7+NOI Linux虚拟机换成NOI Linux，所以我们需要适应Linux系统，从而就有了这篇教程。

### Linux基本操作
![终端](https://s1.ax1x.com/2018/08/01/Pw42Yn.png)

1.打开终端：直接用快捷键Ctrl+Alt+T即可。

2.用root（类似windows的administrator）权限运行命令：在命令的最前面加上sudo，输入完命令后输入此时登录的账号的密码，注意，输入进去的密码是隐藏的，不会显示明文或“*”，不要以为是电脑坏了。

3.终端内打开自己用户的文件夹（类似windows里你的用户文件夹）：cd ~

4.终端里打开某个文件夹：cd 路径

5.终端里返回上一级文件夹：cd ..

6.~与/的区别：“~”放在路径开头表示的是自己的文件夹目录，“/”放在路径开头表示的是根目录，放在中间表示就和普通的“/”意义相同，如果路径的开头没有这两个符号，那么表示将从你当前所在的文件夹开始查找目标文件（夹）。

7.安装软件：

(1)debian系（如debian，ubuntu，NOI Linux(这玩意儿能叫一个系统？)，deepin，elementary OS):sudo apt-get install 软件名

(2)arch系（如arch Linux，manjaro）：sudo pacman -S 软件名

(3)Redhat系（如Redhat，centos）：sudo yum install 软件名

8.删除文件：rm -rf 路径（这个指令爽歪歪，手动滑稽：rm -rf /）

9.以上就是我最常用的，其他的都不常用啊...因为Linux本身也有图形界面，所以复制粘贴，新建文件（夹），重命名之类的操作就不讲了。我讲路径和cd是因为编译时要用到，讲安装软件那是必须的，讲rm是为了某个好玩的指令，如果觉得有什么需要添加的请指出。

### 使用Linux编写c++程序

IDE

(1)北航的鸡肋guide：这个NOI官网上有详细的教程[NOI的教程](http://download.noi.cn/T/noi/GUIDE_v1.1.pdf)，但是我不喜欢用这个弱智IDE。

(2)我认为比较好用的anjuta：

①打开之后先选择“create a new project”。

![anjuta1](https://cdn.luogu.org/upload/pic/26176.png)

②之后弹出一个窗口，上方选择“c++”，里面选“通用c++”，之后弹出的窗口全部点击“确定”或“应用”。

![anjuta2](https://cdn.luogu.org/upload/pic/26178.png)

③然后我们来到下图的页面，左侧按照我的目录树，打开main.cc，记得把拓展名改成“.cpp”，NOIP的c++源文件的拓展名必须是cpp。现在我们看到了hello,world的代码，直接在里面修改即可，写完之后按F9编译，F3运行。

![anjuta3](https://cdn.luogu.org/upload/pic/26179.png)

④调试的方法与dev-c++类似，上方有个“调试”菜单，这里不再讲，后面会讲终端中使用gdb调试。

(3)NOI Linux不自带的Geany、Code::Blocks等：因为不自带，考试用不了，所以我也不做使用讲解。

文本编辑器

无论你是用什么编辑器（vim除外），我都建议你在目录下新建一个cpp文件。

![新建](https://cdn.luogu.org/upload/pic/26182.png)

(1)gedit（类似windows系统的记事本，但是比记事本强大多了）

双击该文件，默认使用gedit打开的，写完代码后，你是不是发现贼丑？

![贼丑](https://cdn.luogu.org/upload/pic/26183.png)

别急，我们来改一下，右下角有一行字，我们将“纯文本”改为c++，制表符宽度按个人喜欢设置，里面还可以设置自动缩进，后面一个框里可以选择显示行号，高亮当前行，右上方的菜单键（三个点）里面可以设置侧边栏，还有搜索、跳转行的功能。

![美化后](https://cdn.luogu.org/upload/pic/26184.png)

是不是好看多了？退出之前别忘了点右上方的“保存”。

(2)emacs（业界有这样一句话：“emacs是神的编辑器，vim是编辑器的神。”）

①我们要用windows中右击选择打开方式的方法，使用emacs打开文件（你也可以设置默认打开方式），我们选择GUI版本，如果选传统版本的话，你还不如用vim。

②没什么想讲的了，直接放张图吧，与gedit相比，emacs支持代码补全，据说有许多强大的功能，但是我是vim选手，不是emacs选手，所以关于emacs的骚操作，请自行百度吧。

![emacs](https://cdn.luogu.org/upload/pic/26187.png)

(3)vim，我用的就是vim，但是vim的教程都能写十篇文章，这里我并不想写啊，而且当初我听信采取他人的蛊惑建议，学vim用了一个多星期...直接扔一个链接吧：[vim教程](https://www.jianshu.com/p/385cb0fdc3a0)。需要注意的是，NOIP不提供vim的插件，所以不能过度依赖vim的插件，但是可以改.vimrc，NOIP发题之前给你的时间已经足够你写.vimrc了。下面fa♂一张我的vim：

![myvim](https://cdn.luogu.org/upload/pic/26190.png)

(4)sublime等编辑器，这些NOI Linux不自带，还是不讲。

等一等，我们写完代码了，该怎么编译？

①先打开终端，在终端里打开你的源代码所在的目录。

②接着在终端里输：g++ 你的代码文件名.cpp -o 随便填（如果你用了cmath，那么你需要-lm，开优化你需要-O0，-O1，-O2，-O3，想用gdb调试要开-g0 -g1 -g2 -g3，建议使用-g2）。

③然后我们在终端里输入：./"随便填"（就是编译的时候你自己填的）。这样就亦可赛艇辣！

![编译运行](https://cdn.luogu.org/upload/pic/26195.png)

### 终端里使用gdb调试

(1)首先用终端在存放编译好的文件的目录下运行：gdb ./你的可执行文件名（编译时必须加了-g选项）

![gdb](https://cdn.luogu.org/upload/pic/26247.png)

(2)gdb命令小全

①设置断点：break（或b） 行号

②查看变量的值：p 变量名

③下一步（跳过函数）：next（或n）

④下一步（不会跳过函数）：step

⑤跳出当前函数：finish

⑥查看断点信息：info b

⑦继续运行：c

⑧开始运行：run

⑨删除某个断点：delete（或d） break 断点号（如果没有则删除所有的断点）

⑩删除某行断点：clear 行号

(3)退出gdb

输入quit，回车之后再输入y，这样就退出了。

(4)一点建议

不要过度依赖gdb，养成静态查错的习惯，NOIP考场上也有过gdb出锅的先例：[gdb出锅](http://tieba.baidu.com/p/4866797834)。

(5)[gdb详细教程](https://www.cnblogs.com/xsln/p/gdb_instructions1.html)

### 对拍

1.除了待测程序，你还需要自己写一个数据生成器和暴力程序，如果要得到一个小于n的数，可以用rand()%n得到。

2.你要写一个shell脚本，我们将其命名为judge.sh（其他的名字也可以，但是拓展名必须是.sh），将它与那三个程序放在同一个目录下，它的模板是下面这个样子的（#在shell里表示注释）：
```cpp
#!/bin/bash #相当于c++的头文件，背下来就对了
while true; do
./makedata>in.txt #数据生成器输出数据重定向到in.txt
./wait_judge<in.txt>out.txt #待测程序重定向输入输出
./check<in.txt>right.txt #正确（暴力）程序
if diff out.txt right.txt; then #比较两个文件
printf AC #正确输出AC
else
printf WA #错误输出WA
#cat out.txt right.txt #显示两个文件
exit 0 #退出
fi #结束if
done #结束while
```
3.我们打开终端，进入到保存对拍程序的目录，输入：sh ./judge.sh，然后回车即可。

![judge](https://cdn.luogu.org/upload/pic/26204.png)

4.需要注意的是，这三个程序的源代码里面都不需要重定向输入输出（加freopen之类的），但是拍完了别忘记加上。还有，如果对拍发现结果不一样，先检查你的暴力程序，防止是暴力程序写错了。

### 赠品——我该如何在Linux下颓废？

1.slay.one

2.linux版的网易云音乐了解一下，steam for linux了解一下。

3.上ab站使人身心愉悦。

4.wine下的TIM、QQ了解一下（强势安利一波deepin、完美wine模拟）。

### 完结撒花！★,°:.☆(￣▽￣)/$:.°★ 。