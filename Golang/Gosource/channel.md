##### channel
channel被设计成用于实现goroutine间进行通信的数据结构

##### channel常见的使用场景
1. 停止信号

2. 定时任务

3. 任务分发

4. 并发控制

5. 解耦生产方和消费方

##### channel底层数据结构
```go
// https://github1s.com/golang/go/blob/master/src/runtime/chan.go#L33-L34

type hchan struct {
	qcount   uint           // 循环数组中channel元素的总数，即元素的数量
	dataqsiz uint           // 循环数组大小，即底层循环数组的大小(长度)
	buf      unsafe.Pointer // 指向底层循环数组的指针，只针对有缓冲区的channel
	elemsize uint16 // channel中元素的大小
	closed   uint32 // channel是否被标记关闭的标志
	elemtype *_type // 元素的类型
	sendx    uint   // 记录已发送在循环数组中的索引
	recvx    uint   // 记录已接收在循环数组中的索引
	recvq    waitq  // 等待接收的goroutine的队列
	sendq    waitq  // 等待发送的goroutine的队列

	// lock protects all fields in hchan, as well as several
	// fields in sudogs blocked on this channel.
	//
	// Do not change another G's status while holding this lock
	// (in particular, do not ready a G), as this can deadlock
	// with stack shrinking.
	// lock用于保护hchan结构的所有字段，以及sudog上的一些字段
	lock mutex
}

// sudog的一个双向链表，sudog实际上就是对goroutine的一个封装
type waitq struct {
	first *sudog
	last  *sudog
}
```



---
that's all
