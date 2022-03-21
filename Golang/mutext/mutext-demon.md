##### 临界区
在并发编程中，如果程序中的一部分会被并发访问或修改，那么，为了避免并发访问导致的意想不到的结果，这部分程序需要被保护起来，这部分被保护起来的程序，就叫临界区。可以说临界区就是一个被共享的资源，或者说是一个整体的一组共享资源，例如对数据库的访问，对某个数据结构的操作，I/O设备的使用等等。


##### 并发同步原语常见使用场景
1. 共享资源
并发的读写共享资源，会出现数据竞争的问题，所以需要Mutex，RWMutex这样的并发原语

2. 任务编排
需要goroutine按照一定的规律执行，而goroutine之间相互等待会者依赖的顺序关系，通常会使用WaitGroup或者channel来实现

3. 消息传递
信息交流以及不同的goroutine之间的线程安全的数据交流通常使用channel来实现


##### Mutex的基本使用方法
在Golang标准库中，sync包中提供了锁相关的一系列同步原语，这个包还定义了Locker接口，Mutex就实现了这个接口

```go
type Locker interface {
	Lock()
	Unlock()
}

...

func (m *Mutex) Lock()
func (m *Mutex) Unlock()
```

> 当一个goroutine通过调用Lock方法获得了这个锁的拥有权后，其他请求锁的goroutine就会阻塞在Lock方法的调用上，知道锁被释放并且自己获取到了这个锁的拥有权


##### Go race detector
1. Go race detector的作用
Go race detector是基于Google的C/C++技术实现的，编译器通过探测锁有的内存访问，加入代码能监视对这些内存地址的访问(读/写)
在代码运行的时候，race detector就能监控到对共享变量的非同步访问，出现race的时候，就会打印出警告信息

2. Go race detector的基本使用
```go
go run -race main.go

go tool compile -race -S main.go # 查看race汇编代码
```

3. Go race detector优缺点
race detector使用起来很方便，能够检测哪些代码中存在data race。但是，因为它的实现方式，只能通过对真正实际地址进行读写访问时才能探测，所以它并不能在编译的时候发现data race的问题。而且，在运行的时候，只有在触发了data race之后，才能检测到。


##### 数据竞争例子和优化
下面是一个存在数据竞争的代码例子，count是一个共享资源，我们开启了10个goroutine对其进行访问和修改。因为存在数据竞争，这导致的问题就是无法保证数据的准确性


```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var count = 0
	var wg sync.WaitGroup

	wg.Add(10)

	for i := 0; i < 10; i++ {
		go func(){
			defer wg.Done()
			for j := 0; j < 100000; j++ {
				count++
			}
		}()
	}
	wg.Wait()
	fmt.Printf("Current count value of : %d\n", count)
}
```


如何改进这块代码?这就需要使用到Mutex进行加锁访问了

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var mu sync.Mutex
	var count = 0
	var wg sync.WaitGroup

	wg.Add(10)

	for i:= 0; i < 10; i++ {
		go func(){
			defer wg.Done()
			for j := 0; j < 100000; j++ {
				mu.Lock()
				count++
				mu.Unlock()
			}
		}()
	}

	wg.Wait()
	fmt.Printf("Current count value of : %d\n", count)
}
```


##### Mutex的其他使用姿势
1. 使用结构体，嵌入mutex
一般会把Mutex放在要控制的字段的上面，然后用空格把字段分隔开来

```go
package main

import (
	"fmt"
	"sync"
)

type Counter struct {
	mu sync.Mutex
	Count int64
}

func main() {
	var couter Counter
	var wg sync.WaitGroup

	wg.Add(10)

	for i := 0; i < 10; i++ {
		go func() {
			defer wg.Done()
			for j := 0; j < 100000; j++ {
				couter.mu.Lock()
				couter.Count++
				couter.mu.Unlock()
			}
		}()
	}

	wg.Wait()
	fmt.Printf("Current counter value of : %d\n", couter.Count)
}
```


2. 封装Mutex
```go
package main

import (
	"fmt"
	"sync"
)

type Counter struct {
	count int64
	mu sync.Mutex
}

func (c *Counter)IncrementCounter() {
	c.mu.Lock()
	c.count++
	c.mu.Unlock()
}

func (c *Counter)GetCounterValue() int64 {
	c.mu.Lock()
	defer c.mu.Unlock()
	return c.count
}

func main() {
	var counter Counter
	var wg sync.WaitGroup

	wg.Wait(10)

	for i := 0; i < 10; i++ {
		go func(){
			defer wg.Done()
			for j := 0; j < 100000; j++ {
				counter.IncrementCounter()
			}
		}()
	}

	wg.Wait()
	fmt.Printf("Current Counter value of : %d\n", counter.GetCounterValue())
}
```


##### 小总结
Mutex是Golang并发同步原语中的一种，用于保护临界区资源，从而达到资源的一致性

---
that's all