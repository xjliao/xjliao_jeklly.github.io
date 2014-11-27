---
layout: post
title: "Objective-C复制对象"
categories:
- Objective-c
tags:
- Objective-c


---
##复制对象
对象的复制就是复制一个对象作为副本，他会开辟一块新的内存（堆内存）来存储副本对象，就像复制文件一样，即源对象和副本对象是两块不同的内存区域。对象要具备复制功能，必须实现<NSCopying>协议或者<NSMutableCopying>协议，常用的可复制对象有：NSNumber、NSString、NSMutableString、NSArray、NSMutableArray、NSDictionary、NSMutableDictionary

##copy和mutableCopy方法
>1.copy返回的是一个不可变的对象副本  
2.mutableCopy返回的是一个可变对象副本,无论源对象是否可变，返回的对象副本是可变的.


Foundation类实现了名为copy何mutableCopy的方法,可以使用这些方法创建对象的副本.通过实现一个符合<NSCopying>协议(用于制作副本)的方法来完成此任务.如果类必须要区分要产生对象时可变副本还是不可变副本,还需要根据<NSMutableCopying>协议实现一个方法。

{% highlight objective-c%}
dataArray2 = [dataArray mutableCopy];
[dataArray2 removeObjectAtIndex:0];
{% endhighlight %}
>上面的这段代码,在内存中创建了一个新得dataArray副本,并复制了它的所有的元素,随后执行语句
{% highlight objective-c%}
[dataArray2 removeObjectAtIndex:0];
{% endhighlight %}
删除了dataArray2中的第一个元素,但却不删除dataArray中的第一个元素,简单复制都会删除.

demo1
{% highlight objective-c linenos%}
//
//  main.m
//  co
//
//  Created by xjliao on 14-5-16.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import <Foundation/Foundation.h>

int main(int argc, const char * argv[])
{

    @autoreleasepool {
        
        // insert code here...
        NSLog(@"简单复制");
        NSMutableArray *dataArray = [NSMutableArray arrayWithObjects:
               @"one", @"tow", @"three", @"four", nil];
        NSMutableArray *dataArray2;
        
        //简单复制
        dataArray2 = dataArray;
        [dataArray2 removeObjectAtIndex:0];
        NSLog(@"dataArray:");
        
        for (NSString *elem in dataArray) {
            NSLog(@"  %@", elem);
        }
        
        NSLog(@"dataArray2:");
        
        for (NSString *elem in dataArray2) {
            NSLog(@"  %@", elem);
        }
        
        //复制一份,然后删除副本的第一个元素
        dataArray2 = [dataArray mutableCopy];
        [dataArray2 removeObjectAtIndex:0];
        
        NSLog(@"dataArray:");
        
        for (NSString *elem in dataArray) {
            NSLog(@"  %@", elem);
        }
        
        NSLog(@"dataArray2:");
        
        for (NSString *elem in dataArray2) {
            NSLog(@"  %@", elem);
        }
        
    }
    return 0;
}
{% endhighlight %}
结果
{% highlight console %}
2014-05-16 23:21:33.405 co[714:303] 简单复制
2014-05-16 23:21:33.406 co[714:303] dataArray:
2014-05-16 23:21:33.407 co[714:303]   tow
2014-05-16 23:21:33.407 co[714:303]   three
2014-05-16 23:21:33.407 co[714:303]   four
2014-05-16 23:21:33.408 co[714:303] dataArray2:
2014-05-16 23:21:33.408 co[714:303]   tow
2014-05-16 23:21:33.408 co[714:303]   three
2014-05-16 23:21:33.409 co[714:303]   four
2014-05-16 23:21:33.409 co[714:303] dataArray:
2014-05-16 23:21:33.410 co[714:303]   tow
2014-05-16 23:21:33.410 co[714:303]   three
2014-05-16 23:21:33.410 co[714:303]   four
2014-05-16 23:21:33.411 co[714:303] dataArray2:
2014-05-16 23:21:33.411 co[714:303]   three
2014-05-16 23:21:33.411 co[714:303]   four
{% endhighlight %}
>demo1这个程序定义了可变数组dataArray,并分别将其元素设置为字符串对象@"one",@"two", @"three"和@"four".复制语句dataArray2 = dataArray;仅仅创建了对内存中同一书中对象的一个引用.这两个引用中的第一个元素都会被删除.  
然后创建一个dataArray的可变副本并赋值给dataArray2的最终副本.这就在内存中创建了两个截然相同的可变数组.所以任意操作它们其中的一个都不会相互影响.

##浅复制与深复制
Objective-C对象的深，浅拷贝的区别就在于对copyWithZone或者mutableCopyWithZone的不同实现  

浅复制：对象的属性直接复制  
深复制：使用属性的copy


>1.浅复制只复制对象的本身,对象里的属性,包含的对象不做复制.对象里的属性和对象指向的还是用同一块内存地址.  
2.深复制则即复制对象本身,对象的属性也会复制一份(即和源对象的属性地址不是同一块)
3.Foundation框架支持复制的类,默认都是浅复制.如果需要为深复制,则需要使用分类(category)添加

{% highlight objective-c linenos%}
NSMutableArray *dataArray3 = [NSMutableArray arrayWithObjects:
    [NSMutableString stringWithString:@"one"],
    [NSMutableString stringWithString:@"two"],
    [NSMutableString stringWithString:@"three"], nil];
NSMutableArray *dataArray4;
NSMutableString *mStr;

NSLog(@"浅拷贝与深拷贝");
NSLog(@"dataArray3:");

for (NSString *elem in dataArray3) {
    NSLog(@"  %@", elem);
}

