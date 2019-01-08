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


### 函数外部参数的忽略

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

### 闭包的省略形式

> 首先来一个求和函数定义，它需要传入加法函数或闭包，实现两数求和
```swift
func sum(a: Int, b: Int, addFunc: (Int, Int) -> Int) -> Int {
    return addFunc(a, b)
}
```

> 下面是闭包的常规写法
```swift
let value1: Int = sum(a: 10, b: 20, addFunc: {(a: Int, b: Int) -> Int in    // 返回 30
    return a + b
})
```

> 使用内联函数可以对参数名称进行缩写
```swift
let value2: Int = sum(a: 10, b: 20, addFunc: {$0 + $1})                     // 返回 30
```

> 传入运算符，swift会自动推断给出运算符函数实现
```swift
let value3: Int = sum(a: 10, b: 20, addFunc: +)                             // 返回 30
```

> 尾随闭包，作为函数最后一个参数调用
```swift
let value4: Int = sum(a: 10, b: 20) {$0 + $1}                               // 返回 30
```

### 闭包与函数自动捕获上下文常量或变量

> 嵌套函数中的闭包不会被自动捕获，swift3默认采用 @nonescaping标记，闭包只能在外部函数执行期间被调用
```swift
func sum(a: Int, b: Int, addFunc: (Int, Int) -> Int) -> () -> Int {
    return {return addFunc(a, b)}       // 报错
}

func sum(a: Int, b: Int, addFunc: (Int, Int) -> Int) -> Int {
    return addFunc(a, b)                    // 可以在外部函数内被执行
}
```

> 使用 @escaping标记，让嵌套函数在外部函数执行完之后仍可以得到被调用，而不会随着原域的销毁而被销毁
```swift
func sum(a: Int, b: Int, addFunc: @escaping (Int, Int) -> Int) -> () -> Int {
    return {return addFunc(a, b)}
}

let function: () -> Int = sum(a: 10, b: 20, addFunc: +)
print(function)         // 输出 (Function)
print(function())       // 输出 30
```

###  没有参数也没有返回值的函数

> 先定义一个没有参数也没有返回值的函数
```swift
func hello() {
    print("hello")
}
```

> 要声明这种类型的函数时，可以采用以下的方法
```swift
let function1: () -> () = hello
let function2: () -> Void = hello
```
