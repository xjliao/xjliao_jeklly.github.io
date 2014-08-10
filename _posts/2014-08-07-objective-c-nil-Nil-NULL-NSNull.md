---
layout: post
title: "Objective-c nil Nil NULL NSNull"
categories:
- Objective-c
tags:
- Objective-c


--- 
[原文地址:http://nshipster.cn/nil/](http://nshipster.cn/nil/)

理解“不存在”的概念不仅仅是一个哲学的问题，也是一个实际的问题。我们是有形宇宙的居民，而原因在于逻辑宇宙的存在不确定性。作为一个逻辑系统的物理体现，电脑面临一个棘手的问题，就是如何用存在表达不存在.

在Objective－C中，有几个不同种类的不存在。这样做的原因要追溯到一个频繁提及的NSHipster，讲解Objective-C如何在C的程序范例以及由Smalltalk启发的面向对象的范例中架起桥梁的。

C用0来作为不存在的原始值，而NULL作为指针(这在指针环境中相当于0)。

Objective-C在C的表达不存在的基础上增加了nil。nil是一个指向不存在的对象指针。虽然它在语义上与NULL不同，但它们在技术上是相等的。

在框架层面，Foundation定义了NSNull，即一个类方法+null，它返回一个单独的NSNull对象。NSNull与nil以及NULL不同，因为它是一个实际的对象，而不是一个零值。

另外，在Foundation/NSObjCRuntime.h中，Nil被定义为指向零的类指针。这个nil的鲜为人知的大写的表兄并不常常出现，但它至少值得注意。

关于 nil 的一些事

刚被分配的NSObject的内容被设置为0。也就是说那个对象所有的指向其他对象的指针都从nil开始，所以在init方法中设置self.(association) = nil之类的表达是没有必要的。

也许nil最显著的行为是，它虽然为零，仍然可以有消息发送给它。

在其他的语言中，比如C++，这样做会使你的程序崩溃，但在Objective-C中，在nil上调用方法返回一个零值。这大大的简化了表达，因为它避免了在使用nil之前对它的检查：

{% highlight objective-c linenos %}
// 举个例子，这个表达...
if (name != nil && [name isEqualToString:@"Steve"]) { ... }
// …可以被简化为：
if ([name isEqualToString:@"steve"]) { ... }
{% endhighlight %}

了解nil如何在Objective-C中工作可以让你将这个便利变成一个功能，而不是潜伏在你的应用中的bug。要确保避免当nil值不需要的情况，要么通过检查或者提前返回来安静的失败，或者通过增加一个NSParameterAssert来抛出异常。

NSNull：有作没有

NSNull在Foundation和其它框架中被广泛的使用，以解决如NSArray和NSDictionary之类的集合不能有nil值的缺陷。你可以将NSNull理解为有效的将NULL或者nil值封装boxing，以达到在集合中使用它们的目的：

{% highlight objective-c linenos %}
NSMutableDictionary *mutableDictionary = [NSMutableDictionary dictionary];
mutableDictionary[@"someKey"] = [NSNull null]; // Sets value of NSNull singleton for `someKey`
NSLog(@"Keys: %@", [mutableDictionary allKeys]); // @[@"someKey"]
{% endhighlight %}

总的来说，这里的四个表达没有的值是每个Objective-C程序员都应该知道的：
<table  width=600px>
	<tr>
		<th>标志</th>
		<th>值</th>
		<th>含义</th>
	</tr>
	<tr align='center'>
		<td>NULL</td>
		<td>(void *)0</td>
		<td>C指针的字面零值</td>
	</tr>
	<tr align='center'>
		<td>nil</td>
		<td>(id)0</td>
		<td>Objective-C对象的字面零值</td>
	</tr>
	<tr align='center'>
		<td>Nil</td>
		<td>(Class)0</td>
		<td>Objective-C类的字面零值</td>
	</tr>
	<tr align='center'>
		<td>NSNull</td>
		<td>[NSNull null]</td>
		<td>用来表示零值的单独的对象</td>
	</tr>		
</table>