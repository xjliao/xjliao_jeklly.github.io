---
layout: post
title: 'jekyll+github 搭建博客'
categories:
- ruby
- jekyll
- github
tags:
- ruby
- jekyll
- github

---

#一、gitHub
## 注册github账户
<https://github.com>, 这个不用教吧！
## 创建Repository
创建的repository名称格式：
{% highlight sh %}
username.github.io  //用户名.github.io
{% endhighlight %}
![create-repository](/assets/jekyll-github/create-repository.jpg)

#二、jekyll
jekyll是一个静态网站生成器

官网：<http://jekyllrb.com/>,中文网站：<http://jekyllcn.com/>

## jekyll安装
####jekyll安装环境:
- rvm
- Ruby
- RubyGems
- Linux，Unix，or Mac OS X

1)、rvm 安装

rvm是ruby的管理器
{% highlight sh %}
$ curl -L https://get.rvm.io | bash -s stable
{% endhighlight %}
安装期间，需要root权限，如果需要本终端生效的话，执行下列命令，新开终端无需执行。
{% highlight sh %}
$ source ~/.rvm/scripts/rvm
{% endhighlight %}
检查是否安装成功
{% highlight sh %}
$ rvm -v
{% endhighlight %}
如果有显示rvm版本信息，则安装成功

2)、ruby安装
{% highlight sh %}
$ rvm install 1.9.3 //安装版本为1.9.3
{% endhighlight %}
可能时间比较长,安装完成测试
{% highlight sh %}
$ rvm -v
{% endhighlight %}
安装完后，RubyGems同时也安装完成
测试
{% highlight sh %}
$ gem -v
{% endhighlight %}
由于RubyGems默认源速度慢，修改为淘宝源
{% highlight sh %}
$ gem source -a http://ruby.taobao.org
$ gem source -r https://rubygems.org/
{% endhighlight %}
###jekyll安装
{% highlight sh %}
$ gem install jekyll
{% endhighlight %}
###测试下
{% highlight sh %}
$ jekyll -v
{% endhighlight %}
###新建站点
{% highlight sh %}
$ jekyll new blog
{% endhighlight %}
将在本目录生成目录名为blog，里面为jekyll所需文件框架
###测试新建站点
{% highlight sh %}
$ cd ./blog && jekyll server
{% endhighlight %}
可以通过访问localhost:4000验证启动是否成功。
如果看到下面这样的页面，就表示成功了！
![jekyll-server-succ](/assets/jekyll-github/jekyll-server-succ.png)

## jekyll主题更换
官方推荐主题的：<https://github.com/jekyll/jekyll/wiki/Sites>
从github上面把自己喜欢的git clone下来就行了！
jekyll默认主题和推荐主题可能不符合你的口味，你可以使用其他主题，如：[jekyl bootstrap](http://jekyllbootstrap.com/)、[jekyll themes](http://jekyllthemes.org/)
不过我不大喜欢！我的是[89ao.info](http://89ao.info/)的主题。
github地址:<https://github.com/89ao/89ao.github.io>
接下来，我就这个主题，github部署jekyll

## github部署jekyll
###项目克隆到本地
{% highlight sh %}
git clone git@github.com:89ao/89ao.github.io.git
{% endhighlight %}
默认在当前目录创建和Repository名称一样的目录，如果要修改，则在后面加上目录名即可
{% highlight sh %}
git clone git@github.com:89ao/89ao.github.io.git xjliao.github.io
{% endhighlight %}

我们看下xjliao.github.io目录里的_config.yml，这是我修改之后的
{% highlight yaml %}
markdown: redcarpet
permalink: /:title/
url: http://xjliao.me 
name: xjliao.me 
author: xjliao
qq: 909012329 
email: lxj990@gmail.com 
duoshuo: xjliao 
weibo: lxj990@gmail.com 
github: xjliao
pygments: true
safe: true
paginate: 30
about: ""
{% endhighlight %}
主要是些配置信息，可以通过如{{ site.url }}在html、markdown文件里直接引用，稍微解释下配置信息：
>markdown 渲染引擎，有redcarpet，rdiscount等，要使用，你确定你已经安装过。
安装
{% highlight sh %}
$ gem install redcarpet
$ gem install rdiscount
{% endhighlight %}
注意冒号后面必须要加一个空格，否则启动会报错

>permalink 永久链接 在生成文章事，是按照这个设置的格式生成目录和文件

>url 网址

>pygments 是否启用pygments代码高亮

>safe 安全连接

>paginate 分页，一页显示多少条文章

其他详细，[jekyll documentation](http://jekyllcn.com/docs/home/)

### 推送到xjliao.github.io
{% highlight sh %}
$ git remote add xjliao git@github.com:xjliao/xjliao.github.io.git //添加自己的repository并别名为xjliao
$ git push xjliao master //推送到自己的主线分支版本
{% endhighlight %}

10分钟左右访问 [xjliao.github.io](xjliao.github.io) 就ok了
