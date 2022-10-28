##### Cgo?

cgo就是让Go包中调用C代码。反过来C中也可以调用Go中的代码。

##### 从一个hello world例子来举例

下面代码是一个最简单的CGo程序，首先我们在Go的源文件中的注释里编写了C相关的代码，然后通过`import "C"`导入了C这个虚拟包表示开启了Cgo，注意这个导入语句必须紧跟这注释且独占一行，中间不能有空行。紧接着我们在`main`函数通过`C.hello()`调用了C的函数。

```go
package main

/*
#include <stdio.h>

void hello(void) {
	printf("hello world\n");
} 
*/
import "C"

func main() {
    C.hello()
}
```

##### C和Go类型对比

|       C数据类型        | CGo数据类型 | Go语言类型 |
| :--------------------: | :---------: | :--------: |
|          char          |   C.char    |    byte    |
|      signed char       |   C.schar   |    int8    |
|     unsigned char      |   C.uchar   |   uint8    |
|         short          |   C.short   |   int16    |
|     unsigned short     |  C.ushort   |   uint16   |
|          int           |    C.int    |   int32    |
|      unsigned int      |   C.uint    |   uint32   |
|          long          |   C.long    |   int32    |
|     unsigned long      |   C.ulong   |   uint32   |
|     long long int      | C.longlong  |   int64    |
| unsigned long long int | C.ulonglong |   uint64   |
|         float          |   C.float   |  float32   |
|         double         |  C.double   |  float64   |
|         size_t         |  C.size_t   |    uint    |



---

that's all