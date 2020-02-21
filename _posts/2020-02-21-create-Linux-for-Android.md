---
layout: post
title: '旧安卓手机的利用-搭建Linux系统和个人网站'
subtitle: '利用旧手机记录日常，学习Linux'
date: 2020-02-21
categories: 记录
cover: 'https://note.youdao.com/yws/api/personal/file/0B53F6F874A44209AF6A0F3F49488399?method=download&shareKey=01aa2c14b320831dae209c01cbf8b797'
tags: Linux_deploy  ubuntu Linux 旧手机
---


旧安卓手机仍了可惜还污染环境（非正规处理渠道），换盆总觉得亏，那怎么能继续发挥余热呢？本文就手把手教你怎么用旧手机搭建Linux系统并以Linux系统为服务器搭建个人网站，可以记录日常生活，做电子日记，不怕信息泄露；也可以内网穿透以外网访问。  
首先说明，本教程在前人种树的前提下做出，感谢大神们对我的帮助，由于参考教程较多，这里就不一一列举感谢了（*主要是自己也记不清了*）当然本教程也会有诸多纰漏，普适性较差还望大家包含和指正。然后我用的硬件为乐视超级手机1（处理器helio x10  arcch64架构），win10电脑一台。 

---
# 安装Ubuntu Core（或其他Linux）
关于准备工作的教程很多，我这里就不详细介绍，做一个汇总（*不保证适用于每个人*），只重点介绍在这个过程中我踩过的坑：
## 方案一：Linux deploy容器  
该方案可以达到很高的运行速度，现在的安卓手机性能已经足够运行，所以可以使用此方案来学习Linux或满足一时好奇心尝尝鲜，用来装X也不错。

准备软件如下：Linux deploy、magisk root包==安卓新版本==或su root包==安卓7以下==、busybox安装器。
手机端操作软件JuiceSSH（*其他ssh*）和VNC Viewer(*其他vnc*)
桌面端操作软件Xshell、VNC Viewer等

地址如下：  

busybox   <https://github.com/meefik/busybox/releases>  
linuxdeploy  <https://github.com/meefik/linuxdeploy/releases>  
JuiceSSH  <https://www.juicessh.com/>各大应用商店有售  
VNC Viewer  <https://www.realvnc.com/en/connect/download/viewer/>


