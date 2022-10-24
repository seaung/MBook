##### Golang中的反射

在编译时不知道类型的情况下，可更新变量，在运行时查看值，调用方法以及值接对它们的布局进行操作，这种机制就叫反射。

##### reflect.Type和reflect.value

Golang提供了reflect包，这个包定义了两种重要的类型: `reflect.Type 和reflect.Value`。Type表示Go语言的一个类型，它是一个很多方法的接口，这些方法可以用来识别类型以及透视类型的组成部分，例如一个结构的各个字段或函数的各个参数等。

`reflect.Type`接口只有一个实现，即类型描述符，接口值中的动态类型也是类型描述符

`reflect.TypeOf`函数接受任何的interface{}参数，并且把接口中的动态类型以`reflect.Type`形式返回

```go
import "reflect"

t := reflect.TypeOf(3)
println(t.String()) // "int"
println(t) // "int"
```

Note: 

1. 把一个具体值赋给一个接口类型时会发生一个隐式类型转换，转换会生成一个包含两部分内容的接口值: 动态类型部分是操作数的类型，动态值部分是操作数的值
2. `reflect.TypeOf`返回一个接口值对应的动态类型，所以它返回总是具体类型(而不是接口类型)

reflect包另一重要的类型Value。`reflect.Value`可以包含一个任意类型的值。`reflect.ValueOf`函数接受任意的interface{}并将接口的动态值以`reflect.Value`的形式返回。与`reflect.TypeOf`类似，`reflect.ValueOf`的返回值也都是具体值，不同的是`refelct.Value`也可以包含一个接口值

```go
v := reflect.ValueOf(2)
println(v) // 2
println(v.String()) // "<int Value>"
```



---

that's all
