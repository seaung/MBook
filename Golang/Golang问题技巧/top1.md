##### defer的执行顺序
1. 源代码
```go
package main

import (
	"fmt"
)

func callDefer() {
	defer func(){ fmt.Println("1") }()
	defer func(){ fmt.Println("2") }()
	defer func(){ fmt.Println("3") }()
	panic("panic")
}

func main() {
	callDefer()
}
```

2. 运行结果
```bash
3
2
1
panic

goroutine 1 [running]:
main.deferCall()
	/home/gexample/0/main.go:21 +0x6b
main.main()
	/home/gexample/0/main.go:6 +0x17
exit status 2
```

3. 解释
defer是后进行先出,panic需要等待defer结束后才会向上传递.出现panic时，会按照defer先进后出的机制顺序执行，最后才执行panic


##### range的使用注意事项
1. 源代码
```go
import "fmt"

type Person struct {
	Name string
	Age  int
}

func printStruct() {
	p := make(map[string]*Person)
	ps := make(map[string]*Person)

        person := []Person{
		{Name: "jack", Age: 23},
		{Name: "tom", Age: 23},
		{Name: "johon", Age: 23},
        }

	for _, item := range person {
		p[item.Name] = &item
	}

	fmt.Println("then p value of ", p)

	for i := 0; i < len(person); i++ {
		p[person[i].Name] = &person[i]
	}

	fmt.Println("then ps value of ", ps)
}

func main() {
	printStruct()
}
```

2. 运行结果
```bash
then p value of  map[jack:0xc000010030 johon:0xc000010030 tom:0xc000010030]
the ps value of  map[jack:0xc00007a050 johon:0xc00007a080 tom:0xc00007a068]
```

3. 解释
range使用的是person的副本,所以上面代码中的p[item.Name] = &item实际上指向的都是同一个指针，最终该指针的值为遍历的最后一个struct的值拷贝


---
that's all
