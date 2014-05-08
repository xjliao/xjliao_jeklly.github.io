---
layout: post
title: "Objective-c预处理"
categories:
- Objective-C
tags:
- Objective-C


---
##预处理
>预处理程序是编译过程的一部分，是可以识别散布在程序中的特定语句。预处理语句使用井号（＃）号标记在一行中第一个非空格字符

预处理语句有以下几个语句
>\#define 给符号名称制定程序常量  
>\#import 导入  
>\#ifdef、#endif、#else、ifndef 条件编译  
>\#undef 消除特定名称的定义

语句使用demo

macro.h：预处理，一些宏的定义

{% highlight objective-c linenos %}
//
//  macro.h
//  Macro
//
//  Created by xjliao on 14-5-7.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

//宏定义
#define OK 5
#define NOOK 6
#define PI 3.141592654
#define TWO_PI 2.0 * PI

#define AND &&
#define OR ||
#define EQUALS ==
#define NOTEQUALS !=
#define UP >
#define LOW <
#define mod %
#define div /

//判断闰年
#define IS_LEAP_YEAR(year) year % 4 == 0 && year % 100 != 0 || year % 400 == 0

//平方
#define SQUARE(x) (x) * (x)

#define CIRCLE_AREA(raduis) [[[Circle alloc] init] area: raduis]

#define MAX(a, b) ((a) > (b) ? (a) : (b))

#define IPAD
//条件编译
#ifdef IPAD
#  define DEVICE "IPAD"
#else
#  define DEVICE "IPHONE"
#endif

//没啥逻辑，只是用下语法
#if defined (IPAD)
#  undef IPAD
#elif !defined (IPAD)
#  define IPAD
#ele
#  define IPHONE
#endif

{% endhighlight %}

圆类.h
Circle.h
{% highlight objective-c linenos %}
//
//  Circle.h
//  Macro
//
//  Created by xjliao on 14-5-7.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface Circle : NSObject
{
 @private
    int _raduis;
}

@property int raduis;

- (double)area;
- (double)circumference;
- (double)area:(int)raduis;

@end
{% endhighlight %}

Circle.m
{% highlight objective-c linenos %}
//
//  Circle.m
//  Macro
//
//  Created by xjliao on 14-5-7.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import "Circle.h"
#include "macro.h"

@implementation Circle

@synthesize raduis = _raduis;

//面积
- (double)area
{
    return PI * _raduis * _raduis;
}

//周长
- (double)circumference
{
    //return 2.0 * PI * _raduis;
    return TWO_PI * _raduis;
}

- (double)area:(int)raduis
{
    return PI * raduis * raduis;
}

@end
{% endhighlight %}

test main.m
{% highlight objective-c linenos %}
//
//  main.m
//  Macro
//
//  Created by xjliao on 14-5-7.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import <Foundation/Foundation.h>
#include "macro.h"
#include "Circle.h"

int main(int argc, const char * argv[])
{

    @autoreleasepool {
        
        // insert code here...
        NSLog(@"OK = %i", OK);
        NSLog(@"NOOK = %i", NOOK);
        
        Circle *cir = [[Circle alloc] init];
        
        for (int i = 1; i < 4; i++) {
            int raduis = i;
            [cir setRaduis: raduis];
            
            double area = [cir area];
            double circumference = cir.circumference;
            
            NSLog(@"area = %0.2f", area);
            NSLog(@"circumference = %0.2f", circumference);
            
            if (area EQUALS circumference AND raduis EQUALS 2) {
                NSLog(@"area == circumference");
            } else if (area UP circumference) {
                NSLog(@"area > circumference");
            } else if (area LOW circumference){
                NSLog(@"area < circumference");
            } else {
                NSLog(@"unknown");
            }
        }
        
        int year = 2003;
        
        if (IS_LEAP_YEAR(year)) {
            NSLog(@"是闰年");
        } else {
            NSLog(@"不是闰年");
        }
        
        double x = 5.25;
        double xx = SQUARE(x);
        NSLog(@"XX = %0.4f", xx);
        //使用宏定义求面积
        double areade = CIRCLE_AREA(3);
        NSLog(@"areade = %0.2f", areade);
        
        int max = MAX(100, 190);        
        NSLog(@"max = %i", max);
        
        NSString *device = @DEVICE;
        NSLog(@"device = %@", device);
        
        
    }
    return 0;
}
{% endhighlight %}

result:

```console
2014-05-08 13:08:05.488 Macro[3562:303] OK = 5
2014-05-08 13:08:05.490 Macro[3562:303] NOOK = 6
2014-05-08 13:08:05.491 Macro[3562:303] area = 3.14
2014-05-08 13:08:05.491 Macro[3562:303] circumference = 6.28
2014-05-08 13:08:05.491 Macro[3562:303] area < circumference
2014-05-08 13:08:05.492 Macro[3562:303] area = 12.57
2014-05-08 13:08:05.492 Macro[3562:303] circumference = 12.57
2014-05-08 13:08:05.492 Macro[3562:303] area == circumference
2014-05-08 13:08:05.493 Macro[3562:303] area = 28.27
2014-05-08 13:08:05.493 Macro[3562:303] circumference = 18.85
2014-05-08 13:08:05.493 Macro[3562:303] area > circumference
2014-05-08 13:08:05.494 Macro[3562:303] 不是闰年
2014-05-08 13:08:05.494 Macro[3562:303] XX = 27.5625
2014-05-08 13:08:05.494 Macro[3562:303] areade = 28.27
2014-05-08 13:08:05.495 Macro[3562:303] max = 190
2014-05-08 13:08:05.495 Macro[3562:303] device = IPAD
Program ended with exit code: 0
```