##### recfrom函数
```c
#include <sys/socket.h>

ssize_t recfrom(int sockfd, void *buffer, size_t nbytes, int flags, struct sockaddr *from, socklen_t *addrlen);
```

参数说明:
	1. sockfd: 本地创建的套接字描述符
	2. buffer: 指向本地的缓存
	3. nbytes: 表示最大接收数据字
	4. flags: 和I/O相关的参数
	5. from和addrlen返回对端发方的地址和端口信息

函数返回值:
	返回值是实际接收的字节数



##### sendto函数
```c
#include <sys/socket.h>

ssize_t sendto(int sockfd, void *buffer, size_t nbytes, int flags, struct sockaddr *to, socklen_t *addrlen);
```


参数说明:
	1. sockfd: 本地创建的套接字描述符
	2. buffer: 指向本地的缓存
	3. nbytes: 表示最大接收数据字
	4. flags: 和I/O相关的参数
	5. from和addrlen返回对端发方的地址和端口信息

函数返回值:
	返回值是实际发送的字节数



---
that's all
