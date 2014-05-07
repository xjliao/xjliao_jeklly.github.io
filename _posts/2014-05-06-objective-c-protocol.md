---
layout: post
title: "Objective-C协议与代理"
categories:
- Objective-C
tags:
- Objective-C


---
##协议
>&nbsp;&nbsp;协议是多个类共享得一个方法列表，协议中列出得方法没有  
相应得实现，由其他类进行实现。类似java中得接口（interface）  

定义协议：Prototol.h 

{% highlight objective-c linenos %}
#import <Foundation/Foundation.h>

@protocol Protocol

@required
- (void)print1;
- (void)print2;

@optional
- (void)print3;
- (void)print4;

@end
{% endhighlight %}
>@protocoal：定义协议  
@required：必须要现实得方法  
@optional：可选实现得方法  
协议也可以继承其他协议，在协议名后面加上<协议名>  
类别也可以使用协议

实现协议:  
TestProtocol1.h

{% highlight objective-c linenos %}
#import <Foundation/Foundation.h>
#import "Protocol.h"

@interface TestProtocol1 : NSObject <Protocol, NSObject>

@end

{% endhighlight %}
>在<>里面放入要实现得协议名，实现多个协议用“,”分隔，无需在定义协议中得方法了，直接在实现文件中实现即可  
 
TestProtocol1.m

{% highlight objective-c linenos %}
#import "TestProtocol1.h"

@implementation TestProtocol1

- (void)print1
{
    NSLog(@"p1");
}

- (void)print2
{
    NSLog(@"p2");
}
@end
{% endhighlight %}
>在这没有实现协议得可选方法  

测试

{% highlight objective-c linenos %}
#import <Foundation/Foundation.h>
#import "TestProtocol.h"

int main(int argc, const char * argv[])
{

    @autoreleasepool {
        
        TestProtocol *tp = [[TestProtocol alloc] init];
        [tp print1];
        [tp print2];
        
    }
    return 0;
}
{% endhighlight %}

结果

```console
2014-05-06 22:51:23.457 JF[2843:303] p1
2014-05-06 22:51:23.458 JF[2843:303] p2
Program ended with exit code:
```
##代理
>协议也是种两个类之间得接口定义。定义了协议得类可以看作是将协议定义得方法代理给了实现他们得类。这样类得定义可以更为通用，因为具体得动作由代理类来实现。

##非正式协议
>&nbsp;&nbsp;非正式协议实际上是一个分类，列出了一组方法但没有实现他们。每个人都继承相同得根对象，因此，非正式分类通常是为根类定义得，有时，非正式协议也成为抽象（abstract）协议  

伪代码

{% highlight objective-c linenos %}
interface NSObject (NSCOMparisonMethods)

- (BOOL)isEqualTo:(id)object;
- (BOOL)isLessThanOrEqualTo:(id)object;
- (BOOL)isLessThan:(id)object;

@end
   
{% endhighlight %}
>伪代码为NSObject类定义了一个名为NSComparisonMethods得分类。这项非正式协议列出了3个方法，可以将它们实现为协议得一部分。非正式协议实际上仅仅是一个名称下得一组方法。这在文档说明和模块化方法时，可能有所帮助。
<p/>
声明非正式协议得类，可以不实现这些方法  
声明正式协议得类必须要实现协议的方法  

>Objective-c 2.0 在声明协议中@required，@optional 来取代正式协议和非正式协议。

