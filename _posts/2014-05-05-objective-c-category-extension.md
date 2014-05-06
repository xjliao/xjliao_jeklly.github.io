---
layout: post
title: "Objective-C类别与类扩展"
categories:
- Objective-C
tags:
- Objective-C


---
##1.类别||类目||分类，爱咋叫咋叫
>&nbsp;&nbsp;为现有类添加新得方法（不能添加实例变量，需要添加实例变量，就要进行使用扩展），但不类定义进行方法得添加，也不是对类进行继承，添加新方法

Fraction.h

{% highlight objective-c linenos %}
#import <Foundation/Foundation.h>

@interface Fraction : NSObject

- (void)add;

@end
{% endhighlight %}

Frachtion.m

{% highlight objective-c linenos %}
#import <Foundation/Founation.h>
#import "Fraction.h"

@implementation Fraction

- (void)add
{
	NSLog(@"add");
}

@end
{% endhighlight %}
 
Fraction+Category.h

{% highlight objective-c linenos %}
#import "Fraction.h"

@interface Fraction (Category)

- (void)sub;
- (void)mul;
- (void)div;

@end
{% endhighlight %}

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

测试:main.m
{% highlight objective-c linenos %}
#import <Foundation/Foundation.h>
#import "Fraction.h"
#import "Fraction+Category.h"

int main(int argc, const char *argv[])
{
	@autoreleasepool {
		Fraction *f = [[Fraction alloc] init];

		[f add];
		[f sub];
		[f mul];
		[f div];
	}

	return 0;
}
{% endhighlight %}
运行结果

```console
2014-05-06 11:13:48.974 JF[541:303] add
2014-05-06 11:13:48.976 JF[541:303] sub
2014-05-06 11:13:48.976 JF[541:303] mul
2014-05-06 11:13:48.977 JF[541:303] div
Program ended with exit code: 0
```
  
  
##2.类扩展
>&nbsp;&nbsp;可以说是匿名得类别  
@interface Fraction ()
但他可以添加实例变量，类别不可以

ExtensionTest.h
{% highlight objective-c linenos %}
#import <Foundation/Foundation.h>


@interface ExtensionTest : NSObject
{
    int _s1, _s2;
}

@property int s1, s2;

- (void)printS;

@end
{% endhighlight %}


类扩展文件：ExtensionTest_Et.h，  
也可以在定义在ExtensionTest.m文件中。

{% highlight objective-c linenos %}
#import "ExtensionTest.h"

@interface ExtensionTest ()
{
    int _et1, _et2;
}

@property int et1, et2;

- (void)printEt;

@end
{% endhighlight %}

ExtensionTest.m

{% highlight objective-c linenos %}
#import "ExtensionTest.h"
#import "ExtensionTest_Et.h"

int s = 100;

@implementation ExtensionTest

@synthesize s1 = _s1, s2 = _s2;
@synthesize et1 = _et1, et2 = _et2;

- (void)printS
{
    extern int s;
    [self setS1: s];
    s = 101;
    
    [self setS2: s];
    NSLog(@"s1 = %i, s1 = %i", [self s1], [self s2]);
}

- (void)printEt
{
    NSLog(@"et1 = %i, et2 = %i", [self et1], [self et2]);
}

@end
{% endhighlight %}

测试:main.m

{% highlight objective-c linenos %}
#import <Foundation/Foundation.h>
#import "ExtensionTest.h"
#import "ExtensionTest_Et.h"

typedef ExtensionTest et;

int main(int argc, const char * argv[])
{

    @autoreleasepool {
        
        et *et = [[ExtensionTest alloc] init];
        
        [et printS];
        
        //扩展得实例变量和方法
        [et setEt1: 134];
        [et setEt2: 135];
        [et printEt];
        
    }
    return 0;
}
{% endhighlight %}

结果：

```console
2014-05-06 15:57:34.224 JF[1923:303] s1 = 100, s1 = 101
2014-05-06 15:57:34.224 JF[1923:303] et1 = 134, et2 = 135
Program ended with exit code: 0
```

