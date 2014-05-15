---
layout: post
title: "Objective-C内存管理"
categories:
- Objective-C
tags:
- Objective-C


---
##先看两句代码
{% highlight objective-c linenos %}
NSArray *myData = [NSArray arrayWithContentsOfFile:@"Person.h"];
myData = [NSArray arrayWithContentsOfFile:@"Person.m"];
{% endhighlight %}
>第1行代码：一个变量myData,从一个文件中得内容读入数组中，内容数组得引用村存储在变量myData中。  
第2行代码：读取另外一个文件，这时myData已经变了，引用到另一个数组了。内容来自第二歌文件中。  
那么第一个数组如何处理？已经不再引用这个数组（当覆盖myData后，数组不再被引用）。第一个数组元素会怎么样？假如你需要重复从成百上千份文件中读取数据，这些数组和他们得元素不会被引用，并且引用不再需要这些数据，如何处理这种情况

##内存管理三种模型：
>1.自动垃圾收集  
>2.手工引用计数和自动释放池  
>3.自动引用计数（ARC)  

1、自动垃圾收集  
此方法只能用在Mac OS X程序开发上,不多介绍  

2、手工管理内存计数 
当创建对象时，初始得引用计数为1。为保证对象得存在，每当创建引用到对象需要为引用数加1，可以给对象发送retain消息：
{% highlight objective-c %}
[myObject retain];
{% endhighlight %}
当不再需要对象时，通过对象发送release消息，为引用计数减1。
{% highlight objective-c %}
[myObject retain];
{% endhighlight %}
当对象得引用计数为0时，系统知道这个对象不再需要使用，所以就可以释放它得内存。通过给对象发送deallo消息发起这个过程。在多数情况下，会使用继承自NSObject得dealloc方法。然而,为了能够释放由对象创建或保持得实例变量或者其他对象，需要覆盖dealloc方法。例如，在你得类中，有一个实例变量为NSArray得对象，并且为他创建了一个数组，当对象销毁时，你需要负责释放数组，在dealloc方法中可以做这些事情。

##一个小例子
Person.h
{% highlight objective-c linenos %}
//
//  Person.h
//  m
//
//  Created by xjliao on 14-5-13.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import <Foundation/Foundation.h>
@class Dog;

@interface Person : NSObject
{
    NSString *_name, *_age, *_sex;
    Dog *_dog;
}

@property NSString *name, *age, *sex;
- (void)printInfo;

- (void)setDog:(Dog *)newDog;
- (Dog *)dog;
@end
{% endhighlight %}

Person.m
{% highlight objective-c linenos %}
//
//  Person.m
//  m
//
//  Created by xjliao on 14-5-13.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import "Person.h"

@implementation Person


@synthesize name = _name, age = _age, sex = _sex;

- (void)printInfo
{
    NSLog(@"姓名：%@, 年龄：%@, 性别：%@", [self name], [self age], [self sex]);
}

- (void)dealloc
{
    NSLog(@"the person %@ died", [self name]);
    [[self dog] release];
    [super dealloc];
}

- (Dog *)dog
{
    return _dog;
}

- (void)setDog:(Dog *)newDog
{
    _dog = [newDog retain];
}

@end
{% endhighlight %}

Dog.h
{% highlight objective-c linenos %}
//
//  Dog.h
//  m
//
//  Created by xjliao on 14-5-14.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface Dog : NSObject
{
    int _id;
    NSString *_name;
}

@property int id;
@property NSString *name;

- (void)printInfo;

@end
{% endhighlight %}

Dog.m

{% highlight objective-c linenos %}
//
//  Dog.m
//  m
//
//  Created by xjliao on 14-5-14.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import "Dog.h"

@implementation Dog

@synthesize id = _id, name = _name;

- (void)printInfo
{
    NSLog(@"This dog id = %i, name = %@", [self id], [self name]);
}

- (void)dealloc
{
    NSLog(@"the dog %@ is died!", [self name]);
    [super dealloc];
}
@end
{% endhighlight %}

测试
{% highlight objective-c linenos %}
//
//  main.m
//  m
//
//  Created by xjliao on 14-5-13.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Person.h"
#import "Dog.h"

int main(int argc, const char * argv[])
{
        
    // insert code here...
    //这句代码可以这样写
    /*@autoreleasepool {
        代码放这里
    }
     */
    NSAutoreleasePool *ap = [[NSAutoreleasePool alloc] init];
    
    Person *p1 = [[Person alloc] init];
    Dog *d1 = [[Dog alloc] init];
    NSLog(@"p1 retain = %lu", [p1 retainCount]);
   
    [p1 setName:@"p1"];
    [p1 setAge:@"25"];
    [d1 setId:1];
    [d1 setName:@"d1"];
    
    //NSLog(@"d1 retain:%lu", [d1 retainCount]);
    
    [d1 printInfo];
    [p1 setDog:d1];
    
    NSLog(@"d1 retain:%lu", [d1 retainCount]);
    
    NSLog(@"p1 retain:%lu", [p1 retainCount]);
    //[p1 release];
    NSLog(@"d1 retain:%lu", [d1 retainCount]);
    
    [d1 release];
    [p1 autorelease];
    
    [ap drain];

    return 0;
}
{% endhighlight %}

3、对象引用和自动释放池
>你可能会编写这样一个方法，先创建一个对象（使用alloc）。然后将它作为方法调用得结果返回。  

{% highlight objective-c linenos %}
- (Dog *)dog
{
    Dog *dog = [[Dog alloc] init];
    [dog setId:1];
    [dog setName:@"d1"];
    
    return dog;
}
{% endhighlight %}

