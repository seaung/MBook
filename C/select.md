##### select函数
select函数是一种常见的I/O多路复用技术，使用select函数，可以通知内核挂起进程，当一个或多个I/O事件发生后，控制权返还给应用程序，由应用程序进行I/O事件的处理

##### select函数的使用方法
```c
#include <sys/select.h>

int select(int nfds, fd_set *restrict readfds, 
fd_set *restrict writefds, 
fd_set *restrict errorfds, 
struct timeval *restrict timeout);
```

1. 返回值： 若有就绪的文件描述符则为其数目，若超时则为0,若出错则为-1

2. 参数说明:
	nfds: 这个值代表待测是的文件描述符基数，它的值是待测试的最大文件描述符加1,比如现在的select待测试的描述符集合是
	`{0, 1, 4}`那么ndfs就是为5

	readfds: 读描述符
	writefds： 写描述符
	errorfds：异常描述符
	这三个描述符分别通知内核，在哪些描述符上检测数据，是可读，可写还是异常

	struct timeval {
		long tv_sec /*second*/
		long tv_vsec /*microseconds*/
	}

	这个参数设置不同的值，会有不同的可能
	1. 当设置成空(NULL)表示如果没有I/O发生，则select一直等待下去
	2. 当设置成一个非零的值,表示等待固定的一段时间后从select阻塞调用中返回
	3. 当`tv_sec`和`tv_vsec`都设置成0,表示根本不用等待，检测完立即返回


##### 常见的设置select文件描述符的宏函数
```c
void FD_ZERO(fd_set *set);
void FD_CLR(int fd, fd_set *set);
void FD_ISSET(int fd, fd_set *set);
void FD_SET(int fd, fd_set *set);
```

1. 宏函数解释:
	FD_ZERO: 将这个向量的所有元素都设置成0
	FD_CLR: 把对应套接字fd元素，a[fd]设置成0
	FD_ISSET： 对这个向量进行检测判断出对应套接字元素,a[fd]是0还是1
	FD_SET: 把对应套接字fd的元素，a[fd]设置成1

	其中0代表不需要处理，1代表需要处理


---
that's all
