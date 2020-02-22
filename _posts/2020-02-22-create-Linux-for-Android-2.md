---
layout: post
title: '旧安卓手机的利用-搭建Linux系统和个人网站 旧手机建立自己的博客网站之踩坑记'
subtitle: '在旧手机上建立自己的服务器（2）||2020年新货'
date: 2020-02-22
categories: 记录
cover: 'https://images.pexels.com/photos/3703074/pexels-photo-3703074.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940'
tags: Linux_deploy  ubuntu Linux 旧手机 wordpress
---

接上一篇
接下来配置LNPM环境。
# 方案一
根据[大佬](https://post.smzdm.com/p/228886/)的博文安装时会出现以下情况网站搜索无果，全是让改软件源的，改完后问题依然，其实分析后可知，无法定位就是源里面没有软件或者源错误，所以 这里不指定版本安装。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200222125759883.png)
这里附上清华的软件源，仅适用于ubuntu bionic for arm64/armhf。
```C-like
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-proposed main restricted universe multiverse
{ color: black }
```

1. 安装nginx，php，mysql，可以一次性输入安装命令，这里不指定版本，因为软件更新的原因会有错误，需要注意的是需要关注安装软件的版本。
```C-like
apt-get install nginx php-fpm mysql-server php-mysql
```
如果安装过程中出现以下情况
```C-like
sudo apt-add-repository https://dl.winehq.org/wine-builds/ubuntu/
#apt-add-repository：找不到命令
错误。
修复方法如下：
apt-get install software-properties-common
重新执行命令即可。
```
装完之后启动服务
```C-like
service nginx start

service php7.2-fpm start
```
service mysql start #当MySQL启动失败时，可以输入以下命令后再次输入启动mysql的命令
```C-like
usermod -a -G aid_net_raw mysql 
```
然后配置一下nginx的配置文件,先看一下php-fpm的配置文件。nginx处理请求是通过fpm（来管理fastcgi的）来实现请求和响应。而nginx和php-fpm可以通过监听9000端口（默认）或者socket来实现。而9000的格式是127.0.0.1:9000，是走网络的。通过ngxin的conf文件，把.php结尾的都交给9000端口处理，php-fpm（fastggi的进程管理器）选择并连接到一个fastcgi子进程，并将环境变量和标准输入发送到fastcgi子进程，然后不断的处理请求响应socket文件就不走网络，是套接字。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200222133504494.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)
然后将该路径和下面修改的路径一致  
vim/etc/nginx/sites-available/default #修改两个，增加一个index.php格式支持，把关于php7.0-fpm的注释去掉，并改为安装的版本php7.2-fpm  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200222130711965.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)
然后重启以下项,出现OK字样表示成功。  
```C-like
service nginx reload
service php7-fpm reload
```
这时候在浏览器地址栏输入你的ip即可预览到nginx默认网页。这时候将wordpress文件复制到手机，并放入/var/www/html/ 中，此目录为nginx的默认网页，原有文件可打开预览。可以删除源文件并将wordpress内文件复制到此目录，输入你的ip即可看到，如果你直接复制的wordpress，那得在你的ip后面加上wordpress。即http:ip/wordpress/。可看到如下画面，这时候可以开始配置wordpress了，在上面选择模板配置网页等 ，这些这里不介绍了。    
传文件我用的数据线放到手机的根目录即我们挂载的根目录 /sdcard。然后在Linux上移动到了/var/www/html/ 中。你也可以使用sftp工具移动文件。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200222152800886.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200222153151177.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)  
# 方案二
安装宝塔界面后，一键配置LNPM环境，一键部署wordpress。ubuntu安装命令为  
```C-like
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh
```
其他系统请参考<https://www.bt.cn/bbs/thread-19376-1-1.html>  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200222153404618.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)
下面即为宝塔的安装完成画面，其中Bt-Panel即为登录网址，如果是局域网本机登录换成localhost或者127.0.0.1。如果是电脑远程 登录 直接换成手机的外网IP。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200222153417494.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200222153522458.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200222153437258.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200222153448555.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)
在软件商店选择一键部署wordpress即可完成网站部署，当然这其中肯定有 很多问题，比如说安装完成后依然显示未安装。socket报错等。好像问题各种各样，你得做好折腾的准备。还有 安装成功 后 无法启动等问题。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020022215431142.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)
# 总结
总结一下这两周的折腾吧，ARM平台的服务器资源还是比较少的，并且由于平台的不同软件包得自己编译安装，是一个大工程。不过这几天的折腾还是学了很多东西的 。喜欢折腾，我折腾最终的结果是取消了ubuntu的图形界面，然后配置了LNPM按方案一弄的网站。取消所有的省电设置，24小时插电，下载一个桌面时钟，装了一个小爱同学。恩！算是物尽其用了吧！   


==后台跑网站+桌面时钟 +语音助手 ==


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200222155750839.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)
欢迎大家到我的[CSDN](https://me.csdn.net/weixin_44235031)参观
