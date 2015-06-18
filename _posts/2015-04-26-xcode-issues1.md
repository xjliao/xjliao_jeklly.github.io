---
layout: post
title: "Xcode issues"
categories:
- xcode 
tags:
- xcode 


##


---
##官方教程
[https://developer.apple.com/library/prerelease/ios/  documentation/Swift/Conceptual/  Swift_Programming_Language/Subscripts.html#//apple_ref/  doc/uid/TP40014097-CH16-ID305](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Subscripts.html#//apple_ref/doc/uid/TP40014097-CH16-ID305)

##下标脚本语法
{% highlight swift linenos %}
subscript(index: Int) -> Int {
    get {
      // 返回与入参匹配的Int类型的值
    }

    set(newValue) {
      // 执行赋值操作
    }
}

subscript(index: Int) -> Int {
    // 返回与入参匹配的Int类型的值
}
{% endhighlight %}

demo
{% highlight swift linenos %}
struct TimesTable {
    let multiplier: Int
    subscript(index: Int) -> Int {
      return multiplier * index
    }
}
let threeTimesTable = TimesTable(multiplier: 3)
println("3的6倍是\(threeTimesTable[6])")
// 输出 "3的6倍是18"
{% endhighlight %}
##下标脚本用法
{% highlight swift linenos %}
var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
numberOfLegs["bird"] = 2
{% endhighlight %}
注意：
>Swift 中字典的附属脚本实现中，在get部分返回值是Int?，上例中的numberOfLegs字典通过附属脚本返回的是一个Int?或者说“可选的int”，不是每个字典的索引都能得到一个整型值，对于没有设过值的索引的访问返回的结果就是nil；同样想要从字典实例中删除某个索引下的值也只需要给这个索引赋值为nil即可

##下标脚本选项
{% highlight swift linenos %}
struct Matrix {
    let rows: Int, columns: Int
    var grid: [Double]
    init(rows: Int, columns: Int) {
      self.rows = rows
      self.columns = columns
      grid = Array(count: rows * columns, repeatedValue: 0.0)
    }
    func indexIsValidForRow(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }
    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValidForRow(row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValidForRow(row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}

var matrix = Matrix(rows: 2, columns: 2)

// 示意图
grid = [0.0, 0.0, 0.0, 0.0]

      col0  col1
row0   [0.0,     0.0,
row1    0.0,  0.0]

matrix[0, 1] = 1.5
matrix[1, 0] = 3.2

[0.0, 1.5,
 3.2, 0.0]
{% endhighlight %}
