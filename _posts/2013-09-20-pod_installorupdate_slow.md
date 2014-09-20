---
layout: post
title: "pod install/update 速度慢 解决方案"
categories:
- ios 
tags:
- ios 

--- 

最近使用CocoaPods来添加第三方类库，无论是执行pod install还是pod update都卡在了Analyzing dependencies不动了，令人甚是DT。  
  
查了好多的资料，原因在于当执行以上两个命令的时候会升级CocoaPods的spec仓库，加一个参数可以省略这一步，然后速度就会提升不少。加参数的命令如下：

{% highlight console %}
xjl:aa xjliao$ pod install --verbose --no-repo-update
xjl:aa xjliao$ pod update --verbose --no-repo-update
{% endhighlight %}




