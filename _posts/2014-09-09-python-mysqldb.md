---
layout: post
title: "python mysql"
categories:
- python 
tags:
- python 

--- 

下载地址  
使用1.2.4版本  
<http://pypi.python.org/pypi/MySQL-python/1.2.4>

{% highlight console %}
xjl:MySQL-python-1.2.4 xjliao$ python ./setup.py install
{% endhighlight %}

可能会出现这个错误  
EnvironmentError: /usr/local/bin/mysql_config not found  

解决方案:  
打开当前目录下的site.cfg, 并去掉#mysql\_config = /usr/local/bin/mysql_config,
改为安装的mysql目录下的mysql_config,
我的目录:/usr/local/mysql/bin/mysql_config
