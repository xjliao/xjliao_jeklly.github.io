---
layout: post
title: "Objective-c block"
categories:
- Objective-c
tags:
- Objective-c


--- 
[原文地址:http://fuckingblocksyntax.com/](http://fuckingblocksyntax.com/)

How Do I Declare A Block in Objective-C?  

{% highlight objective-c linenos %}
//局部量
returnType (^blockName)(parameterTypes) = ^returnType(parameters) {...};

//property属性
@property (nonatomic, copy) returnType (^blockName)(parameterTypes);

//方法参数
- (void)someMethodThatTakesABlock:(returnType (^)(parameterTypes))blockName;

//作为参数被方法调用
[someObject someMethodThatTakesABlock: ^returnType (parameters) {...}];

//typdef
typedef returnType (^TypeName)(parameterTypes);
TypeName blockName = ^returnType(parameters) {...};
{% endhighlight %}.