这样就进入一个困境：尽管方法不再使用这个对象，但时并不能释放它，因为需要将这个对象作为方法得返回值。NSAutoreleasePool类创建得目的就是希望解决这个问题，自动释放池可以帮助追踪需要延迟一些时间释放得对象。通过给自动释放池发送drain消息，自动释放池就会被清理，对象也会被释放。  
给对象发送autorelease消息，将一个对象添加得自动释放池维护得对象列表中［result autorelease］。  
在程序中使用来自Foundation、UIKit、AppKit框架得类时，需要先创建一个自动释放池，因为来自这些框架得类会创建并返回自动释放的对象，需要在应用中这样写:  
NSAutoreleasePool *ap = [[NSAutoreleasePool alloc] init];   
  
自动释放池建立后，框架方法会自动添加对象（如数组、字符串、词典、视图、按钮和其他对象）到自动释放池维护列表中。  
使用完自动释放池，需要给它发送drain消息:  
[ap drain];
这将影响到所有发送过autorelease消息并添加到自动食饭吃好中的对象，会对对象池中的每个对象发送release消息，当这些对象的引用计数减至0时，会发送出dealloc消息，并且他们的内存会被释放。  
需要注意的时，自动释放池并不包含实际的对象，而是只包含对象的引用，对象将在自动释放池清理的时候被释放。  
>并不是所有新创建的对象都会被添加到自动释放池中。任何由alloc、copy、mutableCopy和new为前缀的方法创建的对象都不会被自动释放。在这种情况下，可以说你拥有这个对象。当你拥有一个对象时，需要在使用完这些对象后负责释放这些对象的内存。主动给这些对象发送release消息，或者给对象发送autorelease消息将对象加入到自动释放池中。


{% highlight objective-c linenos %}
- (Dog *)setDog
{
    Dog *dog = [[Dog alloc] init];
    [dog setId:111];
    [dog setName:@"dddd1"];
    
    return dog;
}
{% endhighlight %}
>上面这个方法如果手动管理,这个方法会出现一个问题，创建的对象会在计算执行后，被方法作为结果返回。因为方法返回这个对象，所以并不能释放它，解决这个问题的最好方法是自动释放这个对象，这样它的值就能够作为结果返回，能够延迟对象的释放直到自动释放池被清理。如下：

{% highlight objective-c linenos %}
- (Dog *)setDog
{
  //注释的代码与现在的方法二选一
  //Dog *dog = [[[Dog alloc] init] autorelease];
    Dog *dog = [[Dog alloc] init];
    [dog setId:111];
    [dog setName:@"dddd1"];
    
  //return dog;
    return [dog autorelease];
}
{% endhighlight %}

##手动内存管理规则和总结
>1、如果需要保持一个对象不被销毁，可以使用retain。在使用完对象后需要使用release进行释放。  
2、给对象发送release消息并不会必须销毁这个对象，只当这个对象的引用计数减至0时，对象才被销毁。然后系统会发送dealloc消息给对象用于释放它的内存。  
3、对使用了retain或者copy、mutableCopy、alloc或new方法的任何对象，以及具有retain和copy特性的属性进行释放，需要覆盖dealloc方法，使得在对象被释放的时候能够释放这些实例变量。   
4、在自动释放池被清空时也会发送为自动释放的对象做些事情。系统每次都会在自动释放池被释放时发送release消息给池中的每个对象。如果池中的对象引用计数为0，系统会发送dealloc消息销毁对象。  
5、如果在方法中不再需要用到这个对象，但需要将其返回，可以给这个对象发送autorelease消息用以标记这个对象延迟释放。autorelease消息并不会影响到对象的引用计数。  
6、当应用终止时，内存中的所有对象都会被释放，不论他们是否在自动释放池中。  
7、当开发Cocoa或者ios应用程序时，随着应用程序的运行，自动释放池会被创建和清空（每次的事件都会发生）。在这种情况下，如果要使用自动释放池被清空后自动释放的对象还能够存在，对象需要使用retain方法，只要这些对象的引用计数大于发送autorelease消息的数量，就能够在池清理后生存下来。

##强变量：
当给变量赋值的时候，新的retain，旧的release，然后把新的assign给旧的，如果使用ARC，需要使用__strong关键字，
或者为属性添加strong特性，默认是unsafe_unretained(assign)
{% highlight objective-c %}
__strong Person *p2 = [[Person alloc] init];
@property (strong, nonatomic) Person *person;
{% endhighlight %}
##弱变量：
当两个对象都持有彼此的强引用时，将会产生循环保持(retain cycle)，解决这个问题可以弱引用，需要使用__weak
关键字，或者为属性添加weak特性，需要注意的是，iOS4和Mac10.6中不支持弱变量
{% highlight objective-c %}
__weak Person *p2 = [[Person alloc] init];
@property (weak, nonatomic) Person *person;
{% endhighlight %}
##@autoreleasepool块
在main中，有一个大块，会在应用程序结束的时候进行清理，如果在局部创建，可以再内部进行块的嵌套
{% highlight objective-c linenos %}
@autoreleasepool {
    for (int i = 0, n = 10; i < n; i++) {
        @autoreleasepool {
            Person *p3 = [[Person alloc] init];
            [p3 setName:@"sss"];
            [p3 setAge:13];
            [p3 setSex:@"w"];
            [p3 printInfo];
        }
    }
}
{% endhighlight %}
##@autoreleasepool块

