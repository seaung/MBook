##### 通过通信共享内存

在传统的线程模型中程序在线程之间通信需要使用共享内存。通常，共享数据结构有锁来保护，线程将争用锁来访问数据。在某些情况下，通过使用线程安全的数据，这样会变得更容易。

Go的并发原语goroutines和channels为构造并发软件提供了一种优雅独特的方法。Go没有显式的使用锁来协调对共享数据的访问，而是鼓励使用chan在goroutine之间传递数据的引用。这种方法确保在给定的时间只有一个goroutine可以访问数据。

##### 数据竞争

data race(数据竞争)是两个或多个goroutine访问同一个资源，并尝试对该资源进行读写而不考虑其他goroutine。

go1.1中引入了race detector。竞争检测器在构建过程中内置到程序中的代码。然后，一旦你的程序运行，它就能够检测并报告它发现的任何竞争条件。

命令如下所示:

`go build -race or go test -race`

数据竞争例子，代码如下:

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

var (
    wg sync.WaitGroup
    Counter int = 0
)

func main() {
     for routine := 1; routine <= 2; routine++ {
        wg.Add(1)
        go Routine(routine)
    }
    wg.Wait()
    fmt.Println("Current Counter : ", Counter)
}

func Routine(id int) {
    for count := 0; count < 2; count++ {
        value := Counter
        // time.Sleep(1 * time.Nanosecond)
        value++
        Counter = value
    }
    wg.Done()
}
```

##### sync.atomic









---

that's all