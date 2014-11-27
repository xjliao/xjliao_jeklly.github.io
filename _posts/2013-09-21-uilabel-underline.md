---
layout: post
title: "UILabel 下划线"
categories:
- Ios 
tags:
- Ios 

--- 
  
{% highlight objective-c %}
NSMutableAttributedString *content = [[NSMutableAttributedString alloc] initWithString:[NSString stringWithFormat:@"找回密码"]];
NSRange contentRange = {0,[content length]};
[content addAttribute:NSUnderlineStyleAttributeName value:[NSNumber numberWithInteger:NSUnderlineStyleSingle] range:contentRange];
findPasswordLb.attributedText = content;
{% endhighlight %}
