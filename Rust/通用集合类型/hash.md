##### 哈希映射

Rust中的哈希映射这种数据结构其实跟其他语言一样都是存储key和value的，例如python的字典。

##### 创建一个哈希映射

1. 使用`new()`方法创建哈希映射

```rust
use std::collections::HashMap;

let mut dict = HashMap::new();

dict.insert(String::from("Red"), 0);
dict.insert(String::from("Yellow"), 1);
dict.insert(String::from("Blue"), 2);

println!("the dict red attr of {}", dic["Red"]);
```

2. 使用`collect`方法来将动态数组转换为哈希映射

```rust
use std::collections::HashMap;


let items = vec![String::from("Red"), String::from("Yellow"), String::from("Blue")];
let init_value = vec![1, 2, 3];

let result: HashMap<_, _> = items.iter().zip(init_value.iter()).collect();

println!("the result red value of {}", result["Red"]);
```

##### 哈希映射与所有权

对于那些实现了`Copy`的trait类型，例如i32，它们的值会被简单地复制到哈希映射中。而对于像`String`这种持有所有权的值，其值将会转移且所有权会转移到哈希映射中。

```rust
use std::collections::HashMap;

let first = String::from("red");
let second = String::from("blue");

let mut item = HashMap::new();
item.insert("one", first);
item.insert("tow", second);
// first 和second将不再有效，因为他们的所有权发生了转移
println!("the item value of {}", item["one"]);
```

##### 访问哈希映射

1. 使用`get`方法访问哈希映射

```rust
use std::collections::HashMap;

let mut items = HashMap::new();
items.insert(String::from("Red"), 0);
items.insert(String::from("Blue"), 1);

let value = String::from("Red");
let val = items.get(&value);
println!("the val value of {}", val);
```

使用`get`方法会返回一个`Option<&V>`枚举

1. 使用键来访问哈希映射

```rust
use std::collections::HashMap;

let mut items = HashMap::new();
items.insert(String::from("Red"), 0);
items.insert(String::from("Blue"), 1);

println!("the value of {}", items["Red"]);
```

##### 遍历哈希映射

```rust
use std::collections::HashMap;

let mut items = HashMap::new();
items.insert(String::from("Red"), 0);
items.insert(String::from("Blue"), 1);

for (key, value) in &items {
    println!("the key {} - the value {}", key, value);
}
```



##### 更新哈希映射









---

that's all