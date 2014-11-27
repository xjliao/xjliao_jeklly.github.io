---
layout: post
title: 'jekyll+github 搭建博客'
categories:
- Ruby
- Jekyll
- Github
tags:
- Ruby
- Jekyll
- Github

---

#一、gitHub
## 注册github账户
<https://github.com>
## 创建Repository
创建的repository,名称格式:  
用户名.github.io
{% highlight console %}
username.github.io
{% endhighlight %}
![create-repository]({{ site.tc }}/img/jekyll-github/create-repository.jpg)

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
{% highlight console%}
$ curl -L https://get.rvm.io | bash -s stable
{% endhighlight %}
安装期间，需要root权限，如果需要本终端生效的话，执行下列命令，新开终端无需执行。
{% highlight console %}
$ source ~/.rvm/scripts/rvm
{% endhighlight %}
检查是否安装成功
{% highlight console %}
$ rvm -v
{% endhighlight %}
如果有显示rvm版本信息，则安装成功

2)、ruby安装   
安装版本为1.9.3 
{% highlight console %}
$ rvm install 1.9.3
{% endhighlight %}
可能时间比较长,安装完成测试
{% highlight console %}
$ rvm -v
{% endhighlight %}
安装完后，RubyGems同时也安装完成
测试
{% highlight console %}
$ gem -v
{% endhighlight %}
由于RubyGems默认源速度慢，修改为淘宝源
{% highlight console %}
$ gem source -a http://ruby.taobao.org
$ gem source -r https://rubygems.org/
{% endhighlight %}
###jekyll安装
{% highlight console %}
$ gem install jekyll
{% endhighlight %}
###测试下
{% highlight console %}
$ jekyll -v
{% endhighlight %}
###新建站点
{% highlight console %}
$ jekyll new blog
{% endhighlight %}
将在本目录生成目录名为blog，里面为jekyll所需文件框架
###测试新建站点
{% highlight console %}
$ cd ./blog && jekyll server
{% endhighlight %}
可以通过访问localhost:4000验证启动是否成功。
如果看到下面这样的页面，就表示成功了！
![jekyll-server-succ]({{ site.tc }}/img/jekyll-github/jekyll-server-succ.png)

## jekyll主题更换
官方推荐主题的：<https://github.com/jekyll/jekyll/wiki/Sites>
从github上面把自己喜欢的git clone下来就行了！
jekyll默认主题和推荐主题可能不符合你的口味，你可以使用其他主题，如：[jekyl bootstrap](http://jekyllbootstrap.com/)、[jekyll themes](http://jekyllthemes.org/)
不过我不大喜欢！我的是[89ao.info](http://89ao.info/)的主题。
github地址:<https://github.com/89ao/89ao.github.io>
接下来，我就这个主题，github部署jekyll

## github部署jekyll
###项目克隆到本地
{% highlight console %}
$ git clone git@github.com:89ao/89ao.github.io.git
{% endhighlight %}
默认在当前目录创建和Repository名称一样的目录，如果要修改，则在后面加上目录名即可
{% highlight console %}
$ git clone git@github.com:89ao/89ao.github.io.git xjliao.github.io
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
{% highlight console %}
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
添加自己的repository并别名为xjliao
推送到自己的主线分支版本,注意使用ssh，需要把本地ssh公钥拷贝至github
{% highlight console %}
$ git remote add xjliao git@github.com:xjliao/xjliao.github.io.git
$ git push xjliao master
{% endhighlight %}

10分钟左右访问<http://xjliao.github.io> 就ok了

## Pygments 语法高亮
[下载地址](https://pypi.python.org/pypi/Pygments 'download') 支持100多种[语言](http://pygments.org/languages/) 样式[demo](http://pygments.org/demo/)

安装

```console
$ wget https://pypi.python.org/packages/source/P/Pygments/Pygments-1.6.tar.gz#md5=a18feedf6ffd0b0cc8c8b0fbdb2027b1
$ tar -zxvf ./Pygments-1.6.tar.gz
$ cd ./Pygments-1.6
$ python setup.py install
$ pygmentize -S monokai -f html -a pre > pygments.css
```
-S 指定样式 -f 格式的类型 -a 一个特定的格式

将生成的pygments.css 拷贝到jekyll 目录(可以里面的随意目录)，我就发放到和jekyll主题的css样式文件一个目录吧，然后修改 default.html 默认模板，删除默认高亮样式，加入pygments.css样式。
这个jekyll的主题样式因为与pygments有些冲突，删除了下面两行就可以了

```css
pre,code{font-family:Menlo,"Andale Mono",Consolas,"Courier New",Monaco,monospace;font-size:13px;}
code{background-color:#D0D0D0;}
```

## github 绑定域名
在jekyll根目录新增或修改CNAME文件，里面添加上你的域名

```text
xjliao.me
```

## 域名注册 DNS设置 A记录绑定
推荐号称全球最大的域名注册商[godaddy](http://www.godaddy.com/)上购买，支持支付宝付款，有较多优惠码，就算没有也比国内的便宜点！
购买注册就不多讲了，主要讲下指定A记录和DNS解析

进入domains管理界面选择域名设置DNS
![DNSSET]({{ site.tc }}/img/jekyll-github/godaddy-set-dns.png)
添加DNS，这里使用的是dnspod的解析服务
![DNSSETPOD]({{ site.tc }}/img/jekyll-github/godaddy-set-dns-dnspod.png)
然后登陆dnspod添加下要设置A记录的域名
![DNSSETPOD]({{ site.tc }}/img/jekyll-github/dns-pod-add-domain.png)
点击域名进入A记录设置界面
![DNSSETPOD]({{ site.tc }}/img/jekyll-github/dns-pod-add-a-record.png)
主机记录填写值  
@ 直接解析主域名 xjliao.me、wwww 解析后的域名为 www.xjliao.me、* 泛解析，匹配其他所有域名 *.xjliao.me

记录类型为A记录，记录值为usrename.github.io的ip，ping下就知道了。
然后，就没有然后了，奥，对了，要保存！稍等片刻即可了！  

访问username.github.io 访问指向了绑定的域名 username.github.com也可以！就这样了

其他参考资料  
[jekyll](http://jekyllrb.com/):<http://jekyllrb.com/>  
[markdown](http://wowubuntu.com/markdown/):<http://wowubuntu.com/markdown/>  
[pygments](http://pygments.org/):<http://pygments.org/>
