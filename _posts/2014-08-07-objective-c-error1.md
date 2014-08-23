---
layout: post
title: "ERROR: after modifying system headers, please delete the module cache at"
categories:
- Objective-c
tags:
- Objective-c


--- 
{% highlight console %}
fatal error: file '/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.9.sdk/System/Library/Frameworks/Foundation.framework/Headers/NSObject.h' has been modified since the precompiled header '/Users/xjliao/Library/Developer/Xcode/DerivedData/ModuleCache/3DW0I404IQ8ZW/Foundation.pcm' was built
note: after modifying system headers, please delete the module cache at '/Users/xjliao/Library/Developer/Xcode/DerivedData/ModuleCache/3DW0I404IQ8ZW'
5 warnings and 1 error generated.
{% endhighlight %}

解决方案: 
{% highlight console %} 
1.rm -rf /Users/xjliao/Library/Developer/Xcode/DerivedData/ModuleCache/*  

2.Product -> Clean。(这时上述文件夹中将重新生成系统SDK的数据)
{% endhighlight %}

