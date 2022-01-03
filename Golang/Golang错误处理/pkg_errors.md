##### 包装错误(wrap errors)

为什么我们需要对错误进行包装，让我们用一个例子来解释解释一下吧，代码例子如下

```go
func AuthenticationRequest(r &Request) error {
    err := authenticate(r.User)
    if err != nil {
        return fmt.Errorf("authenticate failed : %v", err)
    }
    return nil
}
```

上述代码为了让错误信息携带更多的上下文使用了fmt.Errorf()函数对其原有的错误进行了二次修改，破坏了原有的错误值。调用用者此函数做等值判断时可能会出现意想不到的事情。

结论:

1. 你应该只处理错误一次。处理错误意味着检查错误值，并做出单一的决策(要么记录错误，要么处理错误)。
2. 日志记录与错误无关且对调试没有帮助的信息应该被视为噪音，应予以质疑。记录的原因是因为某些东西失败了，而日志包含了答案。
3. 错误应该被日志记录。
4. 应用程序处理错误，应保证100%完整性。
5. 之后不再报告当前错误。

##### pkg/errors包

上面描述了如果发生错误时，我们想对原错误添加更多的上下文信息事会对原有的错误值造成破坏。所以我们这时可以通过使用pkg/erros包，这个包可以在不破坏原有错误值的情况下对错误值添加上下文信息，这种方式既可以由人也可以有机器检查。

pkg/erros使用的小技巧

1. 在应用代码中，可使用erros.New或者erros.Errorf返回错误

   ```go
   import "github.com/pkg/errors"
   
   func parseArgs(args []string) error {
       if len(args) < 2 {
           return errors.Errorf("....")
       }
       // do something
   }
   ```

2. 如果调用其他包内函数，通常简单的做法是直接返回(其实是避免对原有错误进行二次包装)

   ```go
   // do something
   if err != nil {
       return err
   }
   // do something
   ```

3. 如果和其他包(github上的包等)进行协作，考虑使用errors.Wrap或者errors.Wrapf保存堆栈信息。这同样适用于和标准库协作的时候

   ```go
   import (
       "github.com/pkg/erros"
       "os"
   )
   
   fs, err := os.Open(path)
   if err != nil {
       return errors.Wrapf(err, "failed to open %q", path)
   }
   ```

4. 直接将错误返回，而不是在每个错误产生的地方到处打日志

5. 在程序的顶部或者是工作的goroutine顶部(请求入口)，使用`%+v`把堆栈详情记录

   ```go
   func main() {
       err := app.Run()
       
       if err != nil {
           fmt.Printf("Error : %v", err)
           os.Exit(1)
       }
   }
   ```

6. 使用errors.Cause获取root error，再进行和sentinel error判定

小总结：

1. 跨多个项目可重用的包只返回根错误值。选择wrap error是只有application可以选择的策略。具有最高可重用性的包只能返回根错误。此机制与Go标准库中使用的相同(kit库中的sql.ErrNoRows)
2. 如果你不打算处理错误，则包装它并返回调用堆栈。这个是关于函数/方法调用返回值的每个错误的基本问题。如果函数/方法不打算处理错误，那么用足够的上下文wrap errors并将其返回到调用堆栈中。例如，额外的上下文信息可以是使用的输入参数或失败的查询语句。确定你记录的上下文是足够多还太多的一个好方法是检查日志并验证它们在开发期间是否为您工作
3. 一旦错误被处理了，就不允许再向上传递调用堆栈信息了。一旦确定函数/方法将处理错误，错误就不再是错误。如果函数/方法仍然需要返回，则它不能返回错误值。它应该只返回零(比如降级处理中，你返回了降级数据，然后需要返回nil)

##### Go1.13和pkg/erros包的结合

Golang在1.13的版本中为errors和fmt标准库中引入了新的特性，以简化处理包含其他错误的错误。其中最重要的是:包含另一个错误的error可以实现返回底层错误的UnWrap方法。如果e1.UnWrap返回e2，那么我们说e1包装e2，可以展开e1以获取e2

Golang1.13还新添加了两个新的用于检测错误的新函数:Is和As



---

that's all