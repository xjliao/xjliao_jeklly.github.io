---
layout: post
title: "Objective-C归档"
categories:
- Objective-C
tags:
- Objective-C


---

##归档概念
对象归档是指将对象写入文件保存在硬盘上，当再次重新打开程序时间，可以还原这些对象。也可以称它为对象序列化，对象持久化
>数据持久性的方式:  
1.NSKeyedArchiver--对象归档  
2.NSUserDefaults  
3.XML属性列表归档(NSArray,NSDictionary保存文件)  
4.SQlite数据库，Core Data数据库  
归档的形式  
1.对Foundation库中对象进行归档  
2.自定义对象进行归档（需要实现归档协议，NSCodeing）  
归档后的文件是加密的，属性列表是明文的

##XML属性列表归档
只支持Foundation里的NSString,NSNumber,NSArray,NSDictionary,NSData进行归档


{% highlight objective-c linenos%}
        //使用属性列表归档
        NSDictionary *glossary = [NSDictionary dictionaryWithObjectsAndKeys:
                                      @"A class defined so other classes can inherit form it",
                                      @"abstract class",
                                      @"To implement all methods defined in a protocol",
                                      @"adopt",
                                      @"Storing an object for later user.",
                                      @"archiving", nil];
        [glossary writeToFile:@"/Users/lxj/glossary" atomically:YES];
        
        if ([glossary writeToFile:@"glossary" atomically:YES] == NO) {
            NSLog(@"Save to file failed!");
        }
{% endhighlight %}

##NSKeyedArchive归档
{% highlight objective-c linenos%}
        NSDictionary *glossary = [NSDictionary dictionaryWithObjectsAndKeys:
                                      @"A class defined so other classes can inherit form it",
                                      @"abstract class",
                                      @"To implement all methods defined in a protocol",
                                      @"adopt",
                                      @"Storing an object for later user.",
                                      @"archiving", nil];
        //归档                             
        [NSKeyedArchiver archiveRootObject:glossary toFile:@"/Users/lxj/glossary.archive"];
        
        //解归档
        getGlossary = [NSKeyedUnarchiver unarchiveObjectWithFile:@"/Users/lxj/glossary.archive"];
        
        for (NSString *key in getGlossary) {
            NSLog(@"%@=%@", key, [getGlossary objectForKey:key]);
        }
{% endhighlight %}
NSData自定义内容归档
{% highlight objective-c linenos%}
        //自定义内容归档（序列化）
        NSMutableData *data = [NSMutableData data];
        NSKeyedArchiver *ka = [[NSKeyedArchiver alloc] initForWritingWithMutableData:data];
        NSArray *a = @[@"xjliao", @"jrliao", @"lhdeng", @"xyliao", @"lihualou"];
        [ka encodeObject:a forKey:@"name"];
        [ka encodeInt:25 forKey:@"age"];
        [ka finishEncoding];
        [ka release];
        [data writeToFile:@"/Users/lxj/data.archive" atomically:YES];
        
        //解归档(反序列化）
        NSData *d = [NSData  dataWithContentsOfFile:@"/Users/lxj/data.archive"];
        NSKeyedUnarchiver *kud = [[NSKeyedUnarchiver alloc] initForReadingWithData:d];
        NSArray *ua = [kud decodeObjectForKey:@"name"];
        int age = [kud decodeIntForKey:@"age"];
        [kud release];
        
        NSLog(@"%@", ua);
        NSLog(@"%i", age);
{% endhighlight %}
##自定义对象归档
需要实现<NSCoding>协议的下面两个方法  

{% highlight objective-c linenos%}
//对属性编码，归档时调用
- (void)encodeWithCoder:(NSCoder *)aCoder;
//对属性解码，解归档调用
- (id)initWithCoder:(NSCoder *)aDecoder;
{% endhighlight %}

Person.h
{% highlight objective-c linenos%}
//
//  Person.h
//  gd
//
//  Created by xjliao on 14-5-18.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface Person : NSObject <NSMutableCopying, NSCoding>

@property (nonatomic, copy) NSString *name, *email;
@property int age;


@end
{% endhighlight %}

Person.m
{% highlight objective-c linenos%}
//
//  Person.m
//  gd
//
//  Created by xjliao on 14-5-18.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import "Person.h"

#define NAME @"name"
#define AGE @"age"
#define EMAIL @"email"

@implementation Person

