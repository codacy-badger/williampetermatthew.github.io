---
title: 【转载】运行linux的第四种方式：双系统
date: 2019-01-07 07:49:58	
tags: 
  - 洛谷日报
---


------------
# 运行linux的第四种方式：双系统
------------

> 声明：本文为转载文章，转载自[洛谷日报#109](https://92602.blog.luogu.org/multi-os-install-ubuntu)，原作者为WarrenWN。

------------
# 双系统
前面日报里已经介绍了3种运行linux的工具，我来介绍第四种，这种方法能让你用上**真正**的linux。

# $\color{red}{Warning}$  
# $\text{\color{red}这种方法可能会损坏您的电脑，请谨慎尝试}$

![不作死就不会死](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1544189391808&di=25bff4b12d82f13c607a5ff423787aba&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201408%2F11%2F20140811140831_ABX4v.thumb.700_0.jpeg)

下面进入正题：

# 1. 准备
一个**被格式化**的USB（容量 > 2GB 且已经插入电脑），提前备份好数据
良好的网络环境
# 2. 下载
点击 [$\text{ Ubuntu 18.04.1 LTS 下载链接 }$](https://www.ubuntu.com/download/desktop/thank-you?country=CN&version=18.04.1&architecture=amd64)

默默地等待下载完...

这么一个文件

![ubuntu-18.04.1-desktop-amd64.iso](https://s1.ax1x.com/2018/12/07/F3Q4HS.png)

下载[UltralISO](https://cn.ultraiso.net/uiso9_cn.exe)并且无脑安装；

将之前的文件用UltralISO打开(如果提示尚未注册或尚未购买什么的点继续试用): 

![](https://s1.ax1x.com/2018/12/07/F3lig1.png)

点击 启动 ，再点击 制作硬盘映像;
![](https://s1.ax1x.com/2018/12/07/F3l8Df.png)

给予管理员权限，然后在弹出窗口里点“写入”: 

![](https://s1.ax1x.com/2018/12/07/F310Fe.png)

# 一定要确保U盘被格式化且里面的数据都已保存!!!
弹出窗口点“是”

等一段时间

![](https://s1.ax1x.com/2018/12/07/F33IAO.png)

# 完成后点终止，然后您的U盘就是一个~~标准的~~Ubuntu 18.04.1 LTS 安装U盘了！
# 3. 硬盘分区
分出**连续的**至少50GB的磁盘空间，具体方法请自行百度（一般从C盘分出，Win+R 运行 “diskmgmt.msc” 之后对C盘单击压缩卷，压缩空间量填51200 (MB为单位)，然后确定，如果分不了的可以用其它工具如DiskGenius等,在此不一一解释）

我分了75GB

![](https://s1.ax1x.com/2018/12/08/F3Dzy8.png)

# 4. BIOS设置
关掉电脑。重启，进入BIOS界面（各个电脑进入bios的方式不同，我的是惠普笔记本，就狂按f10）

Hp的BIOS长这个样子 

![](https://s1.ax1x.com/2018/12/08/F3DoLD.jpg)

$\text{\color{blue}以下步骤各个电脑大致相同但是会有一些出入，在此仅演示Hp笔记本}$

右箭头切换菜单到System Configuration,选中Boot option（上箭头下箭头切换） 

![](https://s1.ax1x.com/2018/12/08/F3DfRx.jpg)

按回车后找到USB Boot选项（USB启动），打开它(选中按回车后进行选择，点Enabled)

![](https://s1.ax1x.com/2018/12/08/F3Dhz6.jpg)

# 找到"Secure boot"(安全启动，关掉它)

![](https://s1.ax1x.com/2018/12/08/F3D5QK.jpg)

![](https://s1.ax1x.com/2018/12/08/F3DWJ1.jpg)

# 把USB Diskette on Key/USB Hard Disk 挪到最上面（uefi启动挪UEFI Boot Order下的，Legacy启动把UEFI和Legacy的都挪一下，用f5,f6挪动）
$\text{\color{green}提示：其它电脑这一步与HP具体操作不同。一句话概括:把可移动设备/USB挪到启动顺序的第一个}$

![](https://s1.ax1x.com/2018/12/08/F3DIsO.jpg)

选中后f5

![](https://s1.ax1x.com/2018/12/08/F3D7ee.jpg)

f10退出，Save changes选Yes

![](https://s1.ax1x.com/2018/12/08/F3DHdH.jpg)

然后关机

# 5. 安装
之前制作的USB插入电脑（不要开机！！！）

然后按开机

**如果一切正常**，会出现

![](https://s1.ax1x.com/2018/12/26/F2Pfeg.jpg)

如果没有，说明前面的步骤有问题,回到BIOS设置

选中Try Ubuntu,回车
```then......```

![Try Ubuntu](https://s1.ax1x.com/2018/12/08/F34Ye1.png)

一个纯Ubuntu！
等等，这个Ubuntu只是试用的...

点击**Install Ubuntu 18.04.1 LTS**

![](https://s1.ax1x.com/2018/12/08/F34Ci8.png)

选语言。简体中文在倒数第3个

![](https://s1.ax1x.com/2018/12/08/F34iRg.png)

继续

![](https://s1.ax1x.com/2018/12/08/F34AMj.png)

这玩意也用不着改，继续(改成英语，美国也可以)

# 下一步它会让你选择要不要连wifi，选择不要连，继续（图略）
更新和其他软件:选择最小安装，其它默认

![](https://s1.ax1x.com/2018/12/08/F34FzQ.png)

然后坐等几分钟:

![](https://s1.ax1x.com/2018/12/08/F34No6.png)

这一步卡住一段时间是正常的，数分钟后下一步:

![](https://s1.ax1x.com/2018/12/08/F34PJS.png)

# 如果它说电脑上没有安装操作系统，就退出安装，回到“BIOS 设置”一步
否则就选“其它选项”

于是又过了一段时间

![](https://s1.ax1x.com/2018/12/08/F34aFK.png)

窗口左下角有一个**加号**，用于创建分区，选中在Windows里边分出的那一大块磁盘空间（至少50GB的，我分了75GB），点 +

跳出来一个对话框

## 分swap
跳出对话框里，“用于”那栏的下拉菜单里选择“交换空间”，“大小”那一栏写自己物理内存的2倍（我4G的内存就写8192 **MB**，注意单位）,分区类型选主分区，空间起始位置 

![](https://s1.ax1x.com/2018/12/08/F34md0.png)

点OK，等一会

## 分EFI（貌似Legacy启动可以跳过这一步）
选择那大块空间，再点“+” (为了让我少打些字，以后这两步不再详述)

分区类型 逻辑分区，空间起始位置，用于EFI系统分区，大小500MB

![](https://s1.ax1x.com/2018/12/08/F34eZq.png)

## 分Boot
创建分区方法如上，逻辑分区，空间起始位置，EXT4日志文件系统，挂载点 ```/boot```，(下拉菜单里选) 大小500MB

![](https://s1.ax1x.com/2018/12/08/F34Ess.png)

## 分rootfs
创建分区方法如上，逻辑分区，空间起始位置，EXT4日志文件系统，挂载点 ```/```，(下拉菜单里选) 大小10G~16G，我填了12G（12248MB）

![](https://s1.ax1x.com/2018/12/08/F34GLR.png)

## 分home
创建分区方法如上，逻辑分区，空间起始位置，EXT4日志文件系统，挂载点 ```/home```，(下拉菜单里选) 大小...自定（至少20480MB，我的空间多就分了30720MB，这个目录的作用类似于'C:\Users'） 

![](https://s1.ax1x.com/2018/12/08/F348y9.png)

## 分usr
创建分区方法如上，逻辑分区，空间起始位置，EXT4日志文件系统，挂载点 ```/usr```，(下拉菜单里选) 大小 剩下的都给它了（一般默认是占用全部空间的）

![](https://s1.ax1x.com/2018/12/08/F34tdx.png)

## 设置引导（Legacy跳过）
下面安装启动引导器的设备:下拉选择刚刚分出来的EFI分区 (下拉菜单里显示设备名，你要让设备名和efi分区的设备名相同，如图，刚刚分出来EFI分区是/dev/sda10,下拉菜单里选择/dev/sda10)

![](https://s1.ax1x.com/2018/12/08/F34noV.png)

## 确定
第几分区可能不一样，只要看看是不是格式化了6个分区且6个分区的类型对得上就行了（注意 继续是右边的按钮）

![](https://s1.ax1x.com/2018/12/08/F34MJU.png)

如果有警报什么的产生，退出安装，Windows里删除刚刚分出来的分区，重来一遍

一切正常就会进入选择地区，就选择上海，不用更改

![](https://s1.ax1x.com/2018/12/08/F34QWF.png)

再点继续，最后一步，填写用户名

![](https://s1.ax1x.com/2018/12/08/F34KiT.png)

我的写完了是这样

![](https://s1.ax1x.com/2018/12/08/F34lz4.png)

继续，开始安装

![](https://s1.ax1x.com/2018/12/08/F34VLn.png)

坐等,最后

![](https://s1.ax1x.com/2018/12/08/F343QJ.png)

# Finish!（先点继续试用，然后关机（关机键在右上角））
**关机后拔掉USB**

# 补充
HP电脑还要在BIOS里改一下

老样子进入BIOS的这个界面

![](https://s1.ax1x.com/2018/12/09/F8yNNQ.jpg)

选中OS Boot Manager 回车

![](https://s1.ax1x.com/2018/12/09/F8ytAg.jpg)

选中ubuntu，f5挪到Windows Boot Manager上面

![](https://s1.ax1x.com/2018/12/09/F8yJHS.jpg)

按f10退出这个界面，再按一次f10保存

# OK!
# $\text{\color{red}补充}$
# 删除双系统（以删除Ubuntu为例）：
参考 [这个链接](https://www.cnblogs.com/pualus/p/7835422.html)

（虽然在我的电脑上直接KO掉Ubuntu的磁盘也不会出事（BIOS比较智能？））

保险起见，还是先把启动项删了:

管理员身份启动cmd，输入
```
bcdedit /enum ALL
```
![del_os1](https://s1.ax1x.com/2018/12/08/F3DxQf.png)

（device和path被我涂掉了，标识符也被涂掉了一部分）

有一个 description 类似或接近ubuntu的（高亮）

把它的标识符一栏(那个{0e ...... 963}的东西)连着括号一起复制下来

在cmd中输入（不要按回车）
```
bcdedit /delete
```
敲一个空格，然后粘贴你刚刚复制的标识符，按回车

![del_os2](https://s1.ax1x.com/2018/12/08/F3DvSP.png)

操作成功完成后，（我的电脑）就可以删除Ubuntu的磁盘空间啦！

（别的电脑我不知道行不行，最好还是用上面那个链接的方法）

# 祝大家成功地装上一个真正的Ubuntu