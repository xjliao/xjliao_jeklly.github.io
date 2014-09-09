---
layout: post
title: "Objective-c @property"
categories:
- Objective-c
tags:
- Objective-c


--- 

@property
参数
nonatomic 非原子操作，没有多线程情况下使用此属性，提高性能

atomic  默认值，原子安全操作

copy  复制，只能用在实现了NSCopying协议的类型上，比如推荐NSString,NSArray, NSDictonary使用此属性

assign 简单赋值，定义为基本数据类型、NSInteger、CGRect等

retain 引用计数加1

strong  强引用，在开启arc下和retain相似

weak  弱引用，在开启arc时和assign类似

readonly  不生成setter方法

readwrite  默认值 生成getter/setter方法

getter 设置getter方法名  getter=getInfo

stter 设置setter方法名  setter=setInfo:

{% highlight objective-c linenos %}

{% endhighlight %}