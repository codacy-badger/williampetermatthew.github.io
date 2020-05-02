---
title: 【转载】轻量级编辑器透彻指南--Notepad++
date: 2019-01-06 21:26:22		
tags: 
  - 洛谷日报
---


------------
# 轻量级编辑器透彻指南--Notepad++
------------

> 声明：本文为转载文章，转载自[洛谷日报#81](https://www.luogu.org/blog/user13091/ghj1222-likes-npp)，原作者为ghj1222。

------------
Notepad++是Windows环境下的一款编辑器。比起VSCode等现代编辑器，Notepad++同样具备很多功能。Notepad++一个特点就是轻巧，方便在Windows环境中使用，且编辑功能强大。本文主要介绍Notepad++的配置过 程和一些使用技巧。

Notepad++的开发者是Don Hu(侯今吾)中国台湾人（反正是个巨佬就对了嘛）

~~切入正题前先扯淡~~

> 我使用Noteapd++的理由：
> 
> 为什么不用Dev-C++，大家不知道Dev-C++打中文注释时候，他自动给你往后面塞了一个空格吗，不知道Dev-C++经常会崩溃吗，不知道Dev-C++在不断撤销时候经常出锅吗，相比之下Notepad++更稳定
> 
> 为什么不用VSCode，~~我感觉VSCode太高大上了，本蒟蒻用不起~~懒得配置那么多
> 
> 在Windows下，我感觉Notepad++是一个很棒的软件
> 
> 所以我选择了Notepad++

下面是正题

#### 一、安装
安装：打开[Notepad++官网](https://notepad-plus-plus.org/)。进入[下载页面](https://notepad-plus-plus.org/download/v7.5.8.html)。如果官网上不去，大家可以使用搜索引擎的快照功能。

为了方便这里直接给出官方下载地址[32位下载地址](https://notepad-plus-plus.org/repository/7.x/7.5.8/npp.7.5.8.Installer.exe)。[64位下载地址](https://notepad-plus-plus.org/repository/7.x/7.5.8/npp.7.5.8.Installer.x64.exe)。（Notepad++ 7.5.8的安装包，如有更新请在下载页面查看）

Notepad++已经在10.15更新7.5.9版本，（笔者所在学校机房网太卡上不去Notepad++官网了，有需要可以自行下载）~~我会告诉你Notepad++的各个版本区别不大吗~~

由于本文初稿写于九月，以下均为7.5.8版本（和最新版没啥区别）

下载下来安装包之后就是安装啦，相信大家都会的

下载后的安装包只有不到5MiB，比起VSCode精简多了(安装后也只有10MiB多一点)

打开安装包

![](https://i.loli.net/2018/09/24/5ba89328eb574.png)

然后OK，下一步，注意这里有一个Localization没有选择，这个Localization指的是多国语言，选不选随便。

![](https://i.loli.net/2018/09/24/5ba8932932144.png)

如果你要把Notepad++安装到可移动存储设备（说白了就是U盘，移动），要选上`Don' t use %APPDATA%`。选择这个选项框后，Notepad++会把所有配置文件都和主程序放在一起。

第二个不用管。第三个是桌面图标大家都懂的

![](https://i.loli.net/2018/09/24/5ba893294ae36.png)

完结撒花！

![](https://i.loli.net/2018/09/24/5ba8932986c90.png)

#### 二、C++ 开发环境配置
和别的多功能编辑器一样，Notepad++也可以编辑多种文件，支持多种语言的代码高亮。然鹅我们是要用Notepad++写程序，所以要配置一下开发环境，比如编译器~~(透彻器)~~什么的

#### 1.配置Notepad++
打开Notepad++，会发现默认是Courier New字体，而Windows较高版本代码字体选择Consolas字体会更漂亮~~(透彻)~~。选择菜单 \rightarrow→ 设置 \rightarrow→ 语言格式设置，把字体名称改为Consolas（如图），再把下面的“使用全局字体”选择上即可把字体设置为Consolas。相似地，在这里可以调整任意语言的代码高亮格式。在这里可以调整各种字体设置。

![](https://i.loli.net/2018/09/24/5ba89329627aa.png)

我们再打开设置 \rightarrow→ 首选项，里面有一些常用选项。其中建议把“新建”中默认语言改为C++。“新建”中编码可以改为GB2312，这样我们新建的文件的文字编码就是GB2312了。（由于Dev-C++坑爹，只识别GB2312编码，如果编码设为UTF-8，中文在Dev-C++上无法正常显示，而且程序编译时候需要开启某开关否则无法正常显示）

#### 2.安装编译器
(已安装MinGW并配置path的同学可以跳过本步骤)

首先是下载编译器。如果我们已经安装了Dev-C++这类自带编译器的IDE，下载编译器这一步可以跳过。作为OIer，在Windows平台上我们一般使用MinGW编译器，可以在[这里](http://www.mingw.org/)获得，64位平台建议使用MinGW-w64，官网在[这里](http://www.mingw-w64.org/doku.php)。下载后选择gcc系列安装就行了。

然后需要配置下path，这样就可以直接在command prompt中输入g++运行了。Windows 7的环境变量配置很坑爹，相比之下Windows 10的环境变量配置比较便捷，但是大体步骤还是一样的，这里分别介绍一下。(Windows XP可以自己百度一下)

我们先找到g++的路径(一般是在编译器安装路径\bin里，如果是Dev-C++，就在Dev-C++安装路径内的MinGW\bin里)，把绝对路径复制到剪贴板。

> Windows 7:
> 
> 桌面/开始菜单 $\rightarrow$ 计算机上右键 $\rightarrow$ 属性 $\rightarrow$ 高级系统设置 $\rightarrow$ 环境变量 $\rightarrow$ 系统变量里的变量Path，编辑系统变量，在后面追加一个刚才复制的绝对路径。注意要加上分号分割。这里面变量值可能有点多，可以先复制到Notepad++里再编辑以防止编辑错误(如果编辑错误就GG了，这里要谨慎小心)
> 
> Windows 10:
> 
> Windows 10的配置方式其实和Windows 7差不太多，只是更方便了。打开桌面/开始菜单 $\rightarrow$ 计算机上右键 $\rightarrow$ 属性 $\rightarrow$ 高级系统设置 $\rightarrow$ 环境变量 $\rightarrow$ 系统变量里的变量PATH，这里的配置是按照一个路径一个路径地列出来了，而不是全挤到一起了，更直观，也就更不容易出错

这里给出一张Windows7和一张Windows10的图。注意是编辑变量`Path`，而不是新建一个变量

![](https://i.loli.net/2018/09/24/5ba893297fbc3.png)

![](https://i.loli.net/2018/09/24/5ba8932960510.png)

这样我们的g++的环境变量就配置好了。按`Win+R`输入`cmd`调出命令提示符，输入g++，如果提示`g++: fatal error: no input files compilation terminated.`，那么我们的安装g++并配置path就成功了。如果你看到`g++ 不是内部或外部.....`的东西那么g++就没有成功安装或配置环境变量。

#### 3.安装NppExec
NppExec是可以直接在Notepad++编辑器里运行命令和程序的插件。我们可以利用Nppexec可以调用g++编译程序，执行程序。在[SOURCEFORGE](https://sourceforge.net/projects/npp-plugins/files/NppExec/NppExec%20Plugin%20v0.6%20RC2/)可以下载到NppExec的最新版(注意要对应 **Notepad++** 的位数下载，如果你的电脑是64位的但是安装了32位的Notepad++，应该下载32位的Nppexec)

下载之后解压。在Notepad++里选择设置 $\rightarrow$ 导入 $\rightarrow$ 导入插件，并选择解压后出来的DLL文件，这样NppEcec就成功地安装了。

注意要开启管理员模式安装插件，否则安装会失败。如果你的标题栏上有[Administrator]，那么说明你已经开启了管理员模式。

安装NppExec后，Notepad++的界面应该有一个控制台，但是它的默认字体十分难看，我们可以在插件 $\rightarrow$ NppExec $\rightarrow$ Change Console Font中调整字体。

#### 4.配置命令
安装完NppExec后即可配置命令。选择插件->NppExec->Excute，在里面新增加一条命令并命名为`C++ Compile`：

```
npp_save
g++ "$(FULL_CURRENT_PATH)" -g -Wall -std=c++98 -fexec-charset=GB2312 -o "$(CURRENT_DIRECTORY)\$(NAME_PART).exe"
```
这里解释一下这两条命令是什么意思。```npp_save```就是保存当前文件，为编译做准备。 ```$(FULL_CURRENT_PATH)``` 是当前打开文件的完整绝对路径， ```$ (CURRENT_DIRECTORY)``` 是当前打开文件所在文件夹的绝对路径，```$(NAME_PART)```是当前文件的名称部分(去掉后缀名)。```-g```是生成调试信息(如果不需要使用gdb调试可以关闭这个开关)，```-Wall```是产生全部警告信息，```-std=c++98```(这个某cjh大佬看着可能不适应)是指定语言标准，可以改为```c++11```等。最重要的是```-fexec-charset=GBK```，当你的文档格式是UTF-8且包含中文时**应该**开启此选项，当你的文档格式是GB2312且包含中文则**不应该**改期此选项，这个选项是用来解决编码问题的。当然大家可以自己写g++的编译命令，适应自己的需求。

然后再新建一条命令命名为`C++ Run`：
```
cmd /c (cd /d "$(CURRENT_DIRECTORY)" & start ConsolePauser "$(CURRENT_DIRECTORY)\$(NAME_PART).exe")
```
注意这里本来是可以使用下面的Console的，但是我在下面的Console调试程序的时候经常出锅，并且为了达到和Dev-C++类似的效果，这里借用了Dev-C++的ConsolePauser。Dev-C++的ConsolePauser就附在Dev-C++的安装目录下，为了使用我们把它复制到g++所在的目录中方便调用。如果你不想复制，你可以把上面的ConsolePauser改为绝对路径。

如果你喜欢用gdb调试，你也可以加一条`C++ Debug`，调用gdb，这里就省略了

![](https://i.loli.net/2018/09/24/5ba8932977b4b.png)

#### 5.配置热键
首先我们需要把刚才配置的命令放到热键区域：菜单栏 $\rightarrow$ 插件 $\rightarrow$ NppExec $\rightarrow$ Advanced Options...。在弹出窗口左下角的Associated script，选择我们刚才配置的C++ Compile，点下面的Add/Modify即可。由于某些奇怪的原因，你需要重新打开这个窗口来添加下一个命令。这样我们就可以在快捷键菜单中找到我们刚才配置的命令了。

打开菜单栏 $\rightarrow$ 宏 $\rightarrow$ 管理快捷键菜单，选择插件命令，找到你刚才配置的两个命令---`C++ Compile`和`C++ Run`，点快捷键一栏即可为他们配置快捷键。此处可以随意配置，个人建议将`C++ Compile`配置为`F9`，`C++ Run`配置为`F10`，这符合Dev-C++的习惯(一些省份NOIP可以使用Windows，Windows上有Dev-C++)，还有是这两个按键不会引起热键冲突。如果你还想配置"编译运行"、"调试"等命令，配制方法和上面差不了多少，这里不再阐述。

![](https://i.loli.net/2018/09/24/5ba8948257c21.png)

#### 6.编写代码
此时`trl+N`新建一个文档，敲一发Hello World(for dalao:动态树)，保存(注意这里一定要写全`.cpp`，因为Notepad++一个坑爹的设置，如果不写文件名，它默认是`.h`，g++可编译，但是生成的不是exe格式)，编译运行。如果你的代码成功运行了，那么就配置成功了！以后就可以嗨皮地使用Notepad++写代码了！

![](https://i.loli.net/2018/09/24/5ba8932985a37.png)

#### 三、玩转Notepad++
作为一款轻量级编辑器，Notepad++还是有很多方便之处的。这里就xjb简单地介绍几个，大家有兴趣可以自己玩玩。~~(反正我知道你看到这里还是没有兴趣用的Emm)~~

#### 1.文字编码
你还在为"锟斤拷"之类的乱码发愁吗？有了Notepad++，你再也看不到这种乱码了。Notepad++会在右下角现实当前文本编码。Notepad++的默认打开为UTF-8(你可以在首选项里更改默认编码)，在菜单栏的”编码“中，你可以更改打开编码(为了看到奇怪的乱码？)，或者是重新编码。如果你选择了UTF-8，而UTF-8无法识别，Notepad++会以16进制的字符保留在原文中，而不会替换为`EFBFBD`，也就是我们熟知的锟斤拷乱码。

> 怕有些同学不了解，这里xjb补充一点文字编码的知识(你就当做扯淡就行了)。在中国有两种通用的汉字编码：GB2312和UTF-8，前者是中国的国标，一个中文字符占2字节，后者是国际上的标准，大部分中文字符占3字节。如果直接用一种编码打开另一种文件，会造成乱码的问题。用GB2312编码打开UTF-8文件，会导致出现“文言文”现象；用UTF-8打开GB2312文件，会出现一堆"问号"。此时如果你保存文件，再用原来的GB2312编码打开文件，这时候不会出现原来的正常文字，而是锟斤拷锟斤拷，因为UTF-8识别文字时候，要识别连续1的数量，如果文字编码不符合UTF-8的规定，UTF-8会统一替换成`EFBFBD`，2个`EFBFBD`合并在一起，用GB2312编码打开，即为锟--`EFBF`；斤--`BDEF`；拷--`BFBD`。

#### 2.行尾序列
当你从某谷下载测试数据后，你想知道哪里出了锅，但是总是“本机AC，提交??E"，这时候如果你用了字符读入处理方式，你要检查是否是数据的行尾序列出了问题。Notepad++会在右下角显示行尾序列，双击就可以更改行尾序列。麻麻再也不用担心Windows和Linux下的换行问题啦。

#### 3.显示所有字符
在普通模式下，我们眼看肯定是看不见缩进使用了Tab还是空格，行尾序列是CR LF还是LF。Notepad++提供了显示所有字符的选项，他就位于工具栏的"¶"符号。点一下它，文本编辑区域瞬间杂乱无章就显示出了所有的空字符，回车，空格，制表符都尽收眼底。众所周知的是某谷的代码中所有的制表符都会被替换为空格。如果我们想把所有4个空格再换成制表符，(这里其实可以用AStyle)只需要Ctrl+F，找到替换，在上面输入4个空格，在下面打一个制表符，点确定，所有四个空格就被统一格式化为制表符了。

![](https://i.loli.net/2018/09/24/5ba893297f697.png)

#### 4.测试数据文件
我们在调试代码时候，有时候会从某谷上下载测试数据文件，但是有时候一下载就是几十M的样例输入。如果用Windows的自带的记事本打开，记事本肯定会崩掉，而我们可以用Notepad++打开，Notepad++用了一些玄学的处理方法，即使打开几十M，甚至几百M的文本文件都能快速，安全地打开。我们只需要右击文件，选择`Edit with Notepad++`。同时，我们可以在设置 $\rightarrow$ 首选项 $\rightarrow$ 文件关联中关联`.in`和`.out`文件，选择`customize`，将`.in`和`.out`加入注册的扩展名，这样测试数据直接双击就可以打开。

#### 5.宏
在Notepad++中，如果你需要对一段文本/多个文件进行同样的操作(而且还是跨行的，还要删除)，你不用每次都Ctrl+C，Ctrl+V，delete，你只需要做一次这个事情，并录制一个宏，在其它文件里播放宏，这些操作即可自动完成。我们也可以把宏设置为缺省源，并设置快捷键，每次就不用再打代码框架了。

#### 6.插件
除了NppExec外，还有很多优秀的插件可以使用，比如现在最新版的Notepad++配置的插件`Converter`，可以将ASCII和16进制互转，在Conversion Panel内可以显示ASCII代表的字符和对应的10/16/2/8进制。另外在菜单栏编辑 $\rightarrow$ 字符面板里也可以查看ASCII对应的字符。更多的插件，例如NppAstyle大家可以自己搜一搜。如果你有兴趣，你也可以自己开发插件，这里不在阐述

#### 7.自定义语言
在Notepad++里，你可以自定义你的代码高亮风格，你甚至可以自定义语言。例如Notepad++内没有内置markdown的高亮，我们可以自己定义一个markdown语言。在菜单栏的语言 $\rightarrow$ 自定义语言格式中，可以自定义语言。自定义markdown的步骤留给大家自己透彻，这里不在阐述

#### 8.其它编辑操作
在菜单 $\rightarrow$ 编辑里隐藏着我们不知道的一些编辑操作，这里有各种超神级编辑操作，你甚至可以对整数进行排序！

#### 9.彩蛋！
上面八个不看，这个也要看看吧。Notepad++里有一个小彩蛋。在编辑区输入文本`random`并选中，按F1，会出现新窗口随机的一段文字。这些文字可以在github的源码中找到。另外，点菜单栏问号下的命令行参数，其中有一个ghost typing，也就是自动打字，也有这个效果，大家可以在命令行测试一下。具体大家可以看命令行帮助。

#### 10.更改主题/自己编写主题
在Notepad++中可以更改主题文件或自己编写主题文件。Notepad++自带了多种主题（可以在设置->语言格式设置中找到），在这里也可以更改主题，我们在这里还可以自定义主题。

如何导入定义的主题呢？

例如@[szy1234](https://www.luogu.org/space/show?uid=30214)自制的高仿dev的主题： [https://pan.baidu.com/s/1oN04lCB1ziboWiZUjsL40Q](https://pan.baidu.com/s/1oN04lCB1ziboWiZUjsL40Q)

下载后把它放到Notepad++的安装目录下即可，一般应该是`C:\Program Files\Notepad++`，然后重启notepad++，在设置->语言格式设置里选择stylers主题就行了。

如果我们要自定义主题，我们可以直接打开xml文件，修改里面的颜色信息就行了~~我会告诉你我把背景调成了#66CCFF了吗~~

以上是Notepad++的基本配置方法和一些乱搞方法，感谢阅读

Notepad++作为一款轻量级编辑器，虽然功能上没有VSCode、Sublime等现代编辑器强大，但是它的优点我觉得就是轻巧、方便使用。~~(总结全文)~~

附：[Notepad++源码仓库地址](https://github.com/notepad-plus-plus/notepad-plus-plus)；大家可以浏览一下`CONTRIBUTING.md`，文件里面的一些代码习惯值得参考借鉴

鸣谢：Notepad++的开发者; [sm.ms](https://sm.ms/)的图床; 机房的Windows7和宿舍的Windows10系统上的截图 @[顾z](https://www.luogu.org/space/show?uid=87960)审稿