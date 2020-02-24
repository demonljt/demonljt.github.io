---
layout: post
title: '为你的jekyll博客添加gitalk评论插件吧！'
subtitle: 'jekyll进阶'
date: 2020-02-24
categories: 记录
cover: 'https://images.pexels.com/photos/3342697/pexels-photo-3342697.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940'
tags: gitalk jekyll 博客 评论 历程
---
首先到你的github账户设置中的最下面开发者设置中申请OAuth Apps，申请后会有clientID和clientSecret两个很长的数，申请时需要注意的是：Authorization callback URL的填写 ，<font color="red">回调地址,一般为你博客的主页地址,但如果绑定域名的话，必须写你自己的域名,不然会很多次回调,报错。</font>


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224172125615.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIzNTAzMQ==,size_16,color_FFFFFF,t_70)

之后在你的博客的相应位置，一般为在每个 文章下面添加，所以在/_layout/post.html中 添加以下代码  ：



首先在HTML头引入以下js和CSS，如果写在下面容易和你的其他冲突。此方式为2020年2月可用。
```HTML
<!-- Link Gitalk 的支持文件 -->
<link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
<script src="https://unpkg.com/gitalk@latest/dist/gitalk.min.js"></script> 
```
将以下代码放置到需要显示评论插件的地方
```HTML
  <!-- Gitalk 评论 start  -->
  <div id="gitalk-container"></div>
      <script type="text/javascript">
      var title = location.pathname.substr(0, 50);//截取路径的前50个字符作为标识符
      var gitalk = new Gitalk({

      // gitalk的主要参数
          clientID: `这里插入你申请到的clientID `,
          clientSecret: `这里插入你申请到的clientSecret`,
          repo: `你的博客仓库`,
          owner: '就是你了',
          admin: ['你的github账户名，不是昵称'],
          id: title,//因为这里不能超过50字符，所以做了处理，当超过50时自动截取前50
     
      });
      gitalk.render('gitalk-container');
      </script>
  <!-- Gitalk end -->
```
通过 以上方式 可以在你的博客相应位置添加gitalk评论，即为 每篇 文章新建一个issues，这时候每篇文章都需要初始化，但是当你 浏览器登陆你的帐户时可以在预览网页的时候直接初始化建立新的issue。关于报错处理部分其他大佬讲的很好。