---
layout: post
title: "Swift nested types"
categories:
- Swift
tags:
- Swift


---
##官方教程
[https://developer.apple.com/library/prerelease/ios/  documentation/Swift/Conceptual/  Swift_Programming_Language/NestedTypes.html#//apple_ref/  doc/uid/TP40014097-CH23-ID242](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/NestedTypes.html#//apple_ref/doc/uid/TP40014097-CH23-ID242)

##嵌套类型
枚举类型常被用于实现特定类或结构体的功能。也能够在有多种变量类型的环境中，方便地定义通用类或结构体来使用，为了实现这种功能，Swift允许你定义嵌套类型，可以在枚举类型、类和结构体中定义支持嵌套的类型。

要在一个类型中嵌套另一个类型，将需要嵌套的类型的定义写在被嵌套类型的区域{}内，而且可以根据需要定义多级嵌套。
##嵌套类型实例
{% highlight swift linenos %}
struct BlackjackCard {
    // 嵌套定义枚举型Suit
    enum Suit: Character {
       case Spades = "♠", Hearts = "♡", Diamonds = "♢", Clubs = "♣"
    }

    // 嵌套定义枚举型Rank
    enum Rank: Int {
       case Two = 2, Three, Four, Five, Six, Seven, Eight, Nine, Ten
       case Jack, Queen, King, Ace
       struct Values {
           let first: Int, second: Int?
       }
       var values: Values {
        switch self {
        case .Ace:
            return Values(first: 1, second: 11)
        case .Jack, .Queen, .King:
            return Values(first: 10, second: nil)
        default:
            return Values(first: self.rawValue, second: nil)
            }
       }
    }

    // BlackjackCard 的属性和方法
    let rank: Rank, suit: Suit
    var description: String {
    var output = "suit is \(suit.rawValue),"
        output += " value is \(rank.values.first)"
        if let second = rank.values.second {
            output += " or \(second)"
        }
        return output
    }
}

let theAceOfSpades = BlackjackCard(rank: .Ace, suit: .Spades)
println("theAceOfSpades: \(theAceOfSpades.description)")
// 打印出 "theAceOfSpades: suit is ♠, value is 1 or 11"
{% endhighlight %}
##嵌套类型的引用
在外部对嵌套类型的引用，以被嵌套类型的名字为前缀，加上所要引用的属性名：
{% highlight swift%}
let heartsSymbol = BlackjackCard.Suit.Hearts.toRaw()
{% endhighlight %}
// 红心的符号 为 "♡"
对于上面这个例子，这样可以使Suit, Rank, 和 Values的名字尽可能的短，因为它们的名字会自然的由被定义的上下文来限定。