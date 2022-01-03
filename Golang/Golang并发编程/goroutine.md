##### Go语言中的并发

go语言中的并发指的是能让某个函数独立于其他函数运行的能力。

##### Goroutine和并行

罗伯.派克说过并发不是并行。并行是指两个或多个线程同时在不同的处理器执行代码。如果将运行时配置为使用多个逻辑处理器，则调度程序将在这些逻辑处理器之间分配 goroutine，这将导致 goroutine 在不同的操作系统线程上运行。但是，要获得真正的并行性，您需要在具有多个物理处理器的计算机上运行程序。否则，goroutine 将针对单个物理处理器并发运行，即使 Go 运行时使用多个逻辑处理器。

Go 语言层面支持的 go 关键字，可以快速的让一个函数创建为 goroutine，我们可以认为 main 函数就是作为 goroutine 执行的。操作系统调度线程在可用处理器上运行，Go运行时调度 goroutine 在绑定到单个操作系统线程的逻辑处理器中运行（P）。即使使用这个单一的逻辑处理器和操作系统线程，也可以调度数十万 goroutine 以惊人的效率和性能并发运行。

##### 开启一个Goroutine

在golang如何开启一个gorutine呢?很简单,golang提供了go关键字，你可以在函数调用前面加上go关键字,这等于说你开启了一个goroutine。例子如下

```go
func increment() {
    for i := 0; i <= 1024; i++ {
        fmt.Println("the i value of : ", i)
    }
}

func main() {
    go increment()
    
    time.Sleep(time.Second * 5)
}
```

##### 如何管控Goroutine的生周期

1. Keep yourself busy or do the work yourself(让自己忙起来，或者自己做这项事)

```go
package main

import (
    "fmt"
    "net/http"
    "log"
)

func main() {
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintln(w, "hello world")
    })
    
    go func() {
        if err := http.ListenAndServe(":8080", nil); err != nil {
            log.Fatal(err)
        }
    }()
    
    select{}
}
```

上述代码描述了我们在后台开启了一个goroutine监听http server,并使用了空select来让程序不推出。空的select语句会将永远阻塞。像这种情况，由于这goroutine是在后台运行的，我们并不知道它什么时候推出。如果一个goroutine在另一个goroutine获得结果之前无法取得进展，那么通常情况下，你自己去做这项工作比委托它更简单。这通常消除了将结果从goroutine返回到其他启动器所需要的大量状态跟踪和chan操作。

```go
package main

import (
    "fmt"
    "net/http"
    "log"
)

func main() {
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintln(w, "hello world")
    })
    
    if err := http.ListenAndServe(":8080", nil); err != nil {
       log.Fatal(err)
    }

}
```

2. Leave concurrency to the caller(让并发交给调用者)
3. Never start a goroutine without knowning when it will stop



---

that's all
