##### 安装Gin

```bash
go get -u github.com/gin-gonic/gin
```

##### 一个Gin Hello World程序

```go
package main

import (
    "github.com/gin-gonic/gin" // 1.导入gin
)

func main() {
    r := gin.Default() // 2.声明一个gin的默认实例
    
    r.GET("/hello", func(c *gin.Context) {
        c.JSON(200, gin.H{"msg": "hello world!"})
    }) // 3.使用gin实例提供的http方法，返回一个hello world字符串
    
    r.Run(":9527") // 4.开启监听
}
```



---

that's all