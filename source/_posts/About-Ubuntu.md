---
title: Ubuntu使用心得
date: 2019-01-07 10:51:53
tags: 
  - 科技
---
由于NOI官网给出的NOI Linux 1.4.1是基于Ubuntu 14.04.5的，因此我特意使用Ubuntu 14.04.5

$\huge\color{red}{Warning}$ 请不要尝试在上个世纪的电脑上执行以下操作，这可能会导致严重的“Kernel panic - not syncing: Attempted to kill init!”错误  
$\huge\color{blue}{Attention}$ 请在执行下述操作时确认自己为根目录管理员（root）或拥有根目录管理权限

# 安装
安装比较类似NOI Linux的安装过程，不再过多阐述。
# 使用
个人喜欢一些小设置。。。
## VMware Tools
这个工具是方便虚拟机与Ubuntu之间交互的一个软件，NOI Linux自带了这个工具，但是我们的纯净版不带，所以我们来安装它。

首先点击VMware的菜单栏里的 虚拟机 ，点击 安装VMware Tools，然后下方会出来一个提示，让我们用文档管理打开VMwareTools-9.6.1-1378637.tar.gz，并执行vmware-install.pl。具体做法是先将那个包复制到桌面，然后双击打开，将文件夹拖到桌面，然后召唤终端，到文件夹下执行
```
./vmware-install.pl
```
然后出来提示就敲回车（无脑安装），最后出现VMware团队的祝福后重启Ubuntu再查看菜单栏，显示出 重新安装VMware Tools 就证明安装成功了。
## 更新
- 点开 全部设置 ，找 语言支持 ，更新
- 点 软件和更新 ，除了 提前释放出的更新 都勾上
- 点 详细信息 更新

最后，我不更新到Ubuntu 16.04.1~~（绝对不是因为更新不到）~~。
## 其他系统
我之前安装的其他系统去哪儿了？  
明明已经安装但是仍显示只有Ubuntu，这时候不要急，打开终端，输入
```
sudo update-grub2
```
当屏幕出现 done 即表示完成，重启后就能看到其他系统了。
## 密码
Ubuntu的密码要求要有一定的强度，所以我们打开终端，输入
```
sudo -s
```
得到了root权限。  
然后输入原密码。  
现在我们输入
```
passwd XXX（你的用户名）
```
输入新密码就好了，就比如我的密码就是六位纯数字，
```
******（我又不傻）
```
若修改成功，则会返回
```
password updated successfully
```

如果忘记密码怎么办？  
额，开机按Shift键，出现如下界面。（手速要快，Shift键要按时间久一点）  
在开机菜单里选择Ubuntu高级选项（第二项）
![](https://res.zhangkai.xin/pic/20161213175823266.jpg)
按回车键进入如下界面，然后选中有recovery mode的选项（第三项）
![](https://res.zhangkai.xin/pic/20161213175947735.jpg)
按e进入如下界面，并找到图中红色框的
```
recovery nomodeset
```
![](https://res.zhangkai.xin/pic/20161213180034937.jpg)
删除，并在这一行的后面输入
```
quiet splash rw init=/bin/bash
```
后，按F10
![](https://res.zhangkai.xin/pic/20161213180105876.jpg)
如果忘记用户名，可以键入
```
mount -o rw,remount /
ls /home
```
查询用户名。

然后，在命令行输入
```
passwd XXX（XXX是你的用户名）
```
此时注意小键盘的指示灯是否亮起。。。  
然后输入两遍123456（即新密码）来修改密码，若修改成功，则会返回
```
password updated successfully
```
然后关机后正常启动，输入123456（即新密码）就能访问了

**切记不可使用此手段进行违法的密码破解行为**
## 外观
桌面主题选的是Radiance，壁纸选的是Foggy Forest (2560 x 1709)  
应用程序->系统工具->首选项->主菜单，把所有勾点上  
开启工作区，添加“显示桌面”图标到启动器  
显示窗口菜单在窗口的标题栏
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
## Gedit
作为一名OIer，我按NOI Linux使用心得介绍的方法配置完Gedit后，兴高采烈地敲好了hello world，正要编译，突然提示我g++没有安装。。。

我们点开Ubuntu软件中心，搜索~~gcc然后发现已经安装，于是重新搜索~~g++，安装它。重启打开Gedit后就能用了(＾－＾)V。
## FireFox
正常设置字体，可以正常登陆洛谷~~3.5~~。
## LibreOffice
其实这是一款非常好用的Office软件，因为它免费而且开源，相比WPS的来说快，相比MS的来说……
## Notepadqq
其实这是Notepad++的非官方Linux版，然后是Notepadqq的官方版。。。
我们在终端执行
```
sudo apt-get update
sudo apt-get install snapd
sudo snap install --classic notepadqq
```
即可安装，可惜的是笔者暂未研究出如何使其具有编译能力，所以暂时无法分享OI用的技巧不过这仍是一款优秀的文本编辑器。
## Ubuntu的其他应用
按Super键（即键盘上的Win键）打开搜索，搜索
- AisleRiot接龙游戏：即Windows自带的纸牌
- Mahjongg对对碰：即Windows自带的麻将消除游戏
- Mines扫雷：即Windows自带的扫雷
- Sudoku数独：即Windows10可以安装的数独


------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。