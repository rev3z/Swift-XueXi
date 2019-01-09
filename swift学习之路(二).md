## swift学习之路(二)

### 使用实例方法修改结构体或枚举的属性

> 结构体和枚举是值类型，使用变异(mutating)方法可以修改值类型的属性，同时给隐含属性self一个全新的实例
```swift
struct area {
    var length = 1
    var breadth = 1

    func area() -> Int {
        return length * breadth
    }

    mutating func scaleBy(res: Int) {
        length *= res
        breadth *= res

        print(length)
        print(breadth)
    }
}

var val = area(length: 3, breadth: 5)
val.scaleBy(res: 3)         //输出 9(换行) 15
```

### 延迟属性

> 延迟存储属性在实例化时才会计算其初始值
```swift
class sample {
    lazy var no = number()          // `var` 关键字是必须的
}

class number {
    var name = "数字"
}

var firstsample = sample()
print(firstsample.no.name)          // 输出 数字 
```

### 计算属性

> 计算属性的定义
```swift
class sample {
    var num = 300
    var middle: (Double, Double) {      // 没有定义初始值，无法重载
        get{
            return (num, num / 2)
        }
        set(axis){
            num = axis.0 + axis.1
        }
    }
}
```

> setter可以省略参数名，使用默认名称 newValue代替
```swift
class sample {
    var num = 300
    var middle: (Double, Double) {
        get{
            return (num, num / 2)
        }
        set{
            num = newValue.0 + newValue.1
        }
    }
}
```

### 属性观察器

> willSet可以使用默认名称newValue，didSet可以使用oldValue
```swift
class sample {
    var num = 300{
        willSet{
            print(newValue)
        }
        didSet{
            print(num - oldValue)
        }
    }
}
```

### 类型属性

> 关键字static定义值类型的类型属性，关键字class为类定义类型属性
```swift
struct Structname {
    static var property = 0
}

enum Enumname {
    static var property = 0
}

class Classname {
    class var property = 0
}
```
