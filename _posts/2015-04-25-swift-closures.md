---
layout: post
title: "Swift closures"
categories:
- Swift
tags:
- Swift


--- 
[原文地址:http://fuckingclosuresyntax.com/](http://fuckingclosuresyntax.com/)

How Do I Declare a Closure in Swift?

##As a variable:
{% highlight swift %}
var closureName: (parameterTypes) -> (returnType)
{% endhighlight %}

##As an optional variable:
{% highlight swift %}
var closureName: ((parameterTypes) -> (returnType))?
{% endhighlight %}
##As a type alias:
{% highlight swift %}
typealias closureType = (parameterTypes) -> (returnType)
{% endhighlight %}
##As a constant:
{% highlight swift %}
let closureName: closureType = { ... }
{% endhighlight %}
##As an argument to a function call:
{% highlight swift %}
func({(parameterTypes) -> (returnType) in statements})
{% endhighlight %}
##As a function parameter:
{% highlight swift %}
array.sort({ (item1: Int, item2: Int) -> Bool in return item1 < item2 })
{% endhighlight %}
##As a function parameter with implied types:
{% highlight swift %}
array.sort({ (item1, item2) -> Bool in return item1 < item2 })
{% endhighlight %}
##As a function parameter with implied return type:
{% highlight swift %}
array.sort({ (item1, item2) in return item1 < item2 })
{% endhighlight %}
##As the last function parameter:
{% highlight swift %}
array.sort { (item1, item2) in return item1 < item2 }
{% endhighlight %}
##As the last parameter, referred to by numbers:
{% highlight swift %}
array.sort { return $0 < $1 }
{% endhighlight %}
##As the last parameter, with an implied return value:
{% highlight swift %}
array.sort { $0 < $1 }
{% endhighlight %}
##As the last parameter, as a reference to an existing function:
{% highlight swift %}
array.sort(<)
{% endhighlight %}
This site is not intended to be an exhaustive list of all possible uses of closures.


Unable to access this site due to the profanity in the URL? [http://goshdarnclosuresyntax.com](http://goshdarnclosuresyntax.com) is a more work-friendly mirror.