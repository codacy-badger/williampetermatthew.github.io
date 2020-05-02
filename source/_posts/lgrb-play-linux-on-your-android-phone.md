---
title: 【转载】在你的Android手机上运行Linux
date: 2019-01-06 21:31:57	
tags: 
  - 洛谷日报
---


------------
# 在你的Android手机上运行Linux
------------

> 声明：本文为转载文章，转载自[洛谷日报#94](https://www.luogu.org/blog/wendster/play-linux-on-your-android-phone)，原作者为wendster。

------------
既然日报里已经有两种“非正常”跑Linux的方法了，我再放一个貌似也不要紧......（吐槽一波为何日报上的VMware还是10的版本，太旧了吧）

之前试过许多方法（也就几种），像什么Complete Linux Installer，Debian noroot，利用已有的Linux构造Bootstrap之类，要么就是复杂得要命（比如Debian上的Bootstrap，调了半天没有调出来），要么就是鸡肋，直到发现了一款叫做Linux Deploy的软件。

![1.jpg](https://wendster.win/blog/usr/uploads/2018/10/2973309071.jpg)

这款软件是需要root才能运行的（作者的劝告：没有root的童鞋虽然可以使用Termux，但是Termux没有Linux Deploy好。Termux不同于Linux Deploy。Termux采用proot，无需root便可运行，但相比使用chroot的Linux Deploy，Termux运行速度慢，功能少，可定制度低，只有一种系统，所以还是root一下，使用Linux Deploy吧。），而且你需要安装好busybox。（官方是这么说的，不过没安装好像也没事，不过还是建议安装一下，反正容易得很）

看来这款软件的先置需求还是~~不多的~~，随随便便就能处理好是不是？

好吧，现在步入正题，开始安装Linux Deploy（欢迎花样作死）。

------------

## 1.安装Linux Deploy
虽然有些应用商店里有Linux Deploy，网上也可以下得到，但是版本估计都很旧，所以推荐到Google Play商店里下载。 为了顾及那些没有梯子的童鞋，作者**贴心**地提供了目前最新的Linux Deploy：https://wendster.win/pan/index.php?share/file&user=1&sid=Cd3ThYKg

就像安装微信一样安装好它就行了。

然后打开......是这个样子的：

![28.jpg](https://wendster.win/blog/usr/uploads/2018/10/899796367.jpg)

你看，上面都教你怎么安装了！（笑）

## 2.配置Linux Deploy
如果你想要安装多个系统，建议更改一下配置文件的名称。（哎呀，强迫症又犯了，你们忽略我吧）

先点击左上的“恒等于号”

![29.jpg](https://wendster.win/blog/usr/uploads/2018/10/3853134255.jpg)

点击配置文件

![30.jpg](https://wendster.win/blog/usr/uploads/2018/10/1755335854.jpg)

点一下那支笔

![31.jpg](https://wendster.win/blog/usr/uploads/2018/10/462216913.jpg)

既然这次我们要安装的是Ubuntu，那就写上Ubuntu好了......

更改好后点击确定

如果不想你的Linux运行时CPU休眠导致卡成龟，最好让其保持唤醒

先返回到这儿

![29.jpg](https://wendster.win/blog/usr/uploads/2018/10/3853134255.jpg)

点击设置

![32.jpg](https://wendster.win/blog/usr/uploads/2018/10/3173883859.jpg)

勾选“保持CPU唤醒”，顺便把“锁定Wi-Fi"也勾上吧，嘿嘿！

## 3.设置安装选项
在设置安装选项之前，需要看看手机的CPU架构和/data分区剩余空间

回到主页面，点击右上方的三个点

![58.jpg](https://wendster.win/blog/usr/uploads/2018/10/508697094.jpg)

点击状态

然后就会滚出来很多行字

![37.jpg](https://wendster.win/blog/usr/uploads/2018/10/2964048571.jpg)

![38.jpg](https://wendster.win/blog/usr/uploads/2018/10/2780299096.jpg)

比如，我的手机CPU是armv8l的，/data分区还剩15.9GB，这些信息都要记住，后面要用到

现在可以正式开始设置安装选项了，点击右下方的一个鬼畜的按钮

![33.jpg](https://wendster.win/blog/usr/uploads/2018/10/1946116582.jpg)

容器类型不用管，直接从发行版开始

由于这回我们要安装Ubuntu，那就选Ubuntu好了~~（废话）~~

![34.jpg](https://wendster.win/blog/usr/uploads/2018/10/2132807448.jpg)

接下来就是架构了，根据我们之前在状态里看到的CPU架构选择合适的架构

> 一般有armv8字样的CPU是64位的，选择arm64或aarch64（看哪个有选哪个，这里是arm64,那就选arm64），其他的选armhf即可（除非你的手机老到炸裂，不过太老的手机是装不上Linux Deploy的），如果有些神机是i386或者x86_64（amd64）的，直接照着选就可以了（表示膜拜）

比如我的手机是armv8l，就选arm64好了

![35.jpg](https://wendster.win/blog/usr/uploads/2018/10/3557802290.jpg)

至于发行版版本嘛，你自己选好了。由于我是更新狂，所以我选了最新的bionic（Ubuntu 18.04）（唉，强迫症又犯了，你们无视我吧 ^_^）

![39.jpg](https://wendster.win/blog/usr/uploads/2018/10/2584764460.jpg)

这四个版本代号分别对应着Ubuntu12.04，Ubuntu14.04，Ubuntu16.04和Ubuntu18.04（应该没人会去用Ubuntu12.04了吧）

现在到了设置源地址的时候了。由于官方的源在国外，所以你若不想调两天还调不好的话，还是不要用官方源吧。这里提供一个基本上是国内最快最全的源（中科大镜像站）：http://mirrors.ustc.edu.cn/ubuntu-ports

![40.jpg](https://wendster.win/blog/usr/uploads/2018/10/843037677.jpg)

嗯，现在可以选择安装类型了。如果你之前看到的/data分区大于等于4G，建议选择目录，这样大概是选择镜像文件的运行速度的两至六倍。假如你的手机/data分区真的没空间了，但是你有一张存储卡，可以选择镜像文件，或者分区。

不过需要注意的是，由于你的存储卡一般是fat32格式的，所以**无法存储大于4G**的文件，所以镜像文件的大小**不能超过4G**。若选择分区的话，就不会出现这种问题，但是选择分区的最大问题就是你的卡里的文件会**被清空**，且**无法在Windows上访问你的卡**，因为它需要被格式化成ext3或ext4格式（具体选ext3还是ext4，取决于你的手机是否支持ext4，可以通过状态查看，看那个Supported FS有没有ext4即可，若连ext3都没有，那就只能选ext2了，不过这种手机实在太古老了，我保证连Linux Deploy都装不上......）。

还有，如果你选择了分区，一定要弄清你要安装到哪个分区，在状态最底部有一个Available partitions，会显示你的存储卡的位置，一般是/dev/block/mmcblkXpY（X和Y根据情况填写），**注意核对分区大小和你的存储卡大小是否匹配**，若你选错了分区，你就杯具了......

至于RAM选项，这是安装到内存里，虽然速度快，但是没有3到6个G内存不要选，而且一重启就没了（所以别选RAM了吧），还有，请忽视Custom那个选项，因为那个一点用都没有๑乛◡乛๑

![41.jpg](https://wendster.win/blog/usr/uploads/2018/10/1327497990.jpg)

由于我们选的发行版是Ubuntu，所以强迫症再次发作，手动分类改名设置安装路径QwQ

![42.jpg](https://wendster.win/blog/usr/uploads/2018/10/1500434476.jpg)

为了不出现因忘记其默认生成的超难记密码而连不上ssh的尴尬局面，强烈建议修改用户密码（用户名改不改随意）

![43.jpg](https://wendster.win/blog/usr/uploads/2018/10/3568052358.jpg)

你看，这个密码多好记？！

特权用户和DNS一般不用改，不过本地化是要改的（除非你喜欢全英文）

![47.jpg](https://wendster.win/blog/usr/uploads/2018/10/402408999.jpg)

改成zh_CN.UTF-8即可

初始化和挂载都跳过，把SSH启用打上勾，否则连不上这个Ubuntu就尬了！

下面那个VNC先不要管（虽然我知道你很可能需要它），直接点左上角那个小箭头回到主页面

这个设置安装选项貌似很复杂，但是实际上还是很简单的，大约半分钟就可以配置好

## 4.开始安装系统
还是点击右上方的三个小点点

![58.jpg](https://wendster.win/blog/usr/uploads/2018/10/508697094.jpg)

点击安装（废话）

![59.jpg](https://wendster.win/blog/usr/uploads/2018/10/446052666.jpg)

肯定选择确定啦

然后就开始安装了......

![61.jpg](https://wendster.win/blog/usr/uploads/2018/10/3090501869.jpg)

根据手机性能的高低和网速的大小，安装时间4至30分钟不等

![62.jpg](https://wendster.win/blog/usr/uploads/2018/10/2481069389.jpg)

嘿嘿，我六分半就装完了(~˘▾˘)~

哦，对了，出现下面这种情况（就是那个W：Couldn‘t什么的）赶紧按停止键重新安装，以免浪费时间（反正过了一段时间后快要安装完时它会报错让你重装）

![60.jpg](https://wendster.win/blog/usr/uploads/2018/10/2481069389.jpg)

建议安装完后重启一波再食用

![65.jpg](https://wendster.win/blog/usr/uploads/2018/10/601565979.jpg)

请忽视时间的问题（逃）

## 5.使用SSH连接容器
在Android上使用人数最多的SSH软件恐怕就是JuiceSSH了......

![2.jpg](https://wendster.win/blog/usr/uploads/2018/10/3496878951.jpg)

~~我在想这个软件的图标为什么那么像Lemon~~

像Linux Deploy一样，作者依然“贴心”地把下载链接给贴了出来：https://wendster.win/pan/index.php?share/file&user=1&sid=AsZ2y4uU

这个软件打开之后是这个样子的：（请忽视一切诡异的东西吧）

![4.jpg](https://wendster.win/blog/usr/uploads/2018/10/3470278250.jpg)

先点击“连接”（保持忽视状态）

![5.jpg](https://wendster.win/blog/usr/uploads/2018/10/4204916576.jpg)

点击那个有趣的加号（因为你点开JuiceSSH时是什么连接记录都没有滴，需要手动添加）

![6.jpg](https://wendster.win/blog/usr/uploads/2018/10/671712810.jpg)

昵称随便填，比如我填了个Linux~~（为什么不填个NOILinux呢）~~

地址填127.0.0.1或localhost（反正都是代表本机）

然后点击认证那个倒三角——新建认证

![7.jpg](https://wendster.win/blog/usr/uploads/2018/10/217025013.jpg)

昵称随便填，比如我填了个Linux......

用户名是之前配置时那个用户名，我没有改，所以是android

密码就是那个好记的xxxxxx

![8.jpg](https://wendster.win/blog/usr/uploads/2018/10/456290526.jpg)

填好之后是这个样子

![9.jpg](https://wendster.win/blog/usr/uploads/2018/10/1739419340.jpg)

私钥不用管，千万别点左上那个返回键，而是应该点右上的那个勾（否则就重来一遍吧）

然后就会返回到填写“新建连接”的页面，再点一下右上那个勾，回到主页面，在连接那儿点击刚刚保存的连接

然后是类似这个样子

![11.jpg](https://wendster.win/blog/usr/uploads/2018/10/668516204.jpg)

不要管，点确定，然后是这个样子

![12.jpg](https://wendster.win/blog/usr/uploads/2018/10/2010271511.jpg)

点接受即可

接着就连接上啦，可以愉快地打命令啦啦啦

![13.jpg](https://wendster.win/blog/usr/uploads/2018/10/2113815591.jpg)

按音量-可以缩小字体

![14.jpg](https://wendster.win/blog/usr/uploads/2018/10/195869916.jpg)

按音量+可以放大字体

![15.jpg](https://wendster.win/blog/usr/uploads/2018/10/3765989965.jpg)

皮皮真开心！

来，打个gcc

![18.jpg](https://wendster.win/blog/usr/uploads/2018/10/2651165218.jpg)

什么？居然没有gcc？

那就安装一个吧！

先sudo su一波

然后apt install gcc g++

![19.jpg](https://wendster.win/blog/usr/uploads/2018/10/3361570747.jpg)

点个y然后回车

然后就刷刷刷得跑起来了！

安装好了......

![20.jpg](https://wendster.win/blog/usr/uploads/2018/10/3462739844.jpg)

输入一个gcc -v，哈，gcc已经有了！

![21.jpg](https://wendster.win/blog/usr/uploads/2018/10/3174968019.jpg)

输入一个g++ -v，哈，g++也已经有了！

![22.jpg](https://wendster.win/blog/usr/uploads/2018/10/3393068978.jpg)

这下就可以编译了！

不过只能编译，不能编辑文件是什么鬼？那就安装一个vim吧！

![23.jpg](https://wendster.win/blog/usr/uploads/2018/10/2998923706.jpg)

![24.jpg](https://wendster.win/blog/usr/uploads/2018/10/2948046666.jpg)

![25.jpg](https://wendster.win/blog/usr/uploads/2018/10/851523187.jpg)

哈，现在vim也可以用了！

## 6.为容器启用图形化界面
有的童鞋并不满足于终端，还希望像普通的Ubuntu一样有图形化界面。这很简单，只需要将我们之前忽略的VNC打开就可以了！

![50.jpg](https://wendster.win/blog/usr/uploads/2018/10/1041918722.jpg)

桌面选择轻量的LXDE，一是安装快，二是运行快，三是丑陋

![51.jpg](https://wendster.win/blog/usr/uploads/2018/10/3688759713.jpg)

现在返回到主页面，点击右上方的三个小点点。因为已经安装过基础系统了，所以不用重新安装系统，只需要安装要增加的软件包即可。说了这么多，其实就一句话：点击“配置”......

![58.jpg](https://wendster.win/blog/usr/uploads/2018/10/508697094.jpg)

然后是等待～～～～～

好了，安装好了！

![63.jpg](https://wendster.win/blog/usr/uploads/2018/10/327581814.jpg)

仍然建议重启一波哦QvQ

## 7.配置并使用图形化界面
要用VNC连接这个毒瘤Ubuntu，需要一款名曰VNCViewer的软件，作者已经贴心地帮忙准备好了QvQ：https://wendster.win/pan/index.php?share/file&user=1&sid=5BzEJ8FY

打开之后是这个燕子的......

![69.jpg](https://wendster.win/blog/usr/uploads/2018/10/983765399.jpg)

然后点击右下角那个加号

![70.jpg](https://wendster.win/blog/usr/uploads/2018/10/3784610216.jpg)

地址填127.0.0.1或localhost，Name就随便啦

然后点击CREATE，然后在主页面点击你创建的连接

会出现下面这个东西

![72.jpg](https://wendster.win/blog/usr/uploads/2018/10/4155667844.jpg)

记得把它的勾去掉，否则每次都会弹出这个拥有着令人做噩梦的颜色的邪恶的提示

然后就要输密码，可以把Remember passwd勾上，下次就不用输入密码了

![73.jpg](https://wendster.win/blog/usr/uploads/2018/10/1465911680.jpg)

emmmm，密码就是那个好记的xxxxxx

然后，就进去了，这个软件会弹出一个使用教程，可以看也可以跳过，反正是全英文的教程

真正连上后的样子：

![74.jpg](https://wendster.win/blog/usr/uploads/2018/10/1866064065.jpg)

哎呀，下面的任务栏太小了！原来是VNC的分辨率调的太高了，调低一点就好了

先返回到配置系统界面

![50.jpg](https://wendster.win/blog/usr/uploads/2018/10/1041918722.jpg)

点击图形界面设置

![53.jpg](https://wendster.win/blog/usr/uploads/2018/10/3792390454.jpg)

哎呀，颜色深度怎么只有16位呢？还是换成24位全彩色以获得更好的视觉体验吧

![54.jpg](https://wendster.win/blog/usr/uploads/2018/10/2242116312.jpg)

好啦，回到正题。可以看到，分辨率调的非常高，高达1920x1080，要那么高分辨率干嘛？调低一点多好？就像这个样子，对于我的手机来说调到960x540比较好，那就调这么多呗

![68.jpg](https://wendster.win/blog/usr/uploads/2018/10/634576101.jpg)

现在再次重启容器，连上VNC。嗯，现在好多了！

![75.jpg](https://wendster.win/blog/usr/uploads/2018/10/3122975015.jpg)

点一下启动器

![76.jpg](https://wendster.win/blog/usr/uploads/2018/10/385674386.jpg)

谒！怎么中文都乱码了？原来是忘记安装中文字体了......

赶紧用SSH连上终端，敲上这么一打命令
```
apt update
apt install fonts-wqy-microhei -y
```
这样中文乱码这种奇异的事件就不会再有了

为了让界面稍微好看一点，并且修复一下图标的问题，再打一波命令
```
apt install lubuntu-default-session lubuntu-default-settings lubuntu-extra-sessions lubuntu-icon-theme -y
```
然后再次重启容器......

然后再次连上VNC......

好了，现在没有乱码了（咦，我的图怎么找不着了）

滑动屏幕操控鼠标点击左下角的飞鸟图案，然后点击首选项——自定义外观和体验

然后就会出现下面的界面

左边菜单选择Lubuntu-default，然后点击Apply

![77.jpg](https://wendster.win/blog/usr/uploads/2018/10/804647080.jpg)

点击上方的图标主题，选择Lubuntu，然后点击Apply

![78.jpg](https://wendster.win/blog/usr/uploads/2018/10/1513580375.jpg)

点击上方的窗口边框，选择Lubuntu-default，然后点击Apply

![79.jpg](https://wendster.win/blog/usr/uploads/2018/10/4117659811.jpg)

现在回到窗体，点击右下方的字体，左边往下翻，翻到文泉驿微米黑，点击OK

![80.jpg](https://wendster.win/blog/usr/uploads/2018/10/164613413.jpg)

关掉自定义外观和体验，打开终端，顺便把字体也配置一下

![81.jpg](https://wendster.win/blog/usr/uploads/2018/10/3388132186.jpg)

![82.jpg](https://wendster.win/blog/usr/uploads/2018/10/1541211119.jpg)

先sudo su一下来安装一个小软件......

![83.jpg](https://wendster.win/blog/usr/uploads/2018/10/2811926769.jpg)

安装一个Vim用于编辑文件......

![84.jpg](https://wendster.win/blog/usr/uploads/2018/10/421068643.jpg)

哦，忘了已经安装过Vim了！

现在就可以愉快地编程啦！如果有蓝牙键盘，码起代码来会更爽呢！

------------

**教程到这里就结束啦！更多高级玩法还期待你们去挖掘（坑）呢！（欢迎花样作死）**

