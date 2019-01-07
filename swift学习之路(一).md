## swift学习之路(一)

### 可选参数拆包

> 使用!强制拆包
```swift
var a: Int? = 19
print(a)		// 输出 Optional(19)
print(a!)		// 输出 19
```

> !强制拆包遇到nil时会出现运行时错误，添加判断`if let num = a`可以代替`if a != nil`，而且不需要二次拆包
```swift
var a: Int? = 19

if a != nil {
	print(a!)		// 输出 19
}

if let b = a {
	print(a)		// 输出 19 自动拆包
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
sum(a: 10, b: 20) 	// 返回 30
sum(10, 20)			// 报错
```

> 如果需要忽略参数标签，需要使用下划线作为外部参数
```swift
func sum(_ a: Int, _ b: Int) -> Int {
	return a + b
}
sum(10, 20)			// 返回 30
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