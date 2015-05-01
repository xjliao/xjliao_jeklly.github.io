---
layout: post
title: "Swift initialization"
categories:
- Swift
tags:
- Swift


---
##官方教程
[https://developer.apple.com/library/prerelease/ios/  documentation/Swift/Conceptual/  Swift_Programming_Language/Initialization.html#//  apple_ref/doc/uid/TP40014097-CH18-ID203](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html#//apple_ref/doc/uid/TP40014097-CH18-ID203)
在此只记录比较特殊的情况
##构造过程
{% highlight swift linenos %}
init() {
    // perform some initialization here
}
{% endhighlight %}
demo
{% highlight swift linenos %}
struct Fahrenhit {
    var temperature: Double
    init() {
        self.temperature = 32.0
    }
}

var f = Fahrenheit()
println("The default temperature is \(f.temperature)° Fahrenheit")
// prints "The default temperature is 32.0° Fahrenheit"
{% endhighlight %}
##默认属性值
{% highlight swift linenos %}
struct Fahrenheit {
    var temperature = 32.0
}

{% endhighlight %}
##自定义构造过程
####1.初始化参数
{% highlight swift linenos %}

{% endhighlight %}
####3、无返回值函数
{% highlight swift linenos %}

{% endhighlight %}