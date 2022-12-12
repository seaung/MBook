##### Rust错误处理

Rust中将错误分为两大类: 可恢复错误和不可恢复错误。对于可恢复错误，例如文件未找到等，应当将此错误报告给用户。而对于不可恢复的错误，例如访问数组越界等。

##### 不可恢复错误与panic

Rust提供了一种特殊的宏函数`panic`.程序会在`panic`宏执行时打印出一段错误提示信息，展开并清理当前的调用栈，然后推出程序。

1. panic的展开与终止

   当panic发生时，程序会默认开始栈展开。这意味着rust会沿着调用栈的反向顺序遍历所有调用函数，并依次清理这些函数中的数据。如果我们所开发的项目为了使最终的二进制包尽可能小，那么我们可以在`Cargo.toml`文件中的`[profile]`区域添加`panic='abort'`来将panic的默认行为从展开切换为终止。

2. 设置`RUST_BACKTRACE`变量可以回溯和确定触发错误的原因

   `RUST_BACKTRACE=1 cargo run`

##### 可恢复错误

1. Rust内部提供了`Result`这种类型的枚举来处理可能失败的情况。

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

这里的T和E都表示泛型，T代表了Ok变体中包含的值的类型，该变体中的值会在执行成功时返回。而E代表了Err变体中包含错误类型，该变体中的值会在执行失败时返回。

以一个打开文件的一段代码为例

```rust
use std::fs::File;

fn main() {
    let f = File::open("test.txt");
    
    let f = match f {
        Ok(file) => file,
        Err(err) => {
            panic!("can't open the test.txt!")
        },
    };
}
```

2. 失败时出发panic的快捷方式: `unwrap`和`expect`

```rust
use std::fs:File;

fn main() {
    let f = File::open("test.txt").unwrap();
    
    let fs = File::open("test.txt").expect("can't open the file!");
}
```

3. 错误的传播

```rust
use std::io;
use std::io::Read;
use std::fs::File;

fn read_file() -> Result<String, io::Error> {
    let f = file::open("test.txt");
    
    let mut f = match f {
        Ok(file) => file,
        Err(err) => return Err(err),
    };
    
    let mut s = String::new();
    match f.read_to_string(&mut s) {
        Ok(_) => Ok(s),
        Err(e) => Err(e),
    }
}
```

4. 使用`？`运算符简化错误处理操作

```rust
use std::io;
use std::io::Read;
use std::fs::File;

fn read_file() -> Result<String, io::Error> {
    let mut s = String::new();
    
    File::open("test.txt")?.read_to_string(&mut s)?;
    
    Ok(s)
}
```

需要注意的一点是`？`运算符只能被用于返回Result的函数





---

that's all