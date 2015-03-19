---
layout: post
title: "macos install lua"
categories:
- Lua
tags:
- Lua


---
##1.源码安装
需要使用make命令, 安装xcode即可解决  
{% highlight lua %}
curl -R -O http://www.lua.org/ftp/lua-5.3.0.tar.gz
tar -zxvf ./lua-5.3.0.tar.gz
cd ./lua-5.3.0
make macosx
make test
sudo make install
lua
{% endhighlight %}

##2.二进制文件
直接下载,解压,设置path或者直接在解压文件夹内执行命令,这里就不演示了