//复制一份,然后删除副本的第一个元素
dataArray4 = [dataArray3 mutableCopy];
mStr = [dataArray3 objectAtIndex:0];
[mStr appendString:@"ONE"];

NSLog(@"dataArray3:");

for (NSString *elem in dataArray3) {
    NSLog(@"  %@", elem);
}

NSLog(@"dataArray4:");

for (NSString *elem in dataArray4) {
    NSLog(@" %@", elem);
}
{% endhighlight %}

结果

```console
2014-05-16 23:21:33.412 co[714:303] 浅拷贝与深拷贝
2014-05-16 23:21:33.412 co[714:303] dataArray3:
2014-05-16 23:21:33.412 co[714:303]   one
2014-05-16 23:21:33.413 co[714:303]   two
2014-05-16 23:21:33.413 co[714:303]   three
2014-05-16 23:21:33.414 co[714:303] dataArray3:
2014-05-16 23:21:33.414 co[714:303]   oneONE
2014-05-16 23:21:33.414 co[714:303]   two
2014-05-16 23:21:33.415 co[714:303]   three
2014-05-16 23:21:33.415 co[714:303] dataArray4:
2014-05-16 23:21:33.415 co[714:303]  oneONE
2014-05-16 23:21:33.416 co[714:303]  two
2014-05-16 23:21:33.416 co[714:303]  three
Program ended with exit code: 0

```
##深浅复制和retain之间的总结  
copy、mutableCopy和retain之间的关系
1.Foundation可复制的对象，当我们copy的时一个不可变的对象时，它的作用相当于retain(cocoa做得内存优化)  
2.当我们使用mutableCopy时，无论源对象是否可变，副本是可变的，并且实现了真正意义上的拷贝  
3.当我们copy的是一个可变对象时，副本对象时不可变得，同样实现了真正意义上的拷贝
##对象自定义复制
>对象的拥有复制特性,须实现NSCopying、NSMutableCopying协议，实现该协议的copyWithZone:方法（NSCopying协议的）和mutableCopyWithZone:方法（NSMutableCoping协议的)  

demo 
不可变  

Person.h
{% highlight objective-c linenos%}
#import <Foundation/Foundation.h>

@interface Person : NSObject <NSCopying>

@property(nonatomic, copy) NSString *name;
@property(nonatomic, retain) NSNumber *age;

@end
{% endhighlight %}

Person.m

{% highlight objective-c linenos%}
#import "Person.h"

@implementation Person

//@synthesize name = _name, age = _age; //新版本xcode 无需再写了

- (id)copyWithZone:(NSZone *)zone
{
//    //浅复制
//    Person *person = [[Person allocWithZone:zone] init];
//    person.name = _name;
//    person.age = _age;
    
    
    //深复制
    Person *person = [[Person allocWithZone:zone] init];
    [person setName:[_name mutableCopy]];
    
    //NSNumber是不可变的，所以只能用copy或者=
    [person setAge:[_age copy]];
    
    return person;
}
@end
{% endhighlight %}

可变对象

Person1.h
{% highlight objective-c linenos%}

#import <Foundation/Foundation.h>

@interface Person1 : NSObject <NSMutableCopying>

@property (nonatomic, copy) NSString *name;

@end

{% endhighlight %}
{% highlight objective-c linenos%}
#import "Person1.h"

@implementation Person1

- (id)mutableCopyWithZone:(NSZone *)zone
{
    Person1 *p = [[Person1 allocWithZone:zone] init];
    //浅拷贝
    //p.name = _name;
    
    //深拷贝
    p.name = [_name mutableCopy];
    
    return p;
}

@end
{% endhighlight %}

测试

{% highlight objective-c linenos%}
//
//  main.m
//  cucp
//
//  Created by xjliao on 14-5-18.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Person.h"
#import "Person1.h"

int main(int argc, const char * argv[])
{

    @autoreleasepool {
        
        // insert code here...
        //不可变复制
        Person *p1 = [[Person alloc] init];
        Person *p2;
        
        [p1 setName:@"廖新建"];
        [p1 setAge:[NSNumber numberWithInt:25]];
        
        p2 = [p1 copy];
        
        NSLog(@"p1.p %p", p1);
        NSLog(@"p2.p %p", p2);
        
        NSLog(@"p1.name %p", [p1 name]);
        NSLog(@"p2.name %p", [p2 name]);
        
        NSLog(@"p1.age %p", [p1 age]);
        NSLog(@"p2.age %p", [p2 age]);
        
        //可变复制
        Person1 *p3 = [[Person1 alloc] init];
        p3.name = @"廖家儒";
        
        Person1 *p4 = [p3 mutableCopy];
        
        NSLog(@"p3.p %p  p4.p %p", p3, p4);
        NSLog(@"p3.name %p  p4.name %p", p3.name, p4.name);
        
    }
    return 0;
}

{% endhighlight %}

结果

{% highlight objective-c linenos%}
2014-05-18 11:53:04.145 cucp[1333:303] p1.p 0x100202830
2014-05-18 11:53:04.147 cucp[1333:303] p2.p 0x1002028d0
2014-05-18 11:53:04.147 cucp[1333:303] p1.name 0x100002598
2014-05-18 11:53:04.148 cucp[1333:303] p2.name 0x100202a10
2014-05-18 11:53:04.148 cucp[1333:303] p1.age 0x1927
2014-05-18 11:53:04.149 cucp[1333:303] p2.age 0x1927
2014-05-18 11:53:04.149 cucp[1333:303] p3.p 0x1002046f0  p4.p 0x100204700
2014-05-18 11:53:04.149 cucp[1333:303] p3.name 0x100002678  p4.name 0x100203580
Program ended with exit code: 0
{% endhighlight %}