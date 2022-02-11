#### 错误和异常

在golang中错误被认为是一种可以预期的结果，而异常则是一种非预期的结果

#### Errors

在Golang中Error是一个普通的接口，普通的值。我们可以在go的源码中找到error的定义

```go
// http://golang.org/pkg/builtin/#error
type error interface {
    Error() string
}
```

#### Error VS Exceptions

golang与其他编程相比它支持多参数返回，不像JAVA，C++，python等需要提供特殊的语句块来捕获处理异常。在Golang中通常情况下，函数的返回的最后一个参数是error，并且调用者调用了一个具有(value, error)返回值时，这是函数的调用者不能对这个value进行任何假设，必须先判定这个error的值。

#### Panic

golang中有panic这种机制，如果程序抛出Panic了这意味着程序挂了。不能假设调用者来解决panic，意味着代码不能继续执行。

#### 什么时候使用panic?

对于正真意外的情况，那些表示不可恢复的程序错误，例如索引越界，不可恢复的环境问题，栈溢出等问题时我们才使用Panic。对与其他情况的错误，我们应该期望使用error来进行判断。

#### 为什么是Errors而不是异常

1. 简单。
2. 应考虑失败的情况而不是成功(成功只是一时的，但是失败可能是持续的)。
3. 没有隐藏的控制流(不像JAVA,C++或python需要用特定的语句块来处理意想不到的异常情况)。
4. 错误处理完全交给程序员自个来控制。
5. Error are value(错误只是一个普通的值)。

#### 使用Errors的常用姿势

1. Sentinel Error

   Sentinel error也称为哨兵错误，golang中允许我们使用特定的值来表示错误。一般是这样使用的，调用这将某个函数返回的错误值与预定义中的错误值进行等值比较。

   ```go
   var NotFound = errors.New("the page not found")
   
   if err == notFound {
       // do something
   }
   ```

   优点：程序员可自己预先定义一些错误类型

   缺点：不能携带一些上下文信息，或者使用fmt.Errof()附加一些上下文信息时，会破坏原有的错误，破坏调用者的等值判定。增大了API的表面积，使得API变得脆弱，在包与包之间创建了依赖。

2. Error type

   Error type所想要做的事情是，程序员自定义一个错误类型，并实现error接口的Error方法即可。调用方在使用error时，对函数返回的错误做断言处理。

   ```go
   type MyError struct {
       Msg  string
       File string
       Line int
   }
   
   func (m *MyError) Error() string {
       return fmt.Sprintf("%s:%d:%s", e.Msg, e.Line, e.File)
   }
   
   func test() error {
       return &MyError{"testing error", "test.go", 12}
   }
   
   func main() {
       err := test()
       switch err := err.(type) {
           case nil:
           // do something
           case *MyError:
           // do something
           default:
           // do something
       }
   }
   ```

   优点：可以携带更多的上下文信息

   缺点：调用者需要使用类型断言和类型switch将这个错误转换为我们自定的错误类型，以获取这个错误的具体信息。自定义的错误类型需要变为public。这种模型会导致和调用方产生强耦合，从而导致API变得脆弱。

3. 不透明的error处理方法

   这种错误处理方式是，程序员直接将错误进行返回，不添加任何的操作。

   ```go
   if err != nil {
       // do something
   }
   ```

   优点：不依赖任何类型和行为

   缺点：代码上会比较啰嗦

4. 对外暴露行为，而不是错误的类型

   这种错误处理方式的做法是对外不暴露细节，而是对外暴露行为(方法)。这个对外暴露的接口方法通常在这个方法里进行断言操作(即断言一个行为)，最终返回一个布尔值，供调用方判定是否成功或失败。

   ```go
   type OpError struct {
       Op  string
       Err error
   }
   
   type temporary interface {
       Temporary() bool
   }
   
   func (e *opError) Temporary() bool {
       if e.Op == "accept" && isConnErr(e.Err) {
           return true
       }
       if ne, ok := e.Err.(*os.SyscallError); ok {
           t, ok := ne.Err.(temporary)
           return ok && t.Temporary()
       }
       t, ok := e.Err.(temporary)
       return ok, && t.Temporary()
   }
   
   func IsTemporary(e error) bool {
       t, ok := e.(temporary)
       return ok && t.Temporary()
   }
   ```

   优点：只断言错误的行为，而不是类型

   缺点：暴露了方法



---

that's all

