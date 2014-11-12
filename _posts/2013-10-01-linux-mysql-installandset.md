---
layout: post
title: "Linux Mysql yum 安装方式及配置"
categories:
- Linux
- Mysql
tags:
- Linux 
- Mysql

--- 

1.安装mysql
{% highlight console %}
[xjliao@li539-59 ~] sudo yum -y install mysql-server 
[xjliao@li539-59 ~] sudo yum -y install mysql-devel
[xjliao@li539-59 ~] sudo yum -y install mysql 
{% endhighlight %}

2.修改root用户默认密码
{% highlight console %}
[xjliao@li539-59 ~]$ mysqladmin -u root password 'root'
{% endhighlight %}

3.开启远程连接权限
{% highlight console %}
[xjliao@li539-59 ~]$ mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 22
Server version: 5.1.73 Source distribution 
Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> grant all privileges on *.* to root@'%' identified by '1';
mysql> flush privileges;
{% endhighlight %}

4.Mysql 配置/etc/my.cnf
{% highlight console %}
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
user=mysql
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
default-character-set = utf8 <-添加这行,设置字符集

[mysql]
default-character-set = utf8  <-添加这行,设置字符集

[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
{% endhighlight %}

5.设置mysql服务开机启动
{% highlight console %}
[xjliao@li539-59 ~]$ sudo chkconfig mysqld on
[xjliao@li539-59 ~]$ chkconfig --list mysqld
[xjliao@li539-59 ~]$ sudo service mysqld restart
{% endhighlight %}

