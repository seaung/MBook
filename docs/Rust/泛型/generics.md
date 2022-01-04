##### 什么是泛型

泛型是具体类型或其他属性的抽象替代。其作用是通过将代码提取为函数来减少重复性的工作。

##### 泛型函数

所谓泛型函数就是我们定义函数时，将泛型放置在函数签名中通常用于制定参数和返回值类型的地方。它的定义语法如下所示

```rust
fn generics_fn_name<T>(params: T) -> T {
    // do something
}
```

##### 泛型结构

所谓的泛型结构就是在结构体名后的一对尖括号中声明泛型参数，然后我们就可以在结构体定义中那些通常用于指定数据类型的位置使用泛型了。定义泛型结构的语法如下所示

```rust
#[derive(Debug)]
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let point = Point{ x: 5, y: 6};
    println!("{:#?}", point);
}
```

需要注意的是上面的结构体成员的类型必须相同，都必须是T类型，不然代码会编译不通过。如果想让结构的成员的数据类型拥有不同的类型，可以使用下面的定义形式

```rust
#[derive(Debug)]
struct Point<T, U> {
    x: T,
    y: U,
}

fn main() {
    let point = Point{ x: 7.0, y: 'a'};
    println!("{:#?}", point);
}
```

##### 泛型枚举

泛型枚举的定义跟泛型结构的定义类似，可以在枚举中的变体放入泛型数据。定义如下所示

```rust
enum Option<T> {
    Some(T),
    None,
}
```

##### 泛型方法

泛型方法其实跟普通方法多了一层泛型的声明罢了，它的定义语法如下所示

```rust
#[derive(Debug)]
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn hi(&self) -> &T {
        &self.x
    }
}

fn main() {
    let p = Point{x:5, y:6};
    println!("p.x = {}", p.hi());
}
```

上述代码中，我们为结构体Point<T>定义了一个名为hi的方法，它会返回一个指向字段x中数据的引用。注意，我们必须紧跟着impl关键字声明T，以便能够在实现方法时制定类型Point<T>。通过在impl之后将T声明为泛型，Rust能够识别出Point尖括号内的类型是泛型而不是具体类型。

有时候结构体定义中的泛型参数并不总是与我们在方法签名上所使用的类型参数一致，例如下面的代码示例

```rust
struct Point<T, U> {
    x: T,
    y: U,
}

impl<T, U> Point<T, U> {
    fn hello<v, W>(self, other: Point<V, W>) -> Point<T, W> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}

fn main() {
    let p1 = Point{x: 5, y: 5.6};
    let p2 = Point{x: 5.5, y: 55};
    let p3 = p1.hello(p2);
    println!("p3.x = {} p3.y = {}", p3.x, p3.y);
}
```



---

that's all