##### Rust动态数组

rust中所谓的动态数组就是允许你在单个数据结构中存储多个相同类型的值，这些值会彼此相邻地排布在内存当中。

##### 创建动态数据

rust中有两种方式来创建动态数组

1. 使用内建的标准提供的`Vec`来创建，语法如下:

   ```rust
   let v: Vec<i32> = Vec::new();
   ```

2. 使用`vec!`宏来创建动态数组，语法如下:

   ```rust
   let v = vec![1, 2, 3];
   ```

##### 更新动态数组

向动态数组追加元素，我们可以使用`push`方法

```rust
let mut v: Vec<i32> = Vec::new();

v.push(1);
v.push(2);
v.push(3);
```

##### 访问动态数组

使用索引和`get`方法访问动态数组

1. 使用索引

```rust
let v = vec![1, 2, 3];

let one: &i32 = &v[0];

println!("the one value of {}", one);
```

2. 使用`get`方法

```rust
let v = vec![1, 2, 3];

match v.get(2) {
    Some(value) => println!("the vlaue of {}", value),
    None => println!("there is no value element!"),
}
```

3. 使用索引和`get`方法获取容器元素的区别

   使用索引获取元素会直接返回元素的引用(以&v[0]的方式)

   使用索引获取不存在的元素时会导致程序panic,而使用`get`方法访问不存在的元素时则会返回`None`

   使用`get`方法则会返回一个`Option<&T>`

4. 这是一个在存在指向动态数组元素的引用时尝试向动态数组中添加元素的例子，这个例子会编译不通过，代码和出错误信息如下

```rust
let mut v = vec![1, 2, 3];
let first = &v[0];
v.push(4);
println!("the first value of {}", first);
```

```bash
   Compiling playground v0.0.1 (/playground)
error[E0502]: cannot borrow `v` as mutable because it is also borrowed as immutable
 --> src/main.rs:4:5
  |
3 |     let first = &v[0];
  |                  - immutable borrow occurs here
4 |     v.push(4);
  |     ^^^^^^^^^ mutable borrow occurs here
5 |     println!("the first value of {}", first);
  |                                       ----- immutable borrow later used here

For more information about this error, try `rustc --explain E0502`.
error: could not compile `playground` due to previous error
```

导致这个例子出错的原因是跟动态数组的工作原理有关。动态数组中的元素是连续存储的，插入新的元素后也许就没有足够多的空间将所有元素依次相邻的放下，这就需要分配新的内存空间，并将旧的元素移动到新的空间上。在这个例子中，第一个元素的引用可能会因为插入行为而指向被释放的内存。但是我们可以使用借用来规避这些问题。

##### 遍历动态数组

```rust
let v = vec![1, 2, 3];

for item in &v {
    println!("the item value of {}", item);
}

// 如果需要改变动态数组元素的之请加上mut,如下
let mut vs = vec![1, 2, 3];
for item in &mut vs {
    *item += 2;
}
```

##### 使用枚举来存储不同类型的值

虽然说动态数组里只能存储同样类型的元素，但是rust允许我们在动态数组中存放枚举类型，我们可以在枚举成员里定义不同类型的成员，从而达到可以存储不同类型的数据，代码例子如下所示:

```rust
fn main() {
    #[derive(Debug)]
    enum SpreadsheetCell {
        Int(i32),
        Float(f64),
        Text(String),
    }

    let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
    ];
    
    println!("the row 0 of {:?}", &row[0]);
}
```

输出结果如下:

```bash
the row 0 of Int(3)
```

##### 注意点

动态数组一旦离开作用域就会被销毁，这样导致的结果是连同动态数组的元素也会随之被销毁。



---

that's all