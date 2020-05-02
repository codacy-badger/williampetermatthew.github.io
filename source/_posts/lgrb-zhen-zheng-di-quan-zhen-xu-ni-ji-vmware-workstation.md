---
title: 【转载】真正的全真虚拟机：VmWare Workstation
date: 2018-10-04 21:07:13	
tags: 
  - 洛谷日报
---


------------
# 真正的全真虚拟机：VmWare Workstation
------------

> 声明：本文为转载文章，转载自[洛谷日报#64](https://www.luogu.org/blog/ztz11/zhen-zheng-di-quan-zhen-xu-ni-ji-vmware-workstation)，原作者为木木小胖。

------------
记得以前有大佬曾教过我们用win10的自带双系统运行Ubuntu，但这玩意儿好像玩起来并不简单（反正本人试了一上午也没成功），而且这个系统是和windows交互的，我们也很难调整性能参数。今天，我来教大家玩一玩真正的虚拟机——VmWare Workstation

## ①.安装

VmWare是个昂贵的软件，但还是请大家多多支持正版，如果家里没矿的话，可以点[这里](https://pan.baidu.com/s/11s2FzeMYvQpUOkNcepfqgg)下载破解版（仅供学习，严禁用于商业）

首先，解压后双击启动安装

![](https://cdn.luogu.org/upload/pic/34462.png)

进入如下页面时点选我接受，下一步

![](https://cdn.luogu.org/upload/pic/34463.png)

我们一般安装典型，如果大佬想自定义也可以，但会相对麻烦

![](https://cdn.luogu.org/upload/pic/34464.png)

选择要安装的位置，单击下一步

![](https://cdn.luogu.org/upload/pic/34465.png)

静候安装完成（大概得4~5分钟）

![](https://cdn.luogu.org/upload/pic/34466.png)

输入许可证号(支持正版，如果实在不想买的话我的压缩包里有密码机vm10keygen.exe)

![](https://cdn.luogu.org/upload/pic/34468.png)

单击完成

![](https://cdn.luogu.org/upload/pic/34467.png)

## ②.安装系统与配置虚拟机(系统以NOILinux为例)

vmware的虚拟机最大的优点就是可以方便的修改虚拟机的配置，让电脑以你想要的性能运行（~~当然，让虚拟机性能超过你运行虚拟机的物理机是不可能的，这辈子也不可能的~~）

启动虚拟机，你会看到这样一个界面,我们点击创建新的虚拟机

![](https://cdn.luogu.org/upload/pic/34470.png)

他会弹出一个窗口，一般我们点选“典型”

![](https://cdn.luogu.org/upload/pic/34471.png)

我们选择稍后安装操作系统

![](https://cdn.luogu.org/upload/pic/34472.png)

因为NOILinux是基于Ubuntu的操作系统，我们选择linux->Ubuntu

![](https://cdn.luogu.org/upload/pic/34473.png)

选择你要安装的位置，并命名虚拟机

![](https://cdn.luogu.org/upload/pic/34474.png)

这个就看你的爱好了，但记住，虚拟机磁盘不能大于你装虚拟机的磁盘

![](https://cdn.luogu.org/upload/pic/34477.png)

这一步非常重要，像不像CCF老爷机就看这个了。我们单击自定义硬件

![](https://cdn.luogu.org/upload/pic/34478.png)

我们看CCF老爷机的配置，处理器是2009年的AMD+Athlon(tm)+II+x2+240，2.8GHz\times×2，内存4G.

![](https://cdn.luogu.org/upload/pic/34479.png)

处理器的话如果你开到双核每个核心单线程就好。因为主系统方面也会占用一些性能。但如果你是I7的话最好还是开成单核双线程，压缩一下性能，这样更像老爷机

![](https://cdn.luogu.org/upload/pic/34480.png)

内存的话开到1G即可，这样就可以评测512M的程序，4G一般用不完，我就不去开他

![](https://cdn.luogu.org/upload/pic/34481.png)

单击完成，我们发现已经创建好一个空白虚拟机了。下面我们来装系统

![](https://cdn.luogu.org/upload/pic/34483.png)

首先，我们双击CD/DVD，选择使用iso映像文件，路径是NOILinux安装包的位置

![](https://cdn.luogu.org/upload/pic/34485.png)

单击完成返回上一页面，单击“开启此虚拟机”，静待载入

![](https://cdn.luogu.org/upload/pic/34486.png)

进入安装界面后，我们选择简体中文->安装Ubuntu

![](https://cdn.luogu.org/upload/pic/34487.png)

选择清除整个磁盘并安装，然后单击现在安装

![](https://cdn.luogu.org/upload/pic/34488.png)

中间会让你选择时区和语言，选择中国-上海和中文简体即可

然后静候安装完成（大概需要20min）

他会让你重启，按指令重启即可

![](https://cdn.luogu.org/upload/pic/34489.png)

安装完成！就可以进入使用了（初始密码为123456）

### PS：A:我没看出vm有什么优点啊？

### 我：别急，下面就让你涨涨见识

## 1.方便的系统间文件传输

vm系列的文件传输才不需要什么高端操作呢！动动鼠标就行。

首先，我们启动虚拟机，让他和主系统并列的放置

![](https://cdn.luogu.org/upload/pic/34678.png)

然后，我们选中要移动的文件，拖放到虚拟机的范围

![](https://cdn.luogu.org/upload/pic/34625.png)

松开鼠标，文件便会自动复制到虚拟机中

![](https://cdn.luogu.org/upload/pic/34631.png)

## 2.虚拟机的移动

很良心的功能啊

不管走到哪里，只要有电脑，你都可以随心所欲地使用原来的虚拟机

### ①.虚拟机的打包

其实说打包是不完全正确的，因为其实方便到无需打包

首先，我们找到虚拟机的安装目录

你会发现这么一堆东西

![](https://cdn.luogu.org/upload/pic/34632.png)

这些东西就是虚拟机的配置文件和虚拟磁盘。我们把他们装在一个文件夹里，复制到U盘里就好了。这些文件会保留你当前虚拟机的一切状态

### ②.虚拟机的载入

上面我们说了如何打包虚拟机，这里我们来说说如何载入打包好的虚拟机

第一步，打开VmWare

看好，这里是没有任何虚拟机的

![](https://cdn.luogu.org/upload/pic/34634.png)

我们选择：文件->打开

![](https://cdn.luogu.org/upload/pic/34637.png)

在弹出的窗口中选择你的虚拟机所在的位置,选中后点击即可

![](https://cdn.luogu.org/upload/pic/34641.png)

OK

![](https://cdn.luogu.org/upload/pic/34646.png)

~~妈妈再也不用担心我出门要背着笔电了~~，只要有一个U盘，你就可以走遍天下，在任何装了VmWare的地方使用虚拟机

### ③.虚拟机的外部硬件连接

如果你想让U盘直接连接到虚拟机上，请看这里

首先，我们把U盘插到物理机上，同时开启虚拟机

![](https://cdn.luogu.org/upload/pic/34650.png)

我们会发现虚拟机的右下角有一排小图标，那里就是虚拟机可以连接或已经连接的硬件

![](https://cdn.luogu.org/upload/pic/34654.png)

我们要安装的是一个U盘（图标框右面第二个，我们单击图标）

![](https://cdn.luogu.org/upload/pic/34657.png)

在弹窗中点击连接即可

![](https://cdn.luogu.org/upload/pic/34660.png)

这时候图标会变亮，表示已经连接

退出连接也很简单，再点击一下即可（这时图标会变暗）

### ④.挂起虚拟机

有时候，我们可能有事，要关闭物理机，但是，我们又不想关掉虚拟机，怎么办呢？

这时候，我们就可以用挂起解决问题

首先，我们在虚拟机的界面找虚拟机->电源->挂起虚拟机，然后单击

![](https://cdn.luogu.org/upload/pic/34670.png)

这时系统会弹出一个加载页面，静候完成，之后界面会变成这个样子,表示挂起完成

![](https://cdn.luogu.org/upload/pic/34672.png)

你可以干别的去了qwq，回来的时候单击“继续运行此虚拟机”即可

## VmWare的介绍到此结束，祝大家玩的愉快