---
title: NOI Linux使用心得
date: 2019-01-06 17:02:29
tags: 
  - 科技
---
我使用的版本是NOI Linux 1.4.1

$\huge\color{red}{Warning}$ 请不要尝试在上个世纪的电脑上执行以下操作，这可能会导致严重的“Kernel panic - not syncing: Attempted to kill init!”错误  
$\huge\color{blue}{Attention}$ 请在执行下述操作时确认自己为根目录管理员（root）或拥有根目录管理权限

# 安装
安装过程比较苟，给出[官方说明文档](https://sys-code.oss-cn-beijing.aliyuncs.com/NOI/noilinux%E5%AE%89%E8%A3%85%E8%AF%B4%E6%98%8E%E6%96%87%E6%A1%A3.docx)和几个可参考的博客地址：
- [【转载】练习Linux？其实你的Win10自带一个Ubuntu](https://www.luogu.org/blog/Peter-Matthew/Run-Ubuntu-On-Windows10)
- [【转载】真正的全真虚拟机：VmWare Workstation](https://www.luogu.org/blog/Peter-Matthew/zhen-zheng-di-quan-zhen-xu-ni-ji-vmware-workstation)
- [【转载】在你的Android手机上运行Linux](https://www.luogu.org/blog/Peter-Matthew/play-linux-on-your-android-phone)
- [【转载】运行linux的第四种方式：双系统](https://www.luogu.org/blog/Peter-Matthew/multi-os-install-ubuntu)

# 使用
个人比较喜欢一些小配置，为了方便我很快地完成所有设置，在这里罗列。。。
## 密码
NOI Linux的默认密码是123456，我们修改密码却要求要有一定的强度，所以我们打开终端，输入
```
sudo -s
```
得到了root权限。  
然后输入123456（即原密码）。  
现在我们输入
```
passwd noilinux
```
输入新密码就好了，就比如我的密码就是六位纯数字，
```
******（我又不傻）
```
若修改成功，则会返回
```
password updated successfully
```

## 外观
桌面主题选的是Radiance，壁纸选的是Foggy Forest (2560 x 1709)  
应用程序->系统工具->首选项->主菜单，把所有勾点上
## 字体
我主要安装的字体是 YaHei Consolas Hybrid

如何安装字体？  
请在执行以下操作时确定自己为根目录管理员（root），也可以通过终端完成操作。
1. 在/usr/share/fonts/内创建一个文件夹
2. 在文件夹内放入字体文件（最好是.ttf格式的文件，其他格式的我还没测试）
3. 在终端执行下面两行命令
```
mkfontscale
mkfontdir
```

如何检验字体是否安装？  

请在终端执行：  

查看系统中的字体  
```
fc-list
```
查看系统中的中文字体  
```
fc-list :lang=zh
```
## FireFox
事实上NOI Linux 1.4.1自带的FireFox已经登不上现在的洛谷~~3.5~~了，所以我们需要手动更新它。
请在终端执行
```
sudo apt-get update
sudo apt-get install firefox
```
然后我们就可以愉快地上洛谷~~3.5~~了  
还有字体也可以在浏览器的首选项里更改了。
## Gedit
Gedit又称文本编辑器~~，类似于Windows的记事本~~。  
这是我认为比较好用的编辑器，其主要原因是配置好后与Dev C++比较相似。~~根本原因是Vim上手太难。~~  
让我们开始配置ing  

编辑->首选项  
- 查看：行号和高亮的两个打开
- 编辑器：制表符宽度4、启动自动缩进
- 字体和颜色：配色方案选择Oblivion，取消使用系统等宽字体而使用YCH的Bold 21号
- 插件：重点，点开外部工具和片段两个插件

在外部工具点开后，在 工具->Manage External Tools里，新建几个工具，命名和右侧选择的快捷键和代码内容如下：    
Compile(Alt+F9)
```
#!/bin/sh
fullname=$GEDIT_CURRENT_DOCUMENT_NAME
name=`echo $fullname | cut -d. -f1`
    g++ $fullname -o $name -g
```
Run(Alt+F10)
```
#!/bin/sh
fullname=$GEDIT_CURRENT_DOCUMENT_NAME
dir=$GEDIT_CURRENT_DOCUMENT_DIR
name=`echo $fullname | cut -d. -f1`
    gnome-terminal -x bash -c "time '$dir/$name';echo;echo 'Press ENTER to continue';read"
```
Compile And Run(Alt+F11)
```
#!/bin/sh
fullname=$GEDIT_CURRENT_DOCUMENT_NAME
dir=$GEDIT_CURRENT_DOCUMENT_DIR
name=`echo $fullname | cut -d. -f1`
    g++ $fullname -o $name -g
    gnome-terminal -x bash -c "time '$dir/$name';echo;echo 'Press ENTER to continue';read"
```
Debug(Alt+F5)
```
#!/bin/sh
fullname=$GEDIT_CURRENT_DOCUMENT_NAME
dir=$GEDIT_CURRENT_DOCUMENT_DIR
name=`echo $fullname | cut -d. -f1`
    gnome-terminal -x gdb "$dir/$name"
```
这四条的代码都从All Languages改为C++，然后将Compile和Compile And Run的工具里将保存从 无 改为 当前文档。

大功告成，我们可以写一份代码保存到桌面（注意Gedit不会自动添加扩展名），然后执行快捷键体验了。

额，没有括号补全？  
首先我们来安装所有的插件，在终端执行
```
sudo apt-get install gedit-plugins
```
然后我们在插件里找到括号补全，点开就好了。还有个Code Comment也可以启用，它可以在编辑菜单里看到。
## Arbiter
又称为考试评测系统Primer，是北航联合CCF推出的一款官方评测系统，其系统bug极多，效果极差，稍有操作不慎将从头来过，被我机房所独骂。

虽说官方已经发了[介绍](http://www.noi.cn/RequireFile.do?fid=6tj9d3hF)不过我们确实需要说一说这个。

我们NOI Linux上安装的一般都是单机版~~（我也没有网络版）~~

**请注意下面的每一步最好都保存以免意外重来。**

新建一个文件夹，然后在Arbiter里新建比赛，目录选择那个文件夹。目录里就会出现这些东西。
![](https://res.zhangkai.xin/pic/NLA1.png)
```
└Exam				<-评测根目录
 ├data				<-数据目录
 │└[无文件]
 ├evaldata			<-整合数据目录（数据格式必须为XXxx.in/.ans，XX为字母，xx为数字）
 │└[无文件]
 ├filter			<-评测比较器
 │├fulltext
 │├fulltext_e
 │├fulltext_e.c
 │└...
 ├final				<-提供存储最终成绩的目录
 │└[无文件]
 ├players			<-选手程序目录
 │└[无文件]
 ├result			<-成绩目录
 │└[无文件]
 ├tmp				<-评测临时目录
 │└[无文件]
 └setup.cfg			<-评测设置文件
```
下面我们开始配置
我们有两种配置方式：一种是多场次共文件夹的（情况A）、一种是单场次单文件夹的（情况B）。  
我们假设Day1有三道题A、B、C，Day2有三道题D、E、F。
### 多场次共文件夹
```
└Exam				<-评测根目录
 ├data				<-数据目录
 │├A
 ││├A1.in
 ││├A1.ans
 ││├A2.in
 ││├A2.ans
 ││└...
 │├B
 ││├B1.in
 ││├B1.ans
 ││├B2.in
 ││├B2.ans
 ││└...
 │├C
 ││├C1.in
 ││├C1.ans
 ││├C2.in
 ││├C2.ans
 ││└...
 │├D
 ││├D1.in
 ││├D1.ans
 ││├D2.in
 ││├D2.ans
 ││└...
 │├E
 ││├E1.in
 ││├E1.ans
 ││├E2.in
 ││├E2.ans
 ││└...
 │└F
 │ ├F1.in
 │ ├F1.ans
 │ ├F2.in
 │ ├F2.ans
 │ └...
 ├evaldata			<-整合数据目录（数据格式必须为XXxx.in/.ans，XX为字母，xx为数字）
 │├A1.in
 │├A1.ans
 │├A2.in
 │├A2.ans
 │├...
 │├B1.in
 │├B1.ans
 │├B2.in
 │├B2.ans
 │├...
 │├C1.in
 │├C1.ans
 │├C2.in
 │├C2.ans
 │├...
 │├D1.in
 │├D1.ans
 │├D2.in
 │├D2.ans
 │├...
 │├E1.in
 │├E1.ans
 │├E2.in
 │├E2.ans
 │├...
 │├F1.in
 │├F1.ans
 │├F2.in
 │├F2.ans
 │└...
 ├filter			<-评测比较器
 │├fulltext
 │├fulltext_e
 │├fulltext_e.c
 │└...
 ├final				<-提供存储最终成绩的目录
 │└[自己放的东西]
 ├players			<-选手程序目录
 │├HA-0001
 ││├A
 │││└A.cpp
 ││├B
 │││└B.cpp
 ││├C
 │││└C.cpp
 ││├D
 │││└D.cpp
 ││├E
 │││└E.cpp
 ││└F
 ││ └F.cpp
 │├HA-0002
 ││├A
 │││└A.cpp
 ││├B
 │││└B.cpp
 ││├C
 │││└C.cpp
 ││├D
 │││└D.cpp
 ││├E
 │││└E.cpp
 ││└F
 ││ └F.cpp
 │└...
 ├ps				<-成绩打印表
 │├day1
 ││├HA-0001
 │││└HA-0001.ps
 ││├HA-0002
 │││└HA-0002.ps
 ││└...
 │└day2
 │ ├HA-0001
 │ │└HA-0001.ps
 │ ├HA-0002
 │ │└HA-0002.ps
 │ └...
 ├result			<-成绩目录
 │├day1
 ││├HA-0001
 │││└HA-0001.result
 ││├HA-0002
 │││└HA-0002.result
 ││└...
 │└day2
 │ ├HA-0001
 │ │└HA-0001.result
 │ ├HA-0002
 │ │└HA-0002.result
 │ └...
 ├tmp				<-评测临时目录
 │└[系统产生的文件]
 ├day1.info
 ├day2.info
 ├player.info
 ├setup.cfg			<-评测设置文件
 ├task1_1.info
 ├task1_2.info
 ├task1_3.info
 ├task2_1.info
 ├task2_2.info
 ├task2_3.info
 └team.info
```
我们在data文件夹下放数据，怎么放都行，貌似这个文件夹不会影响测评  
![](https://res.zhangkai.xin/pic/NLA4.png)

evaldata下要放所有数据，必须在文件夹下放.in/.ans，不能缺少，而且输出文件只能为.ans，否则**Arbiter评测时就会闪退**

接着在players里放下选手的文件夹，在每个选手的文件夹里题应该独立建子文件夹，并将程序放在子文件夹下，如上树形图所示

我们返回Arbiter，在试题管理选项卡中右键-添加考试，在第一场上添加三个试题，更改试题名称、（试题分值）、测试点数量、时间限制、内存限制和比较方式（我不知道spj怎么设置），如果有需要还可以设置编译选项。

![](https://res.zhangkai.xin/pic/NLA5.png)

接着我们转到试题评测选项卡，点击右侧添加选手，添加完所有选手。然后导出名单，可以保存成任意格式，北航官方说保存成.txt~~其实明眼人一看就是.csv的文件~~，但是无所谓。这个名单方便下次测评可以直接导入名单而不用一一添加选手名字。

添加好后，更改左上角的场次，勾选要测评的选手然后点击评测选定选手，等待评
测。

![](https://res.zhangkai.xin/pic/NLA6.png)

双击选手可查看其程序，若提示**“找不到答案文件”**，说明放置的选手程序有误或是没有，反正系统找不到源程序。  

![](https://res.zhangkai.xin/pic/NLA7.png)

评测完后，可以查看相应的成绩，在result文件夹下对应场次中找到选手，打开.result文件即可查看对应的单测试点时间、成绩及信息。

![](https://res.zhangkai.xin/pic/NLA8.png)

最后还有成绩统计选项卡，这个就随意了，你也可以导出成绩到文件，如果打印到文件会多出一个ps文件夹用于存储打印表。

![](https://res.zhangkai.xin/pic/NLA9.png)

### 单场次单文件夹
```
└Exam				<-评测根目录
 ├data				<-数据目录
 │├day1
 ││├A
 │││├A1.in
 │││├A1.ans
 │││├A2.in
 │││├A2.ans
 │││└...
 ││├B
 │││├B1.in
 │││├B1.ans
 │││├B2.in
 │││├B2.ans
 │││└...
 ││└C
 ││ ├C1.in
 ││ ├C1.ans
 ││ ├C2.in
 ││ ├C2.ans
 ││ └...
 │└day2
 │ ├D
 │ │├D1.in
 │ │├D1.ans
 │ │├D2.in
 │ │├D2.ans
 │ │└...
 │ ├E
 │ │├E1.in
 │ │├E1.ans
 │ │├E2.in
 │ │├E2.ans
 │ │└...
 │ └F
 │  ├F1.in
 │  ├F1.ans
 │  ├F2.in
 │  ├F2.ans
 │  └...
 ├evaldata			<-整合数据目录（数据格式必须为XXxx.in/.ans，XX为字母，xx为数字）
 │├A1.in
 │├A1.ans
 │├A2.in
 │├A2.ans
 │├...
 │├B1.in
 │├B1.ans
 │├B2.in
 │├B2.ans
 │├...
 │├C1.in
 │├C1.ans
 │├C2.in
 │├C2.ans
 │├...
 │├D1.in
 │├D1.ans
 │├D2.in
 │├D2.ans
 │├...
 │├E1.in
 │├E1.ans
 │├E2.in
 │├E2.ans
 │├...
 │├F1.in
 │├F1.ans
 │├F2.in
 │├F2.ans
 │└...
 ├filter			<-评测比较器
 │├fulltext
 │├fulltext_e
 │├fulltext_e.c
 │└...
 ├final				<-提供存储最终成绩的目录
 │└[自己放的东西]
 ├players			<-选手程序目录 *请注意：有时候由于Arbiter的bug，无法识别某个场次的文件夹里的选手程序，此时请将此场次里的文件复制到别的场次里
 │├day1
 ││├HA-0001
 │││├A
 ││││└A.cpp
 │││├B
 ││││└B.cpp
 │││└C
 │││ └C.cpp
 ││├HA-0002
 │││├A
 ││││└A.cpp
 │││├B
 ││││└B.cpp
 │││└C
 │││ └C.cpp
 ││└...
 │└day2
 │ ├HA-0001
 │ │├D
 │ ││└D.cpp
 │ │├E
 │ ││└E.cpp
 │ │└F
 │ │ └F.cpp
 │ ├HA-0002
 │ │├D
 │ ││└D.cpp
 │ │├E
 │ ││└E.cpp
 │ │└F
 │ │ └F.cpp
 │ └...
 ├ps				<-成绩打印表
 │├day1
 ││├HA-0001
 │││└HA-0001.ps
 ││├HA-0002
 │││└HA-0002.ps
 ││└...
 │└day2
 │ ├HA-0001
 │ │└HA-0001.ps
 │ ├HA-0002
 │ │└HA-0002.ps
 │ └...
 ├result			<-成绩目录
 │├day1
 ││├HA-0001
 │││└HA-0001.result
 ││├HA-0002
 │││└HA-0002.result
 ││└...
 │└day2
 │ ├HA-0001
 │ │└HA-0001.result
 │ ├HA-0002
 │ │└HA-0002.result
 │ └...
 ├tmp				<-评测临时目录
 │└[系统产生的文件]
 ├day1.info
 ├day2.info
 ├player.info
 ├setup.cfg			<-评测设置文件
 ├task1_1.info
 ├task1_2.info
 ├task1_3.info
 ├task2_1.info
 ├task2_2.info
 ├task2_3.info
 └team.info
```
还是老话，data文件夹下怎么放都行，所以这里唯一变化的是players文件夹。
为什么推荐情况A的设置方法，原因很简单，Arbiter有个巨大的bug，就是按情况B这样设置Arbiter极容易找不到个别场次的程序，为了方便起见，我们用情况A的方法配置，但是这样也是可以配置的。
### 目录文件转换
考虑到有学校使用Windows下Lemon或Cena评测  
在这里提供几组bat文件方便老师使用。

data.bat  
用于提取evaldata文件夹下的数据并自动放在data文件夹里形成Lemon或Cena可用的data文件夹  
适用于情况B，需手动设置题目名称和场次。
```batch
@echo off
set a=A		::<-第一题
set b=B		::<-第二题
set c=C		::<-第三题
set d=day1	::<-场次
md data
cd data
md %d%
cd %d%
for /d %%i in (%a% %b% %c%) do (
   md %%i
   copy ..\..\evaldata\%%i*.* .\%%i
)
pause
```
data-noday.bat  
用于提取evaldata文件夹下的数据并自动放在data文件夹里形成Lemon或Cena可用的data文件夹  
适用于情况A，需手动设置题目名称。
```batch
@echo off
set a=A		::<-第一题
set b=B		::<-第二题
set c=C		::<-第三题
md data
cd data
for /d %%i in (%a% %b% %c%) do (
   md %%i
   copy ..\evaldata\%%i*.* .\%%i
)
pause
```
evaldata.bat  
用于提取data文件夹下的数据并自动放在evaldata文件夹里形成Arbiter可用的evaldata文件夹  
适用于情况B，需手动设置场次。
```batch
@echo off
md evaldata
cd data
cd day1		::<-场次
for /d %%i in (*) do copy .\%%i\*.* ..\..\evaldata
pause
```
evaldata-noday.bat  
用于提取data文件夹下的数据并自动放在evaldata文件夹里形成Arbiter可用的evaldata文件夹  
适用于情况A，无需设置任何内容。
```batch
@echo off
md evaldata
cd data
for /d %%i in (*) do copy .\%%i\*.* ..\evaldata
pause
```
players.bat  
用于提取source文件夹下的数据并自动放在players文件夹里形成Arbiter可用的players文件夹  
适用于情况B，需手动设置题目名称、场次和选手名称对应代号（也可直接让选手以代号提交）。
```batch
@echo off
set a=A		::<-第一题
set b=B		::<-第二题
set c=C		::<-第三题
set d=day1	::<-场次
md players
cd players
md %d%
cd..
xcopy source\* players\%d% /y /e /i /q
cd players
cd %d%
for /d %%i in (*) do (
   @echo %%i
   cd %%i
   for /d %%j in (%a% %b% %c%) do (
      @echo %%j
      md %%j
      copy .\%%j.cpp .\%%j\%%j.cpp
   )
   del /f /q .\*.*
   cd..
)
ren std HA-0001		::<-选手名称对应代号，若让选手以代号提交请删除
pause
```
players-noday.bat  
用于提取source文件夹下的数据并自动放在players文件夹里形成Arbiter可用的players文件夹  
适用于情况A，需手动设置题目名称和选手名称对应代号（也可直接让选手以代号提交）。
```batch
@echo off
set a=A		::<-第一题
set b=B		::<-第二题
set c=C		::<-第三题
md players
xcopy source\* players /y /e /i /q
cd players
for /d %%i in (*) do (
   @echo %%i
   cd %%i
   for /d %%j in (%a% %b% %c%) do (
      @echo %%j
      md %%j
      copy .\%%j.cpp .\%%j\%%j.cpp
   )
   del /f /q .\*.*
   cd..
)
ren std HA-0001		::<-选手名称对应代号，若让选手以代号提交请删除
pause
```
source.bat  
用于提取players文件夹下的数据并自动放在source文件夹里形成Lemon或Cena可用的source文件夹  
适用于情况B，需手动设置场次和选手名称对应代号（也可直接以选手代号评测）。
```batch
@echo off
md source
xcopy players\day1\* source /y /e /i /q		::<-day1即为场次
cd source
for /d %%i in (*) do (
   @echo %%i
   cd %%i
   for /d %%j in (*) do (
      @echo %%j
      copy .\%%j\%%j.cpp .\%%j.cpp
	  del /f /s /q .\%%j\*.*
      rd /q /s .\%%j\
   )
   cd..
)
ren HA-0001 std		::<-选手名称对应代号，若直接以选手代号评测请删除
pause
```
source-noday.bat  
用于提取players文件夹下的数据并自动放在source文件夹里形成Lemon或Cena可用的source文件夹  
适用于情况A，需手动设置选手名称对应代号（也可直接以选手代号评测）。
```batch
@echo off
md source
xcopy players\* source /y /e /i /q
cd source
for /d %%i in (*) do (
   @echo %%i
   cd %%i
   for /d %%j in (*) do (
      @echo %%j
      copy .\%%j\%%j.cpp .\%%j.cpp
	  del /f /s /q .\%%j\*.*
      rd /q /s .\%%j\
   )
   cd..
)
ren HA-0001 std		::<-选手名称对应代号，若直接以选手代号评测请删除
pause
```
使用方法是放在目录文件夹，然后更改bat内容并运行。

![](https://res.zhangkai.xin/pic/NLA2.png)  
![](https://res.zhangkai.xin/pic/NLA3.png)  

如果提示“存在一个重名文件，或是找不到文件”，这是因为
1. bat的内容设置写法有误~~（肯定是你们写错了）~~，请检查你填写的内容是否多空格少空格，如果目录名有空格，请加英文半角的双引号```"```
2. 已经有文件夹，例如我执行source-noday.bat之前文件夹里已经有std文件夹，那么就会这么提示，解决方法是手动替换或提前删除。。。

## GNU Emacs 24
其实并不会用这个软件，这个软件是用来学（tui）习（fei）用的，你可以看到菜单栏里的Tools->Games里有很多东西，(#^.^#)，可以自己试着体验体验这些东西。


------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。