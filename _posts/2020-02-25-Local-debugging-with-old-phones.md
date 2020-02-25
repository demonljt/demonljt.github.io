---
layout: post
title: 'win下jekyll调试博客不方便，不妨用手机试试吧！！'
subtitle: 'jekyll进阶-本地调试&&旧手机的利用'
date: 2020-02-25
categories: 记录
cover: 'https://images.pexels.com/photos/1036936/pexels-photo-1036936.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940'
tags: gitalk jekyll 博客 评论 历程 本地调试
---

win下jekyll调试博客不方便，还有很多错误。而博客每次写完或调试完需要重复git add， git commit， git push，即使有了github desktop，一样是很不方便。而在jekyll官网上我们知道jekyll博客支持本地调试，麻烦的是搭建环境，相比于windows，Linux下的环境搭建就方便的多了，本文就介绍怎么用安卓手机来搭建环境本地调试博客程序。旧手机搞他，再做个代理和内网穿透，实现手机服务器搭建博客，解决百度蜘蛛摸github烫腿的问题。  
这里接上一篇,请参考[旧手机建立自己的博客网站之踩坑记||在旧手机上建立自己的服务器（2）||2020年新货](https://blog.csdn.net/weixin_44235031/article/details/104442137)  
[安卓装Linux ，坑真的多，Linux deploy&Termux踩坑记||在旧手机上建立自己的服务器（1）||2020年新货](https://blog.csdn.net/weixin_44235031/article/details/104424131)  
这篇博文的前提是你已经在手机上装好的Linux系统，这里以ubuntu18LTS版本为例说明：
打开jekyll安装官网<https://www.jekyll.com.cn/docs/installation/ubuntu/>  
根据提示进行环境配置  ，首先安装依赖  
```C-like
sudo apt-get install ruby-full build-essential zlib1g-dev
```
最好避免将 Ruby Gem 安装为根用户。因此，我们需要为您的用户帐户设置 Gem 安装目录。以下命令将环境变量添加到文件中，以配置 Gem 安装路径。立即运行它们：~/.bashrc    

```C-like
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```
安装 jekyll  
```C-like
gem install jekyll bundler
```
至此你已经准备好使用jekyll了  ，如果有以下 错误说明你的镜像源错误，这时候你需要准备梯子或者修改镜像源 。
![出错](https://img-blog.csdnimg.cn/20200223190932187.png)  
更新国内镜像源
```C-like
$ gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
```
查看镜像源
```C-like
gem sources -l
```
既然要将网站部署在Github Page上，那自然少不了使用git，可以使用如下命令安装git：
```C-like
sudo apt-get install git
```
在终端中输入git –version，出现git version 1.9.1，则说明git安装成功：
```C-like
  就像官网所说的简单的只需要四步
  快速开始
  gem install jekyll bundler
  jekyll new my-awesome-site
  cd my-awesome-site
  bundle install
  bundle exec jekyll serve

 打开浏览器 http://localhost:4000
```
关于环境配置问题这里推荐[intzero大神](https://lszero.github.io/coding4fun/static-website-with-jekyll.html)的方式和处理，下面补充介绍在网站clone主题下来本地调试容易出现的问题。  
**这里推荐使用手机的内部存储作为博客的存放地即sdcard（前提是你挂载了）**
如果你是代码晕的话，跟着走吧，首先在主题网站选择好你的主题，并找到github仓库，点击右侧下载zip文件后，解压拷到手机的内部储存，就是数据线连接你看到 的那个，然后在ssh客户端连接你的Linux，输入以下命令：
```C-like
sudo apt-get install ruby-full   # Debian 或 Ubuntu 系统
gem install jekyll bundler
cd /sdcard/your-site   #your-site即你的博客文件夹
bundle install
bundle exec jekyll serve

最后 打开浏览器 http://localhost:4000
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200225203113559.png)
如果你遇到以上问题：首先要确保你安装了'jekyll-paginate'，可以执行以下命令尝试：  


```C-like
sudo gem install jekyll-paginate
```

如果已安装，那根据ensure that you have included the jekyll-paginate gem in your Gemfile as well. 报错来说你需要在你的  Gemfile 中添加 ```gem jekyll-paginate```字段如下面所示：  

```C-like
source "https://rubygems.org"
# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!
gem "jekyll", "~> 4.0.0"
# This is the default theme for new Jekyll sites. You may change this to anything you like.
gem "minima", "~> 2.5"
# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
# gem "github-pages", group: :jekyll_plugins
# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
install_if -> { RUBY_PLATFORM =~ %r!mingw|mswin|java! } do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :install_if => Gem.win_platform?

gem 'jekyll-paginate'
```

如果你遇到了下面的问题：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200225204055902.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)
说明你的配置文件中没有配置扩展文件。在_config.yml配置文件的扩展选项中添加以下字段，则分别可以支持在X86架构的ubuntu和arm64架构下本地调试。  

```C-like
# Plugins
plugins: [jekyll-paginate]
plugins_dir: [jekyll-paginate]
gems: [jekyll-paginate]
```

修改以上问题后，如果看见了以下界面，那么恭喜你，在浏览器输入http://localhost:4000/即可看到的站点。之后就可以直接在旧手机上本地调试。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020022520510024.png) 

欢迎大家到我的[CSDN](https://me.csdn.net/weixin_44235031)参观