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
常见错误
{% highlight console %}
EnvironmentError: /usr/local/bin/mysql_config not found  
{% endhighlight %}

解决方案:  
打开当前目录下的site.cfg, 并去掉#mysql\_config = /usr/local/bin/mysql\_config,
改为安装的mysql目录下的mysql\_config,
我的目录:/usr/local/mysql/bin/mysql\_config

{% highlight console %}
 Referenced from: /Library/Python/2.7/site-packages/MySQL_python-1.2.4-py2.7-macosx-10.9-intel.egg/_mysql.so
  Reason: image not found
{% endhighlight %}
解决方案:  
{% highlight console %}
sudo ln -s /usr/local/mysql/lib/libmysqlclient.18.dylib /usr/lib/libmysqlclient.18.dylib
{% endhighlight %}




