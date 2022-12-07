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

##### 遍历动态数组

```rust
let v = vec![1, 2, 3];

for item in &v {
    println!("the item value of {}", item);
}
```







---

that's all