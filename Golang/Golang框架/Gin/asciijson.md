##### 使用AsciiJSON

```go
package main

import (
    "github.com/gin-gonic/gin"
)

func main() {
    r := gin.Default()
    
    r.GET("/asciijson", func(c *gin.Context) {
        data := map[string]interface{}{
            "name": "gin框架",
            "tag": "<strong>Golang Framework.</strong>",
        }
        
        c.AsciiJSON(200, data)
    })
    
    r.Run(":9527")
}
```

##### 使用JSONP

```go
package main

import (
    "github.com/gin-gonic/gin"
)

func main() {
    r := gin.Default()
    
    r.GET("/jsonp", func(c *gin.Context) {
        data := map[string]interface{}{
            "name": "jsonp",
            "tag": "golang framework",
        }
        c.JSONP(200, data)
    })
    
    r.Run(":9527")
}
```

##### 使用SecureJSON

```go
package main

import (
    "github.com/gin-gonic/gin"
)

func main() {
    r := gin.Default()
    
    r.GET("/securejson", func(c *gin.Context) {
        data := map[string]interface{}{"name": "golang"}
        
        c.SecureJSON(200, data)
    })
    
    r.Run(":9527")
}
```



---

that's all