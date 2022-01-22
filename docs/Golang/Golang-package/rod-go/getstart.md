##### 安装rod

```bash
go get github.com/go-rod/rod
```


##### 打开一个窗口并截屏
1. New()创建一个浏览器实例
2. MustConnect()启动并连接到浏览器
3. MustPage()创建一个页面对象
4. MustWaitLoad()等待页面加载完毕
5. MustScreenshot()屏幕截屏


```go
package main

import (
	"github.com/go-rod/rod"
)

func main() {
	page := rod.New().MustConnect().MustPage("https://www.baidu.com")
	page.MustWaitLoad().MustScreenshot("baidu.png")
}
```


##### 输入和点击
1. MustElement()获取元素,会自动等待直到元素出现为止
2. MustInput()输入文本到输入框中
3. MustClick()点击操作


```go
package main

import (
	"time"
	"github.com/go-rod/rod"
)

func main() {
	page := rod.New().MustConnect().MustPage("https://www.baidu.com").MustWindowFullscreen()
	page.MustElement("#s_ipt").MustInput("hello world")
	page.MustElement("#bg #s_btn").MustClick()
	page.MustWaitLoad().MustScreenshot("baidu.png")

	time.Sleep(time.Hour)
}
```


##### 获取文本内容和图片内容
1. MustText()获取元素的文本内容
2. MustResource()获取图片的二进制数据
```go
package main

import (
	"fmt"
	"github.com/go-rod/rod"
	"github.com/go-rod/rod/lib/utils"
)

func main() {
	page := rod.New().MustConnect().MustPage("https://www.baidu.com").MustWindowFullscreen()
	page.MustElement("#s_ipt").MustInput("hello world png")
	page.MustElement("#bg #s_btn").MustClick()

	el := page.MustElement("c-abstract")
	fmt.Println(el.MustText())

	img := page.MustElement("#c-row#c-gap-top-small > div > a > img")
	_ := utils.OutputFile("baidu.png", img.MustResource())
}
```


##### .rod文件
.rod文件提供了一种操作浏览器的配置


---
that's all