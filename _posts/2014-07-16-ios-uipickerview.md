---
layout: post
title: "Ios UIPickerView demo"
categories:
- IOS
tags:
- IOS


--- 

关键点:
>需实现<UIPickerViewDataSource, UIPickerViewDelegate>这两个协议  
1.UIPickerViewDataSource需要实现的方法:
{% highlight objective-c linenos%}
//设置选择框的个数  
- (NSInteger)numberOfComponentsInPickerView:(UIPickerView *)pickerView  
{  
      return 2;  
}  
//设置每个选择框的数量  
- (NSInteger)pickerView:(UIPickerView *)pickerView numberOfRowsInComponent:(NSInteger)component {  
    if (component == kStateComponent) {  
            return [[self provinces] count];  
    } else {  
        return [[self countrys] count];  
    }  
    
}
{% endhighlight %} 

>2.UIPickerViewDelegate需要实现的方法:   
 
{% highlight objective-c linenos%}
//设置每个选择框的内容  
- (NSString *)pickerView:(UIPickerView *)pickerView titleForRow:(NSInteger)row forComponent:(NSInteger)component {
    if (component == kStateComponent) {
        return self.provinces[row];
    } else {
        return self.countrys[row];
    }
}  
//可选实现方法，自定义picker 显示图片
- (UIView *)pickerView:(UIPickerView *)pickerView viewForRow:(NSInteger)row forComponent:(NSInteger)component reusingView:(UIView *)view
{
    UIImage *image = self.images[row];
    UIImageView *imageView = [[UIImageView alloc] initWithImage:image];
    return imageView;
}

{% endhighlight %}

以下是demo
依赖选择，联动

BIDDependentPickerViewController.h  
  
{% highlight objective-c linenos%}
//
//  BIDDependentPickerViewController.h
//  Pickers
//
//  Created by xjliao on 14-7-1.
//  Copyright (c) 2014年 xjl. All rights reserved.
//

#import <UIKit/UIKit.h>
#define kStateComponent 0
#define kZipComponent 1

@interface BIDDependentPickerViewController : UIViewController <UIPickerViewDataSource, UIPickerViewDelegate>
@property (strong, nonatomic) IBOutlet UIPickerView *dependentPicker;
@property (strong, nonatomic) UIButton *btn;
@property (strong, nonatomic) NSDictionary *provinceDics;
@property (strong, nonatomic) NSArray *provinces;
@property (strong, nonatomic) NSArray *countrys;

//@property (strong, nonatomic) NSDictionary *stateZips;
//@property (strong, nonatomic) NSArray *states;
//@property (strong, nonatomic) NSArray *zips;


- (void)btnPressed;
@end
{% endhighlight %}
 
BIDDependentPickerViewController.m  

{% highlight objective-c linenos%}
//
//  BIDDependentPickerViewController.m
//  Pickers
//
//  Created by xjliao on 14-7-1.
//  Copyright (c) 2014年 xjl. All rights reserved.
//

#import "BIDDependentPickerViewController.h"

@interface BIDDependentPickerViewController ()

@end

@implementation BIDDependentPickerViewController

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
    UIButton *btn = [[UIButton alloc] initWithFrame:CGRectMake(150, 300, 100, 100)];
    [btn setTitle:@"单击" forState:UIControlStateNormal];
    [btn setTitleColor:[UIColor redColor] forState:UIControlStateNormal];
    [self.view addSubview:btn];
    [btn addTarget:self action:@selector(btnPressed) forControlEvents:UIControlEventTouchUpInside];
    
    NSBundle *boundle = [NSBundle mainBundle];
    NSURL *plistURL = [boundle URLForResource:@"statedictionary" withExtension:@"plist"];
    
    self.provinceDics = [NSDictionary dictionaryWithContentsOfURL:plistURL];
    
    NSArray *allprovinces = [self.provinceDics allKeys];
    NSArray *sortedprovinces = [allprovinces sortedArrayUsingSelector:@selector(compare:)];
    
    self.provinces = sortedprovinces;
    NSString *selectedState = self.provinces[0];
    self.countrys = self.provinceDics[selectedState];
    
    // Do any additional setup after loading the view.
}

- (void)didReceiveMemoryWarning
{
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

//每个选择组件里的内容的数量
- (NSInteger)pickerView:(UIPickerView *)pickerView numberOfRowsInComponent:(NSInteger)component {
    if (component == kStateComponent) {
        return [[self provinces] count];
    } else {
        return [[self countrys] count];
    }
    
}

//选择组件的数量，多少个选择项
- (NSInteger)numberOfComponentsInPickerView:(UIPickerView *)pickerView
{
    return 2;
}

//选择组件的内容
- (NSString *)pickerView:(UIPickerView *)pickerView titleForRow:(NSInteger)row forComponent:(NSInteger)component {
    if (component == kStateComponent) {
        return self.provinces[row];
    } else {
        return self.countrys[row];
    }
}

- (void)btnPressed {
    NSInteger stateRow = [self.dependentPicker selectedRowInComponent:kStateComponent];
    NSInteger zipRow = [self.dependentPicker selectedRowInComponent:kZipComponent];
    
    NSString *state = self.provinces[stateRow];
    NSString *zip = self.countrys[zipRow];
    
    NSString *title = [[NSString alloc] initWithFormat:@"你选择的省分为:%@", state];
    NSString *msg = [[NSString alloc] initWithFormat:@"你选择的省份:%@, 城市:%@", state, zip];
    
    UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:title message:msg delegate:nil cancelButtonTitle:@"取消" otherButtonTitles:nil, nil];
    
    [alertView show];
//    
//    UIAlertView *uav = [[UIAlertView alloc] initWithTitle:@"fdojf" message:@"Nothing" delegate:nil cancelButtonTitle:@"cancle" otherButtonTitles:nil, nil];
//    [uav show];
}

