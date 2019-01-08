## swift学习之路(一)

### 可选参数拆包

> 使用!强制拆包
```swift
var a: Int? = 19
print(a)        // 输出 Optional(19)
print(a!)       // 输出 19
```

> !强制拆包遇到nil时会出现运行时错误，添加判断`if let num = a`可以代替`if a != nil`，而且不需要二次拆包
```swift
var a: Int? = 19

if a != nil {
    print(a!)       // 输出 19
}

if let b = a {
    print(a)        // 输出 19 自动拆包
}
```


### 函数可变参数的忽略

> 首先来一个求和函数的定义：
```swift
func sum(a: Int, b: Int) -> Int {
    return a + b
}
```

> 该求和函数调用需要给定参数标签：
```swift
sum(a: 10, b: 20)  // 返回 30
sum(10, 20)	        // 报错
```

> 如果需要忽略参数标签，需要使用下划线作为外部参数
```swift
func sum(_ a: Int, _ b: Int) -> Int {
    return a + b
}
sum(10, 20)	        // 返回 30
```

> 使用函数变量，外部参数会被自动忽略
```swift
func sum(a: Int, b: Int) -> Int {
    return a + b
}
let addition: (Int, Int) -> Int = sum
addition(a: 10, b: 20)	// 报错
addition(10, 20)		// 返回 30
```

> 闭包的省略形式
```swift
func sum(a: Int, b: Int, addFunc: (Int, Int) -> Int) -> Int {
    return addFunc(a, b)
}

let value1: Int = sum(a: 10, b: 20, addFunc: {(a: Int, b: Int) -> Int in    // 返回 30 普通闭包形式
    return a + b
})
let value2: Int = sum(a: 10, b: 20, addFunc: {$0 + $1})                     // 返回 30 内联函数参数名称缩写
let value3: Int = sum(a: 10, b: 20, addFunc: +)                             // 返回 30 参数符函数
let value4: Int = sum(a: 10, b: 20) {$0 + $1}                               // 返回 30 尾随闭包
```

> 闭包与函数自动捕获上下文常亮或变量，@escaping标记让嵌套函数可以在外部函数执行完之后调用
```swift
func sum(a: Int, b: Int, addFunc: @escaping (Int, Int) -> Int) -> () -> Int {
    return {return addFunc(a, b)}
}

let function: () -> Int = sum(a: 10, b: 20, addFunc: +)
print(function)         // 输出 (Function)
print(function())       // 输出 30

func sum(a: Int, b: Int, addFunc: (Int, Int) -> Int) -> () -> Int {
    return {return addFunc(a, b)}       // 报错 swift3默认对函数参数采用 @nonescaping标记
}
```

> 没有参数也没有返回值的函数，可以使用如下声明
```swift
func hello() {
    print("hello")
}
let function1: () -> () = hello
let function2: () -> Void = hello
let function3 = hello
```
