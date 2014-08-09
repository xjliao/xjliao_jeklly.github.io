---
layout: post
title: "Ios UITableView demo"
categories:
- IOS
tags:
- IOS


--- 

关键点:
>需实现<UITableViewDataSource, UITableViewDelegate>这两个协议  
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
{% endhighlight %}