具体过程和诸位搜到的大同小异，

 1. 将手机root
 2. 安装busybox，进入后点击安装,并记下安装路径，一般为/system/xbin。
 3. 安装Linux deploy ，配置环境，点击PATH变量更换为第二步中的地址（==其他大佬都说要这样做，也就写上了，但自己的经验是这个并没什么用==）一般上面三项都选上，有
 4. 点击下图中按钮即可进入系统配置,具体配置选项可参考其他大佬的介绍，这里推荐两个[1](https://blog.csdn.net/qq_20084101/article/details/80816045)和[2](https://post.smzdm.com/p/228886/)。其中SSH一定要开启，图形界面看自己需求，其实ARM平台的软件相对来说很少，即使安装到了图形界面，虽然现在处理器性能足够，但由于驱动和VNC转发的缘故，总会有很多的延迟和缓慢，勉强能用吧！  
![配置镜像](https://img-blog.csdn.net/20180626152423465?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIwMDg0MTAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
 5. 配置完成后点击下图2，这里需要特别说明的是网络一定得是稳定且没有防火墙的，或者没有屏蔽某些内容的，否则会无法下载资源，当安装失败时可以尝试更换网络试试。  
 ![Linux deploy](https://img-blog.csdnimg.cn/20200221212518838.jpg)
6. 当出现以下画面并显示<<<deploy后说明安装已完成，需要看一下以下两个部分最后两行的ssh和vnc是否是done。如果出现fail可以换个网络，或者换个安装方式，如目录的换成镜像文件。如果以上 方式 都不行的 话，那说明你不正确的操作太多了，一定 在点击 停止后在进行系统文件的删除，然后恭喜你可以重新刷机了。  
![正确安装](https://img-blog.csdnimg.cn/20200221220838644.jpg)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200221221925477.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)
![完全成功](https://img-blog.csdnimg.cn/20200221222712202.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)
## 方案二：Termux
termux据说是安卓端神器，仅仅几兆的软件就可以模拟Linux众多命令，安装即用，当然扩展也是其杀手锏，通过安装不同的环境文件，可以内建很多的Linux，左侧右滑可以调出对话选项，一个对话可以新建一个环境。可以利用闲暇时间来学习，相信手机码字感觉非常不好，它还支持ssh或vnc到window。同时还可以模拟window等，非常强大，有一个支持的社区。Termux是一个Android下一个高级的终端模拟器, 开源且不需要root, 支持apt管理软件包，十分方便安装软件包, 完美支持Python, PHP, Ruby, Go, Nodejs, MySQL等。随着智能设备的普及和性能的不断提升，如今的手机、平板等的硬件标准已达到了初级桌面计算机的硬件标准, 用心去打造完全可以把手机变成一个强大的工具.  
termux有两个官方下载途径因为众所周知的缘故，大多可以从[F-Droid](https://f-droid.org/packages/com.termux/)下载。  
![termux软件运行图](https://img-blog.csdnimg.cn/20200221203535851.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)  
关于termux的使用已经有太多的教程，我不想在重新总一遍了。只是推荐一个termux的魔改版，添加了太多实用的功能具体可参考文件链接：<https://pan.baidu.com/s/17228hWttFatUYHNNlDU9Vg>提取码：sw5w

---
# 为ubuntu安装桌面环境（大约占1G）(转)[原文链接](https://www.cnblogs.com/blowhail/p/12080855.html)
## 方案一：最简单
当然就是Linux deploy自带的配置是需要做出选择的桌面了。我在尝试后还是推荐这种方式，其他方式安装的桌面会出现文件目录挂载不正确的情况 。这时候就需要有一定的技术去做软连接，对于新手有一定的难度（大神请自动略过）。
## 方案二：安装gnome桌面
```bash
sudo apt-get install gnome-core
```
安装vnc
```bash
sudo apt-get install vnc4server
```
启动vnc
```bash
vncserver
```
<font color="green">设置一下密码</font>

接着设置一下配置文件
```bash
vim ~/.vnc/xstartup
```
修改为

复制代码
```bash
#!/bin/sh

# Uncomment the following two lines for normal desktop:
# unset SESSION_MANAGER
# exec /etc/X11/xinit/xinitrc

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
vncconfig -iconic &
x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
x-window-manager &

gnome-panel &
gnmoe-settings-daemon &
metacity &
nautilus &
```
复制代码
然后重启vnc
```bash
vncserver -kill :1 //关闭

vncserver :1　　　　//开启
```
用vnc连接的时候 地址栏填写 ip:1 然后输入刚刚设置的密码就可以进入了

 

如果出现桌面是灰色的现象，尝试一下下面的方法

解决灰色桌面问题：
```bash
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh
```
原因是gnome有些组件没有装上  
## 方案三 安装ubuntu desktop（转）[原文链接](https://www.linuxidc.com/Linux/2018-12/156031.htm)
 Tasksel是一个特定于Ubuntu和Debian的工具，它有助于将多个相关软件包安装为协调任务。 Tasksel使得安装相关软件包非常容易，这些软件包组成了以下内容：

LAMP Server
Mail Server
Print Server
Database servers
Samba file server
And more


例行升级
```bash
sudo apt-get update
sudo apt-get upgrade -y
```
安装桌面工具
```bash
sudo apt-get install tasksel -y
```
运行工具
```bash
sudo tasksel
```
将打开一个基于curses的GUI。使用键盘箭头键，向下滚动以选择Ubuntu desktop.选择Ubuntu桌面进行安装。
![tesksel](https://img-blog.csdnimg.cn/20200221143310268.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)
选择Ubuntu桌面后，单击空格键将其选中，按Tab键选择Ok，然后按键盘上的Enter键。 这将在Ubuntu Server上安装成功的GNOME桌面所需的一切。 完成此过程后，重新启动服务器，重启后，LightDM显示管理器将迎接您
![安装过程](https://img-blog.csdnimg.cn/20200221142448918.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)


---
最后附上我的 桌面吧：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200221223022448.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)
也欢迎大家进入我的[CSDN](https://mp.csdn.net/console/article)参观