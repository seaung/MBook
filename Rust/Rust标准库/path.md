##### `path::Path`模块

这个模块是一个路径的切片（类似于str）。

此类型(Path类型)支持许多检查路径的操作，包括将路径拆分为其组件（在Unix上由/分隔，在Windows上由/或\分隔）、提取文件名、确定路径是否为绝对路径等。

##### new方法

函数原型

```rust
pub fn new<S: AsRef<OsStr> + ?Sized>(s: &S) -> &Path
```

函数作用

直接将字符串切片包装为Path切片。这是一种无成本的转换。返回一个`Path`类型的一个引用

例子

```rust
use std::path::Path;

fn main() {
    let path = Path::new("./bar/test.txt");
    println!("the path object = {:?}", path);
}
```

输出结果

```bash
➜  rustc main.rs
➜  ./main
the path object = "./bar/test.txt"
```

##### `parent`方法

函数原型

```rust
pub fn parent(&self) -> Option<&Path>
```

函数作用

返回一个不带最终路径的Path组件（就是返回父路径），返回之类型为`Option<&Path>`的枚举类型

例子

```rust
use std::path::Path;

fn main() {
    let path = Path::new("./bar/test.txt");
    let parent_dir = let if Some(parent_dir) = path.parent() { parent_dir } else { Path::new("") };
    println!("the parent dir = {:?}", parent_dir);
}
```

输出结果

```bash
➜  rustc main.rs
➜  ./main 
the parent dir = "./bar"
```

##### `is_file`方法

函数原型

```rust
pub fn is_file(&self) -> bool
```

函数作用

检测所给定的路径是否是一个文件，如果是则返回true,否则返回false。如果检测所给定的路径不存在也会返回false。如果无法访问文件的元数据，例如由于权限错误或符号链接断开，则返回false。

例子

```rust
use std::path::Path;

fn main() {
    let path = Path::new("./bar/test.txt");
    let is_file_ok = path.is_file();
    println!("the path object is file = {}", is_file_ok);
}
```

输出结果

```bash
➜  ./main
the path object is file = true

```

##### `is_dir`方法

函数原型

```rust
pub fn is_dir(&self) -> bool
```

函数作用

如果路径存在于磁盘上并指向目录，则返回true。此函数将遍历符号链接以查询有关目标文件的信息。如果无法访问文件的元数据，例如由于权限错误或符号链接断开，则返回false。

例子

```rust
use std::path::Path;

fn main() {
    let path = Path::new("./bar/test.txt");
    let is_dir_ok = path.is_dir();
    println!("the path object is dir = {}", is_dir_ok);
}
```

输出结果

```bash
➜   rustc main.rs
➜   ./main 
the path object is dir = false
```





---

that's all