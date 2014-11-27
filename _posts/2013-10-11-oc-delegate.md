---
layout: post
title: 'objective-c delegate'
categories:
- objective-c
tags:
- objective-c

---
###What Is Delegate?  

参考资料:  
<https://developer.apple.com/library/ios/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html>  

delegate中文叫做委托,通俗点讲就是一些事件处理"委托"给别人去完成.

实现步骤:  
通常，一个delegate的使用过程，需要经过五步：

1.    创建一个 delegate；___(下面的例子:BIDVCAViewControllerDelegate)___

2.    委托者声明一个delegate；___(BIDVCBViewController)___

3.    委托者调用delegate内的方法（method）；___(BIDVCBViewController)___

4.    被委托者设置delegate，以便被委托者调用；___(BIDVCAViewController)___

5.    被委托者实现Delegate 所定义的方法。___(BIDVCAViewController)___


下面举个例子  
从A页面PUSH进入B页面,然后在B修改A页面的值

AppDelegate程序入口 主要方法
{% highlight objective-c linenos%}
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    // Override point for customization after application launch.
    BIDVCAViewController *vca = [[BIDVCAViewController alloc] init];
    UINavigationController *nav = [[UINavigationController alloc] initWithRootViewController:vca];
    self.window.rootViewController = nav;
    self.window.backgroundColor = [UIColor whiteColor];
    [self.window makeKeyAndVisible];
    return YES;
}

{% endhighlight %}

我们自定义的delegate:  
BIDVCAViewControllerDelegate.h  

{% highlight objective-c linenos%}
//
//  BIDVCBViewControllerDelegate.h
//  DelegatePro
//
//  Created by xjliao on 14-11-25.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import <Foundation/Foundation.h>

@protocol BIDVCAViewControllerDelegate <NSObject>

@required
- (void)setNameLableText:(NSString *)text;

@end
{% endhighlight %}

A页面  
BIDVCAViewController.h

{% highlight objective-c linenos%}
//
//  BIDVCAViewController.h
//  DelegatePro
//
//  Created by xjliao on 14-11-25.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import <UIKit/UIKit.h>
#import "BIDVCAViewControllerDelegate.h"


@interface BIDVCAViewController : UIViewController <BIDVCAViewControllerDelegate>

@property (strong, nonatomic) IBOutlet UIButton *pushBBtn;
@property (retain, nonatomic) IBOutlet UILabel *nameLabel;

@end
{% endhighlight %}

A页面  
BIDVCAViewController.m

{% highlight objective-c linenos%}
//
//  BIDVCAViewController.m
//  DelegatePro
//
//  Created by xjliao on 14-11-25.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import "BIDVCAViewController.h"
#import "BIDVCBViewController.h"

@interface BIDVCAViewController ()

@end

@implementation BIDVCAViewController
{
    BIDVCBViewController *_vcb;
}

- (id)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil
{
    self = [super initWithNibName:nibNameOrNil bundle:nibBundleOrNil];
    if (self) {
        // Custom initialization
    }
    return self;
}

- (void)viewDidLoad
{
    [super viewDidLoad];
    self.title = @"VCA";
    [self.pushBBtn addTarget:self action:@selector(pushBBtnPressed:) forControlEvents:UIControlEventTouchUpInside];
    // Do any additional setup after loading the view from its nib.
}

- (void)pushBBtnPressed:(UIButton *)sender
{
    _vcb = [[BIDVCBViewController alloc] init];
    _vcb.delegate = self;
    [self.navigationController pushViewController:_vcb animated:YES];
    [_vcb release];
}

- (void)didReceiveMemoryWarning
{
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

- (void)dealloc
{
    [_nameLabel release];
    [_vcb release];
    [super dealloc];
}

- (void)setNameLableText:(NSString *)text;
{
    NSLog(@"9009090=====%@", text);
    self.nameLabel.text = text;
}

@end
{% endhighlight %}

B页面
BIDVCBViewController.h

{% highlight objective-c linenos%}
//
//  BIDVCBViewController.h
//  DelegatePro
//
//  Created by xjliao on 14-11-25.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import <UIKit/UIKit.h>
#import "BIDVCAViewControllerDelegate.h"

@class BIDVCAViewController;

@interface BIDVCBViewController : UIViewController

@property (retain, nonatomic) IBOutlet UIButton *setVCANameLabelBtn;
@property (assign, nonatomic) id<BIDVCAViewControllerDelegate> delegate;

- (IBAction)setVCANameLabelBtnPressed:(id)sender;
- (IBAction)backBtnPressed:(UIButton *)sender;

@end
{% endhighlight %}

B页面
BIDVCBViewController.m

{% highlight objective-c linenos%}
//
//  BIDVCBViewController.m
//  DelegatePro
//
//  Created by xjliao on 14-11-25.
//  Copyright (c) 2014年 xjliao. All rights reserved.
//

#import "BIDVCBViewController.h"
#import "BIDVCAViewController.h"
#import "BIDVCAViewControllerDelegate.h"

@interface BIDVCBViewController ()

@end

@implementation BIDVCBViewController

- (id)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil
{
    self = [super initWithNibName:nibNameOrNil bundle:nibBundleOrNil];
    if (self) {
        // Custom initialization
    }
    return self;
}

- (void)viewDidLoad
{
    [super viewDidLoad];
    self.title = @"VCB";
}

- (void)didReceiveMemoryWarning
{
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

- (IBAction)backBtnPressed:(UIButton *)sender
{
    [self.navigationController popViewControllerAnimated:YES];
}

- (void)dealloc
{
    [_setVCANameLabelBtn release];
    [super dealloc];
}

- (IBAction)setVCANameLabelBtnPressed:(id)sender
{
    [self.delegate setNameLableText:[NSString stringWithFormat:@"%li", random()]];
}

@end
{% endhighlight %}

xib文件就就不贴了,做完来个妹纸
![](http://xjliao-images.qiniudn.com/mm.jpeg)