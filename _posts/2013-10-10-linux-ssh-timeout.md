---
layout: post
title: 'ssh连接闲置超时，被踢出， 解决方案'
categories:
- Linux
tags:
- Linux

---
方案一：在客户端设置  
在客户端电脑上编辑（需要root权限）/etc/ssh/  ssh_config，并添加如下一行:	    
  
ServerAliveInterval 60  
此后该系统里的用户连接SSH时，每60秒会发一个KeepAlive请求，避免被踢。  
  
方案二：在服务器端设置  
如果有相应的权限，也可以在服务器端设置，即编辑/etc/ssh/  sshd_config，并添加：  
  
ClientAliveInterval 300  
ClientAliveCountMax 3   
按以上的配置的含义就是 SSH每5分钟秒向客户端发送一次心跳 ，连续发送3次没有收到客户端道应答，则断开客户端。系统默认是ClientAliveInterval为15，ClientAliveCountMax 为3，所以SSH会在45秒之后断开。  
保存设置并重载配置文件  
{% highlight console %}
sudo /etc/init.d/sshd restart
{% endhighlight %}