##### 实战中常常会用到的字符串函数

1. `strlen`函数

```c
/*
头文件  : #include<string.h>
函数原型: size_t strlen(const char *s);
函数作用: 返回字符串的长度
函数说明: 用来计算指定字符串s的长度,但不包括'\0'
返回值  : 返回字符串s的字符数
*/

#include <string.h>
#include <stdio.h>

int main()
{
	char *src = "hello world";

	printf("the src length : %d\n", strlen(src));
	return 0;
}
```

2. `bzero`函数

```c
/*
头文件   : #include<string.h>
函数原型 : void bzero(void *s, int n);
函数作用 : 将一段内存内容全清零
函数说明 : 将参数s所指的内存区域前n个字节，全部设置为零值
返回值   : 无
*/

#include <string.h>

int main()
{
	char *src;
	bzero(&src, sizeof(src));
	return 0;
}
```

3. `memset`函数

```c
/*
头文件   : #include<string.h>
函数原型 : void *memset(void *s, int c, size_t n);
函数作用 : 将一段内存空间填入某值
函数说明 : 将参数s所指的内存区域前n个字节，以参数c填入，然后返回指向s的指针
返回值   : 返回指向s的指针
Note     : 参数c虽然为int，但是必须是unsigned char,所以范围在0~255之间
*/

#include <string.h>

int main
{
	char arr[30];
	memset(arr, 'A', sizeof(arr));
	arr[30] = '\0' // 需要在字符串末尾补上`\0`结束字符串
	printf("the value of = %s\n", arr);
	return 0;
}
```

3. `memcpy`函数

```c
/*
头文件   : #include<string.h>
函数原型 : void *memcpy(void *dest, const void *src, size_t n);
函数作用 : 拷贝内存内容
函数说明 : 拷贝内src所指向的内存内容前n个字节到dest所指的地址上。与函数`stcpy`不同的是`memcpy`函数会完整的复制n个字节，不会因遇到`\0`就结束
返回值   : 返回指向dest的指针
Note     : 指针src和dest所指的内存区域不可重叠
*/

#include <string.h>
#include <stdio.h>

int main()
{
	char a[30] = "string {a}";
	char b[30] = "string\0string";
	int i;
	strcpy(a, b);
	printf("strcpy end\n");

	for (i = 0; i < 30; i++)
		printf("the char a %c\n", a[i]);

	memcpy(a, b, 30);
	printf("memcpy end\n");
	
	for (i = 0; i < 30; i++)
		printf("the char a %c", a[i]);

	return 0;
}
```

4. `strcat`函数

```c
/*

*/
```









---
that's all