- (id)mutableCopyWithZone:(NSZone *)zone
{
    Person *person = [[[self class] allocWithZone:zone] init];
    [person setName:[_name mutableCopy]];
    [person setAge:_age];
    [person setEmail:[_email mutableCopy]];
    
    return person;
}


//对属性编码，归档时调用
- (void)encodeWithCoder:(NSCoder *)aCoder
{
    [aCoder encodeObject:[self name] forKey:NAME];
    [aCoder encodeInt:[self age] forKey:AGE];
    [aCoder encodeObject:[self email] forKey:EMAIL];
}

//对属性解码，解归档调用
- (id)initWithCoder:(NSCoder *)aDecoder
{
    self = [super init];
    
    if (self != nil) {
        self.name = [aDecoder decodeObjectForKey:NAME];
        _age = [aDecoder decodeIntForKey:AGE];
        [self setEmail:[aDecoder decodeObjectForKey:EMAIL]];
    }
    
    return self;
}

- (NSString *)description
{
    NSString *nstr = [NSString stringWithFormat:@"名字：%@ 年龄:%i 邮箱：%@",
                      [self name], [self age], [self email]];
    return nstr;
}
@end
{% endhighlight %}

测试main.m

{% highlight objective-c linenos%}
//
//  main.m
//  gd
//
//  Created by xjliao on 14-5-18.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Person.h"

int main(int argc, const char * argv[])
{

    @autoreleasepool {        
        //自定义对象归档Person实现NSCoding
        //归档
        Person *p = [[Person alloc] init];
        [p setName:@"liaoxinjian"];
        [p setAge:25];
        [p setEmail:@"lxj990@gmail.com"];
        
        [NSKeyedArchiver archiveRootObject:p toFile:@"/Users/lxj/person.arc"];
        
        [p release];
        
        //解归档
        Person *p1;
        p1 = [NSKeyedUnarchiver unarchiveObjectWithFile:@"/Users/lxj/person.arc"];
        
        NSLog(@"p1 = %@", p1);
        NSLog(@"p1.name=%@, age=%i, email=%@", [p1 name], [p1 age], [p1 email]);
        
    }
    return 0;
}
{% endhighlight %}

//NSData自定义内容归档
{% highlight objective-c linenos%}
        //自定义内容归档
        Person *p3 = [[Person alloc] init];
        [p3 setName:@"liaoxinjian"];
        [p3 setAge:25];
        [p3 setEmail:@"lxj990@gmail.com"];

        Person *p4 = [[Person alloc] init];
        [p4 setName:@"loulihua"];
        [p4 setAge:23];
        [p4 setEmail:@"suplss@163.com"];

        Person *p5 = [[Person alloc] init];
        [p5 setName:@"denglianhong"];
        [p5 setAge:25];
        [p5 setEmail:@"denglianhong@163.com"];
        
        NSMutableData *pd = [NSMutableData data];
        NSKeyedArchiver *pka = [[NSKeyedArchiver alloc] initForWritingWithMutableData:pd];
        [pka encodeObject:p3 forKey:@"p3"];
        [pka encodeObject:p4 forKey:@"p4"];
        [pka encodeObject:p5 forKey:@"p5"];
        [pka finishEncoding];
        [pka release];
        
        [pd writeToFile:@"/Users/lxj/personData.arc" atomically:YES];
        
        //自定义内容解档
        NSData *dp = [NSData dataWithContentsOfFile:@"/Users/lxj/personData.arc"];
        NSKeyedUnarchiver *pku = [[NSKeyedUnarchiver alloc] initForReadingWithData:dp];
        Person *p33 = [pku decodeObjectForKey:@"p3"];
        Person *p44 = [pku decodeObjectForKey:@"p4"];
        Person *p55 = [pku decodeObjectForKey:@"p5"];
        
        NSLog(@"p33.name=%@ age=%i email=%@", [p33 name],[p33 age], [p33 email]);
        NSLog(@"p44.name=%@ age=%i email=%@", [p44 name], [p44 age], [p44 email]);
        NSLog(@"p55.name=%@ age=%i email=%@", [p55 name], [p55 age], [p55 email]);
{% endhighlight %}

##归档总结
- 归档和解归档的两种方式  
1.简单的把一个对象归档为一个文件
2.把数据编码到NSData文件中，然后归档到文件中
- 自定义对象如何归档，实现NSCoding协议
- 归档后的文件是加密的