---
layout: post
title: "Error loading either pysqlite2 or sqlite3 modules (tried in that order): No module named _sqlite3"
categories:
- Centos
- Python
- Django
tags:
- Centos
- Python
- Django

--- 
在安装或升级python之前,安装
{% highlight console %}
[xjliao@li539-59 ~]$ sudo yum install sqlite-devel
{% endhighlight %}
