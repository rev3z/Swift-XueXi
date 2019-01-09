## swift学习之路(三)

### 循环强引用

> 实例之间的循环强引用问题，可以通过弱引用或无主引用解决
```swift
class Person {
    var car: Car?
    deinit { print("Person deinit") }
}
class Car {
    var person: Person?
    deinit { print("Car deinit") }
}
var person: Person? = Person()
var car: Car? = Car()

person!.car = car
car!.person = person

person = nil                // 无输出
car = nil                   // 无输出
```

> 弱引用用于生命周期中会变为nil的实例
```swift
class Person {
    var car: Car?
    deinit { print("Person deinit") }
}
class Car {
    weak var person: Person?
    deinit { print("Car deinit") }
}
var person: Person? = Person()
var car: Car? = Car()

person!.car = car
car!.person = person

person = nil                // 输出 Person deinit
car = nil                   // 输出 Car deinit
```

> 无主引用用于初始化赋值后不再改变为nil的实例
```swift
class Person {
    var car: Car?
    deinit { print("Person deinit") }
}
class Car {
    unowned let person: Person
    init(_ person: Person) { self.person = person }
    deinit { print("Car deinit") }
}
var person: Person? = Person()

person!.car = Car(person!)

person = nil                // 输出 Person deinit(换行) Car deinit
```

### 闭包引起的循环强引用

> 通过定义捕获列表，将被捕获的self变为弱引用或无主引用
```swift
class Monitor {
    let name: String = "XX"
    lazy var show: () -> Void = { [unowned self] in
        print("hello", self.name)
        }
    deinit { print("Monitor deinit") }
}

var monitor: Monitor? = Monitor()
monitor?.show()             // 输出 hello XX
monitor = nil               // 输出 Monitor deinit
```

### 类型转换

> as 可以用于向上转型
```swift
class Subjects {
    var physics: String
    init(physics: String) {
        self.physics = physics
    }
}

class Chemistry: Subjects {
    var equations: String
    init(physics: String, equations: String) {
        self.equations = equations
        super.init(physics: physics)
    }
}
let samplechem = Chemistry(physics: "固体物理", equations: "赫兹")
let samplesub = samplechem as Subjects          // 派生类转为基类   
```

> 向下转型不一定会成功，所以必须使用 as? 和 as! 
```swift
class Subjects {
    var physics: String
    init(physics: String) {
        self.physics = physics
    }
}

class Chemistry: Subjects {
    var equations: String
    init(physics: String, equations: String) {
        self.equations = equations
        super.init(physics: physics)
    }
}
var samplesub: Subjects = Chemistry(physics: "固体物理", equations: "赫兹")
var samplechem = samplesub as? Chemistry            // 基类转为派生类

samplesub = Subjects(physics: "固体物理")
samplechem = samplesub as? Chemistry                // nil

samplesub: Subjects = Chemistry(physics: "固体物理", equations: "赫兹")
samplechem = samplesub as Chemistry                 // 报错，向下转型不能直接用 as
```

### 可变方法mutating

> 之前有提到 mutating 用于值类型的属性修改
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
val.scaleBy(res: 3)                     // 输出 9(换行) 15
```

> 可变方法也可以用于在扩展中添加实例方法修改实例本身
> #扩展本身只能添加不能重写已有功能，存储类型属性不属于功能，所以不能添加
```swift
extension Double {
    mutating func square() {
        let pi = 3.1415
        self = pi * self * self
    }
}

var Trial = 3.3
Trial.square()
print("圆的面积为: \(Trial1)")           // 输出 圆的面积为: 34.210935
```
