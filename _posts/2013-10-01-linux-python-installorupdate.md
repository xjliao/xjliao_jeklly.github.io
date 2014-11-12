---
layout: post
title: "Linux python 安装或者升级"
categories:
- Python
- Linux
tags:
- Python
- Linux 

--- 

在这里使用最新的2.x版本 在官网下载源码包    
下载、解压、安装、拷贝执行文件、如果是升级的话，先备份下旧的，然后覆盖
{% highlight console %}
[xjliao@li539-59 ~]$ python --version
[xjliao@li539-59 ~]$ wget https://www.python.org/ftp/python/2.7.8/Python-2.7.8.tar.xz
[xjliao@li539-59 ~]$ tar -Jxvf ./Python-2.7.8.tar.xz
[xjliao@li539-59 ~]$ cd ./Python-2.7.8
[xjliao@li539-59 ~]$ sudo ./configure --prefix=/usr/local/python-2.7.8/ && sudo make && sudo make install
[xjliao@li539-59 ~]$ sudo cp /usr/bin/python /usr/bin/python_bak
[xjliao@li539-59 ~]$ sudo cp /usr/local/python-2.7.8/bin/python /usr/bin/python
[xjliao@li539-59 ~]$ python --version
{% endhighlight %}

如果是用得redhat centos之类得Linux操作系统,可能会影响到yum的使用, 需要修改

{% highlight console %}
[xjliao@li539-59 ~] sudo vim /usr/bin/yum
#!/usr/bin/python2.6
{% endhighlight %}

修改为原来的版本即可
