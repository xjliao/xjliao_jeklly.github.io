---
layout: post
title: "Objective-C分类与扩展"
categories:
- Objective-C
tags:
- Objective-C


---
##1.分类  
对一个类添加新得方法（但是不能添加新得实例变量）

头文件：Fraction.h
{% highlight objective-c linenos %}
#import <Foundation/Foundation.h>

@interface Fraction : NSObject
{
	int _numerator, _denominator;
}

@property int numerator, denominator;

- (void)add;

@end
{% endhighlight %}

实现文件：Frachtion.m
{% highlight objective-c linenos %}
#import <Foundation/Founation.h>
#import "Fraction.h"

@implementation Fraction

@synthesize int numerator = _numerator, denominator = _denominator;

- (void)add
{
	NSLog(@"add");
}

@end
{% endhighlight %}

分类头文件  
Fraction+Category.h
{% highlight objective-c linenos %}
#import "Fraction.h"

@interface Fraction (Category)

- (void)sub;
- (void)mul;
- (void)div;

@end
{% endhighlight %}
分类实现文件
Fraction+Category.m
{% highlight objective-c linenos %}
#import <Foundation/Foundation.h>
#import "Fraction+Category.h"

@implementation Fraction (Category)

- (void)sub
{
	NSLog(@"sub");
}

- (void)mul
{
	NSLog(@"mul");
}

- (void)div
{
	NSLog(@"div");
}

@end
{% endhighlight %}

测试
{% highlight objective-c linenos %}
#import <Foundation/Foundation.h>
#import "Fraction.h"

int main(int argc, const char *argv[])
{
	@autoreleasepool {
		Fraction *f = [[Fraction alloc] init];

		[f add];
		[f sub];
		[f mul];
		[f div];
	}

	return 0；
}
{% endhighlight %}

##2.扩展