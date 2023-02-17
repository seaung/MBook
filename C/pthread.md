##### 线程的创建

```c
#include <pthread.h> // 头文件

int pthread_create(pthread_t *restrict thread,
                          const pthread_attr_t *restrict attr,
                          void *(*start_routine)(void *),
                          void *restrict arg);

thread       : 线程id
attr         : 线程属性
start_routine: 线程函数
arg          : 线程函数参数
```

创建线程例子

```c
#include<pthread.h>
#include<stdlib.h>
#include<stdio.h>

void *thread(void * arg);

int main()
{
	pthread_t pthid;

	if (pthread_create(&pthid, NULL, thread, NULL) != 0)
	{
		printf("create thread faild!\n");
		exit(-1);
	}

	return 0;
}

void *thread(void * arg)
{
	printf("hello thread\n");
}
```

运行结果
可以看到没有任何输出，这是因为main函数这个主线程退出了，没给子线程thread提供时间执行
```bash
gcc -o demon main.c -lpthread
./demon
```

##### 等待线程的退出

```c
#include <pthread.h>

int pthread_join(pthread_t thread, void **retval);

thread : 线程id
retval : 线程返回值
```

使用例子

```c
#include<pthread.h>
#include<stdlib.h>
#include<stdio.h>

void *thread(void * arg);

int main()
{
	pthread_t pthid;

	if (pthread_create(&pthid, NULL, thread, NULL) != 0)
	{
		printf("create thread faild!\n");
		exit(-1);
	}

	pthread_join(pthid, NULL);

	return 0;
}

void *thread(void * arg)
{
	printf("hello thread\n");
}
```

运行结果

```bash
./demon 

hello thread
```

##### 向线程函数传递参数


##### 线程终止

##### 线程分离


---
that's all
