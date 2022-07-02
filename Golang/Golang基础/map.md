##### map简介

在Go中map是散列表的引用，map类型是`map[k]v`,其中k和v是字典的键和值对应的数据类型。map中所有的键都必须是相同的类型，值可以是相同的数据类型，但也可以是不同的数据类型。**键的类型，必须是可以通过操作符`==`来进行比较的数据类型**，所有map可以检测某一个键是否已经存在。

##### 创建map

常用创建map方式有以下两种

```go
// 使用内建函数make
ages := make(map[string]int)
ages["golang"] = 12

// 使用字面量的方式创建map
numbers := map[string]int {
    "python": 12,
    "java": 13
}
```

##### 使用map

```go
// 访问map,通过下标的方式
println(ages["golang"]) // 通过键名来访问对应的值

// 移除一个元素
delete(numbers, "java") // 移除键为java元素

// 操作键对应的值
ages["golang"] +=1
ages["golang"] ++
ages["golang"] + 1

// _ = &ages["golang"] map元素不是一个变量，不可以获取他的地址
```

##### 遍历map

```go
for k, v := range ages{
    println(k, v)
}
```

##### 判断map元素是否存在

```go
age, ok := ages["rust"]
if !ok {
    println("age == ", age)
}

age, ok := ages["rust"]; !ok {
    println("age == ", age)
}
```

##### 小结

1. map中即使键不存在也是安全的，即对应的键不存在，就会返回值类型的零值。
2. 我们无法获取一个map元素的地址，因为map的增长可能会导致已有的元素被重新散列到新的位置，这样就可能使得获取的地址无效。
3. map中元素的迭代顺序是不固定的。
4. map类型的零值是nil，即没有引用任何散列表。
5. 设置元素之前，必须初始化map。



---

that's all





