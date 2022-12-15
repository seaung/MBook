##### 什么是Context

context中文译为上下文的意思，其作用用于不同goroutine间传递元数据，查看和控制gorutine的生命周期

##### Context四个重要方法

1. `WithTimeout`和`WithDealine` 

   用于超时控制和设置过期时间

2. `WithCancel`

​      用于取消操作

3. `WithValue`

   用于传递元信息





---

that's all