---
layout: post
title: "route命令使用"
categories:
- Linux
tags:
- Linux


---

说明：route命令是打印和操作ip路由表

描述：route操作基于内核ip路由表，它的主要作用是创建一个静态路由让指定一个主 机或者一个网络通过一个网络接口，如eth0。当使用"add"或者"del"参数时，路由表被修改，如果没有参数，则显示路由表当前的内容。
{% highlight java linenos %}
	参数说明：add:添加一条新路由。
          del:删除一条路由。
          -net:目标地址是一个网络。
          -host:目标地址是一个主机。
          netmask:当添加一个网络路由时，需要使用网络掩码。
          gw:路由数据包通过网关。注意，你指定的网关必须能够达到。
          metric：设置路由跳数。
{% endhighlight %}
实例：
{% highlight java linenos %}
1、route add -net 192.168.2.0 netmask 255.255.255.0 dev eth0
{% endhighlight %}
添加一条到达192.168.2.0网络的路由，指定网络掩码为255.255.255.0,数据包通过网络接口eth0。
{% highlight java linenos %}
2、route add -net 192.57.66.0 netmask 255.255.255.0 gw 192.168.2.1
{% endhighlight %}
添加一条到达192.57.66.0网络的路由，指定网络掩码为255.255.255.0,数据包通过网关地址192.168.2.1。
{% highlight java linenos %}
3、route add -host 192.57.66.200 gw 192.168.2.1
{% endhighlight %}
所有去往192.57.66.200主机的数据包发往网关地址192.168.2.1。
{% highlight java linenos %}
4、route add default gw 192.168.1.1
{% endhighlight %}
添加一条默认网关，所有的数据包将被转发到192.168.1.1。

路由表内容说明：
查看路由
{% highlight java linenos %}
public class {
	public static void main(String[] args) {
		System.out.println('Hello World');
	}
}	
{% endhighlight %}
Destination：目标网络或主机。
Gateway：网关地址。
Genmask：目标网络的网络掩码。"255.255.255.255"表示一个主机。"0.0.0.0"表示网关。
Flags：标记。	
      U、路由被启用。	
      H、目标是一个主机		
      G、使用网关。	
