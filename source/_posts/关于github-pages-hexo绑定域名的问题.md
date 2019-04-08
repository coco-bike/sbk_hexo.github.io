---
title: 关于github-pages-hexo绑定域名的问题
copyright: true
date: 2019-04-08 15:59:59
categories: hexo
tags: [hexo,github]
---
<blockquote class="blockquote-center">生活有度，人生添寿。 —— 书摘</blockquote>

<!-- more -->
### 1.创建github pages
可以参考官方文档

### 2.注册域名
到阿里云或者其他的域名购买喜欢的域名，个人的话不推荐买.com的域名

### 3.github pages上绑定
在点击设置
{% asset_img 2019-01-22.jpg  图1 %}
输入你购买的域名
{% asset_img 2019-01-22-2.jpg  图2 %}
### 4.域名解析
添加如下2个，注意填写自己的github-pages地址
{% asset_img 2019-01-22-1.jpg 图3 %}
### 5.hexo配置
在hexo的source文件夹下添加个CNAME文件，写入你的域名 例如："abc.cn"
这样的话www,不加头等其他方式的访问都支持
在hexo根目录下配置url,如果不进行这一步的配置的话，会遇到样式加载不出来404的错误，因为你的url设置还是以前的githubpages的地址
{% asset_img 2019-01-22-3.jpg 图4 %}
###补充一点
我后来自己又重新配了下域名被坑了，在hexo的source文件夹下要添加个CNAME文件,并在文件写入你的域名
{% asset_img 2019-04-08-02.png 图5 %}
{% asset_img 2019-04-08-01.png 图6 %}