//拖动左边的齿轮时，右边的数据相应的Reload更新
- (void)pickerView:(UIPickerView *)pickerView didSelectRow:(NSInteger)row inComponent:(NSInteger)component
{
    if (component == kStateComponent) {
        NSString *selectedState = self.provinces[row];
        self.countrys = self.provinceDics[selectedState];
        [self.dependentPicker reloadComponent:kZipComponent];
        [self.dependentPicker selectRow:0 inComponent:kZipComponent animated:YES];
    }
}
/*
#pragma mark - Navigation

// In a storyboard-based application, you will often want to do a little preparation before navigation
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
{
    // Get the new view controller using [segue destinationViewController].
    // Pass the selected object to the new view controller.
}
*/

@end
{% endhighlight %}


自定义UIPickerView  

BIDCustomPickerViewController.h  

{% highlight objective-c linenos%}
//
//  BIDCustomPickerViewController.h
//  Pickers
//
//  Created by xjliao on 14-7-1.
//  Copyright (c) 2014年 xjl. All rights reserved.
//

#import <UIKit/UIKit.h>

@interface BIDCustomPickerViewController : UIViewController<UIPickerViewDataSource, UIPickerViewDelegate, UIAlertViewDelegate>

@property (strong, nonatomic) UIPickerView *picker;
@property (strong, nonatomic) UIButton *btn;
@property (strong, nonatomic) UILabel *winLabel;
@property (strong, nonatomic) NSArray *images;

- (void)btnPressed;
- (void)spin;

@end
{% endhighlight %}

BIDCustomPickerViewController.m

{% highlight objective-c linenos%}
//
//  BIDCustomPickerViewController.m
//  Pickers
//
//  Created by xjliao on 14-7-1.
//  Copyright (c) 2014年 xjl. All rights reserved.
//

#import "BIDCustomPickerViewController.h"

@interface BIDCustomPickerViewController ()

@end

@implementation BIDCustomPickerViewController
@synthesize picker, btn, images, winLabel;

- (void)loadView
{
    UIView *view = [[UIView alloc] initWithFrame:[[UIScreen mainScreen] applicationFrame]];
    view.backgroundColor = [UIColor blueColor];
    self.view = view;
    [view release];
    
    [super loadView];
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
    // Do any additional setup after loading the view.
    btn = [[UIButton alloc] initWithFrame:CGRectMake(150, 150, 100, 100)];
    [btn setTitle:@"按我啊" forState:UIControlStateNormal];
    [btn setTitleColor:[UIColor greenColor] forState:UIControlStateNormal];
    [btn addTarget:self action:@selector(btnPressed) forControlEvents:UIControlEventTouchUpInside];
    [self.view addSubview:btn];
    
    winLabel = [[UILabel alloc] initWithFrame:CGRectMake(200, 200, 100, 100)];
    [winLabel setTextColor:[UIColor redColor]];
    [self.view addSubview:winLabel];
    
    picker = [[UIPickerView alloc] initWithFrame:CGRectMake(0, 0, 200, 200)];
    picker.dataSource = self;
    picker.delegate = self;
    [self.view addSubview:picker];
    
    self.images = @[[UIImage imageNamed:@"Custom"], [UIImage imageNamed:@"Date"],
                    [UIImage imageNamed:@"Dependent"], [UIImage imageNamed:@"Double"],
                    [UIImage imageNamed:@"Single"]];
    
    srandom(time(NULL));
//    picker
//    []
}

- (NSInteger)numberOfComponentsInPickerView:(UIPickerView *)pickerView
{
    return [self.images count];
}

- (NSInteger)pickerView:(UIPickerView *)pickerView numberOfRowsInComponent:(NSInteger)component
{
    return [self.images count];
}

//picker 显示图片
- (UIView *)pickerView:(UIPickerView *)pickerView viewForRow:(NSInteger)row forComponent:(NSInteger)component reusingView:(UIView *)view
{
    UIImage *image = self.images[row];
    UIImageView *imageView = [[UIImageView alloc] initWithImage:image];
    return imageView;
}

- (void)didReceiveMemoryWarning
{
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}


- (void)btnPressed
{
    [self spin];
}

- (void)spin
{
    BOOL win = NO;
    int numInRow = 1;
    int lastVal = -1;
    
    for (int i = 0; i < 5; i++) {
        int newValue = random() % [self.images count];
        
        if (newValue == lastVal) {
            numInRow++;
        } else {
            numInRow = 1;
        }
        
        lastVal = newValue;
        
        [self.picker selectRow:newValue inComponent:i animated:YES];
        [self.picker reloadComponent:i];
        
        if (numInRow >= 3) {
            win = YES;
        }
        
        if (win) {
            self.winLabel.text = @"恭喜您，赢了！";
        } else {
            self.winLabel.text = @"很遗憾，就差一点了，不要灰心，继续努力";
        }
    }
}

/*
#pragma mark - Navigation

// In a storyboard-based application, you will often want to do a little preparation before navigation
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
{
    // Get the new view controller using [segue destinationViewController].
    // Pass the selected object to the new view controller.
}
*/

@end
{% endhighlight %}
