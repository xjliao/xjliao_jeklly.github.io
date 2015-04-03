---
layout: post
title: "python执行shell命令"
categories:
- Python
tags:
- Python 

--- 

##Python 执行shell命令主要有四种方式
##1、os.system()
这个函数来执行shell命令  
{% highlight python %}
os.system('ls')
{% endhighlight %}  
这个方法得不到shell命令的输出

##2、popen()
这个方法能得到命令执行后的结果是一个字符串，要自行处理才能得到想要的信息。
{% highlight python %}
import os
str = os.popen("ls").read()
a = str.split("\n")
for b in a:
	print b
这样得到的结果与第一个方法是一样的。
{% endhighlight %}

##3、commands
可以很方便的取得命令的输出（包括标准和错误输出）和执行状态位
{% highlight python %}
import commands
a,b = commands.getstatusoutput('ls')
a是退出状态
b是输出的结果。
import commands
a,b = commands.getstatusoutput('ls')
print a
0
print b
anaconda-ks.cfg
install.log
install.log.syslog
commands.getstatusoutput(cmd)返回（status,output)
commands.getoutput(cmd)只返回输出结果
commands.getstatus(file)返回ls -ld file 的执行结果字符串，调用了getoutput，不建议使用这个方法。  
{% endhighlight %}

##4、subprocess
使用subprocess模块可以创建新的进程，可以与新建进程的输入/输出/错误管道连通，并可以获得新建进程执行的返回状态。使用subprocess模块的目的是替代os.system()、os.popen*()、commands.*等旧的函数或模块。
{% highlight python %}
import subprocess
1、
subprocess.call(command, shell=True)
会直接打印出结果
2、
subprocess.Popen(command, shell=True)  
也可以是
subprocess.Popen(command, stdout=subprocess.PIPE, shell=True}
{% endhighlight %}  
这样就可以输出结果了。
如果command不是一个可执行文件，shell=True是不可省略的。
shell=True意思是shell下执行command

这四种方法都可以执行shell命令。

