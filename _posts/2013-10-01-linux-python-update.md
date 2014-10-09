---
layout: post
title: "Linux python 安装或者升级"
categories:
- python
- linux
tags:
- python
- linux 

--- 

在这里使用最新的2.x版本 在官网下载源码包    
下载、解压、安装、拷贝执行文件、如果是升级的话，先备份下旧的，然后覆盖
{% highlight console %}
[xjliao@li539-59 ~]$ python --version
[xjliao@li539-59 ~]$ wget https://www.python.org/ftp/python/2.7.8/Python-2.7.8.tar.xz
[xjliao@li539-59 ~]$ tart -xzvf ./Python-2.7.8.tar.xz
[xjliao@li539-59 ~]$ cd ./Python-2.7.8
[xjliao@li539-59 ~]$ sudo ./configure --prefix=/usr/local/python-2.7.8/ && sudo make && sudo make install
[xjliao@li539-59 ~]$ sudo cp /usr/local/python-2.7.8/bin/python /usr/bin/python
[xjliao@li539-59 ~]$ python --version
{% endhighlight %}

