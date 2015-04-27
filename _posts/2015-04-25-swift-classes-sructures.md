---
layout: post
title: "Swift Classes and Structures"
categories:
- Swift
tags:
- Swift


---
##官方教程
[https://developer.apple.com/library/prerelease/ios/  documentation/Swift/Conceptual/  Swift_Programming_Language/ClassesAndStructures.html#//  apple_ref/doc/uid/TP40014097-CH13-ID82](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-ID82)

##类和结构体对比
Swift 中类和结构体有很多共同点。共同处在于：  
>定义属性用于存储值  
定义方法用于提供功能  
定义附属脚本用于访问值  
定义构造器用于生成初始化值  
通过扩展以增加默认实现的功能  
符合协议以对某类提供标准功能
 
更多信息请参见 属性，方法，下标脚本，初始过程，扩展，和协议。

与结构体相比，类还有如下的附加功能：
>继承允许一个类继承另一个类的特征  
类型转换允许在运行时检查和解释一个类实例的类型  
解构器允许一个类实例释放任何其所被分配的资源  
引用计数允许对一个类的多次引用

更多信息请参见继承，类型转换，初始化，和自动引用计数。

##定义语法
{% highlight swift%}
class SomeClass {
    // class definition goes here
}
struct SomeStructure {
    // structure definition goes here
}
{% endhighlight %}
example
{% highlight swift linenos %}
struct Resolution {
    var width = 0
    var height = 0
}
class VideoMode {
    var resolution = Resolution()
    var interlaced = false
    var frameRate = 0.0
    var name: String?
}
{% endhighlight %}
##类和结构体实例
{% highlight swift linenos %}
let someResolution = Resolution()
let someVideoMode = VideoMode()
{% endhighlight %}
##访问属性
{% highlight swift linenos %}
println("The width of someResolution is \(someResolution.width)")
// prints "The width of someResolution is 0"

println("The width of someVideoMode is \(someVideoMode.resolution.width)")
// prints "The width of someVideoMode is 0"

someVideoMode.resolution.width = 1280
println("The width of someVideoMode is now \(someVideoMode.resolution.width)")
// prints "The width of someVideoMode is now 1280"
{% endhighlight %}
##结构类型的成员初始化
{% highlight swift linenos %}
let vga = Resolution(width: 640, height: 480)
{% endhighlight %}
##结构体和枚举是值类型
{% highlight swift linenos %}
let hd = Resolution(width: 1920, height: 1080)
var cinema = hd
cinema.width = 2048
println("cinema is now  \(cinema.width) pixels wide")
// 输出 "cinema is now 2048 pixels wide"
println("hd is still \(hd.width    ) pixels wide")
// 输出 "hd is still 1920 pixels wide"
enum CompassPoint {
    case North, South, East, West
}
var currentDirection = CompassPoint.West
let rememberedDirection = currentDirection
currentDirection = .East
if rememberDirection == .West {
    println("The remembered direction is still .West")
}
// 输出 "The remembered direction is still .West"
{% endhighlight %}
##类是引用类型
{% highlight swift linenos %}
let tenEighty = VideoMode()
tenEighty.resolution = hd
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0

let alsoTenEighty = tenEighty
alsoTenEighty.frameRate = 30.0

println("The frameRate property of tenEighty is now \(tenEighty.frameRate)")
// 输出 "The frameRate property of theEighty is now 30.0"
{% endhighlight %}
##恒等运算符
Swift 内建了两个恒等运算符:
>等价于 （ === ）  
不等价于 （ !== ）

以下是运用这两个运算符检测两个常量或者变量是否引用同一个实例：
{% highlight swift linenos %}
if tenEighty === alsoTenTighty {
    println("tenTighty and alsoTenEighty refer to the same Resolution instance.")
}
{% endhighlight %}
请注意“等价于"（用三个等号表示，===） 与“等于"（用两个等号表示，==）的不同：  
>“等价于”表示两个类类型（class type）的常量或者变量引用同一个类实例。  
“等于”表示两个实例的值“相等”或“相同”，判定时要遵照类设计者定义定义的评判标准，因此相比于“相等”，这是一种更加合适的叫法。
##指针
如果你有 C，C++ 或者 Objective-C 语言的经验，那么你也许会知道这些语言使用指针来引用内存中的地址。一个 Swift 常量或者变量引用一个引用类型的实例与 C 语言中的指针类似，不同的是并不直接指向内存中的某个地址，而且也不要求你使用星号（*）来表明你在创建一个引用。Swift 中这些引用与其它的常量或变量的定义方式相同。

##类和结构体的选择
在你的代码中，你可以使用类和结构体来定义你的自定义数据类型。  

然而，结构体实例总是通过值传递，类实例总是通过引用传递。这意味两者适用不同的任务。当你在考虑一个工程项目的数据构造和功能的时候，你需要决定每个数据构造是定义成类还是结构体。  

按照通用的准则，当符合一条或多条以下条件时，请考虑构建结构体：  
>结构体的主要目的是用来封装少量相关简单数据值。  
有理由预计一个结构体实例在赋值或传递时，封装的数据将会被拷贝而不是被引用。  
任何在结构体中储存的值类型属性，也将会被拷贝，而不是被引用。  
结构体不需要去继承另一个已存在类型的属性或者行为  

合适的结构体候选者包括：  
>几何形状的大小，封装一个width属性和height属性，两者均为Double类型。  
一定范围内的路径，封装一个start属性和length属性，两者均为Int类型。  
三维坐标系内一点，封装x，y和z属性，三者均为Double类型。  

在所有其它案例中，定义一个类，生成一个它的实例，并通过引用来管理和传递。实际中，这意味着绝大部分的自定义数据构造都应该是类，而非结构体。
##集合（Collection）类型的赋值和拷贝行为
Swift 中字符串（String）,数组（Array）和字典（Dictionary）类型均以结构体的形式实现。这意味着String，Array，Dictionary类型数据被赋值给新的常量(或变量），或者被传入函数（或方法）中时，它们的值会发生拷贝行为（值传递方式）。  

Objective-C中字符串（NSString）,数组（NSArray）和字典（NSDictionary）类型均以类的形式实现，这与Swfit中以值传递方式是不同的。NSString，NSArray，NSDictionary在发生赋值或者传入函数（或方法）时，不会发生值拷贝，而是传递已存在实例的引用。
>注意： 以上是对于数组，字典，字符串和其它值的拷贝的描述。 在你的代码中，拷贝好像是确实是在有拷贝行为的地方产生过。然而，在 Swift 的后台中，只有确有必要，实际（actual）拷贝才会被执行。Swift 管理所有的值拷贝以确保性能最优化的性能，所以你也没有必要去避免赋值以保证最优性能。（实际赋值由系统管理优化)