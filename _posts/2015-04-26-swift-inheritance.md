---
layout: post
title: "Swift inheritance"
categories:
- Swift
tags:
- Swift


---
##官方教程
[https://developer.apple.com/library/prerelease/ios/  documentation/Swift/Conceptual/  Swift_Programming_Language/Inheritance.html#//apple_ref/  doc/uid/TP40014097-CH17-ID193](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Inheritance.html#//apple_ref/doc/uid/TP40014097-CH17-ID193)

##定义一个基类
{% highlight swift linenos %}
class Vehicle {
    var currentSpeed = 0.0
    var description: String {
        return "traveling at \(currentSpeed) miles per hour"
    }
    func makeNoise() {
        // 什么也不做-因为车辆不一定会有噪音
    }
}

let someVehicle = Vehicle()
println("Vehicle: \(someVehicle.description)")
// Vehicle: traveling at 0.0 miles per hour
{% endhighlight %}
##子类生成
为了指明某个类的超类，将超类名写在子类名的后面，用冒号分隔：
{% highlight swift linenos %}
class SomeClass: SomeSuperclass {
    // 类的定义
}
{% endhighlight %}
demo
{% highlight swift lineos %}
class Bicycle: Vehicle {
	var hasBasket = false
}

let bicycle = Bicycle()
bicycle.hasBasket = true

bicycle.currentSpeed = 15.0
println("Bicycle: \(bicycle.description)")

class Tandem: Bicycle {
	var curerntNumberOfPassengers = 0
}

let tandem = Tandem()
tandem.hasBasket = true
tandem.currentNumberOfPassengers = 2
tandem.currentSpeed = 22.0
println("Tandem: \(tandem.description)")
{% endhighlight %}
##重写（Overriding）
子类可以为继承来的实例方法（instance method），类方法（class method），实例属性（instance property），或下标脚本（subscript）提供自己定制的实现（implementation）。我们把这种行为叫重写（overriding）。

如果要重写某个特性，你需要在重写定义的前面加上override关键字。这么做，你就表明了你是想提供一个重写版本，而非错误地提供了一个相同的定义。意外的重写行为可能会导致不可预知的错误，任何缺少override关键字的重写都会在编译时被诊断为错误。

override关键字会提醒 Swift 编译器去检查该类的超类（或其中一个父类）是否有匹配重写版本的声明。这个检查可以确保你的重写定义是正确的。

访问超类的方法，属性及下标脚本

当你在子类中重写超类的方法，属性或下标脚本时，有时在你的重写版本中使用已经存在的超类实现会大有裨益。比如，你可以优化已有实现的行为，或在一个继承来的变量中存储一个修改过的值。

在合适的地方，你可以通过使用super前缀来访问超类版本的方法，属性或下标脚本：

在方法someMethod的重写实现中，可以通过super.someMethod()来调用超类版本的someMethod方法。
在属性someProperty的 getter 或 setter 的重写实现中，可以通过super.someProperty来访问超类版本的someProperty属性。
在下标脚本的重写实现中，可以通过super[someIndex]来访问超类版本中的相同下标脚本。
##重写方法
{% highlight swift linenos %}
class Train: Vehicle {
	override func makeNoise() {
		println("Choo Choo")
	}
}

let train = Train()
train.makeNoise()
{% endhighlight %}
##重写属性
你可以重写继承来的实例属性或类属性，提供自己定制的getter和setter，或添加属性观察器使重写的属性观察属性值什么时候发生改变
####重写属性的Getters和Setters
注意：
>如果你在重写属性中提供了 setter，那么你也一定要提供 getter。如果你不想在重写版本中的 getter 里修改继承来的属性值，你可以直接通过super.someProperty来返回继承来的值，其中someProperty是你要重写的属性的名字。
{% highlight swift linenos %}
class Car: Vehicle {
    var gear = 1
    override var description: String {
        return super.description + " in gear \(gear)"
    }
}

let car = Car()
car.currentSpeed = 25.0
car.gear = 3
println("Car: \(car.description)")
{% endhighlight %}

##重写属性观察器
注意：
>你不可以为继承来的常量存储型属性或继承来的只读计算型属性添加属性观察器。这些属性的值是不可以被设置的，所以，为它们提供willSet或didSet实现是不恰当。此外还要注意，你不可以同时提供重写的 setter 和重写的属性观察器。如果你想观察属性值的变化，并且你已经为那个属性提供了定制的 setter，那么你在 setter 中就可以观察到任何值变化了。
{% highlight swift linenos %}
class AutomaticCar: Car {
    override var currentSpeed: Double {
        didSet {
            gear = Int(currentSpeed / 10.0) + 1
        }
    }
}

let automatic = AutomaticCar()
automatic.currentSpeed = 30
println("AutomaticCar: \(automatic.description)")
{% endhighlight %}
##防止重写
你可以通过把方法，属性或下标脚本标记为final来防止它们被重写，只需要在声明关键字前加上@final特性即可。（例如：final var, final func, final class func, 以及 final subscript）

如果你重写了final方法，属性或下标脚本，在编译时会报错。在扩展中，你添加到类里的方法，属性或下标脚本也可以在扩展的定义里标记为 final。

你可以通过在关键字class前添加final特性（final class）来将整个类标记为 final 的，这样的类是不可被继承的，否则会报编译错误。