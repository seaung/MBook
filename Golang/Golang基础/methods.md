##### 方法的声明

方法的声明和普通函数的声明类似，只是在函数名字前面多了一个参数。这个参数把这个方法绑定到这个参数对应的类型上

```go
type dog {
    name string
    attr string
}

func (d dog) Running() {
    fmt.Println(d.name, d.attr)
}
```

上述代码中`(d dog)`被称之为接收者，通常的，接收者不使用特殊的名字，而是使用接收者的名字的首字母。

##### 指针接收者的方法

如果一个方法里需要更新接收者里所包含的变量或成员，或者说如果一个实参太大而我们希望避免复制整个实参，这时候我们必须使用指针来传递变量的地址。

命令类型(dog)与指针向的指针(*dog)是唯一可以出现在接收者声明的类型。而且，为了防止混淆，不允许本身是指针的类型进行方法声明

```go
type (d *dog) Rename(name string) {
    d.name = name
    fmt.Println("my name is : ", d.name)
}

type p *int
func (p) f(){}// 编译出错
```

##### nil是一个合法的接收者

就像函数允许nil指针作为实参，方法的接收者也一样，尤其是当nil是类型中有意义的零值(例如map,slice类型)时

```go
type Node struct {
    Val int
    Tail *Node
}

func (n *Node) Sum() int {
    if n == nil {
        return 0
    }
    return n.Val + n.Tail.Sum()
}
```



---

that's all
