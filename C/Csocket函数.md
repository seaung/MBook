##### TCP Socket编程相关函数介绍

1. `socket`函数

函数原型

```c
#include <sys/socket.h>
int socket(int family, int type, int protocol);
```

函数作用

创建一个用于通信用的`socket`文件对象，成功则返回socket描述符，失败则返回-1

函数参数解释

family：指明协议族，例如，AF_INET, AF_INET6这些常量

type：协议的类型，例如，IPPROTO_TCP, IPPROTO_UDP, IPPROTO_SCTP

protocol： 某个协议类型常量值

2. `connect`函数

函数原型

```c
#include<sys/socket.h>
int connect(int fd, const struct sockaddr * servaddr, socklen_t addrlen);
```

函数作用

与tcp服务器建立连接，成功返回0, 失败返回-1

函数参数解释

fd：由`socket`函数返回的socket描述符

servaddr：指向socket地址结构的指针

addrlen：servaddr结构的大小

3. `bind`函数

函数原型

```c
#include<sys/socket.h>
int bind(int fd, const struct sockaddr * myaddr, socklen_t addrlen);
```

函数作用

绑定socket结构地址，成功返回0,失败返回-1

函数参数解释

fd：由`socket`函数返回的socket描述符

myaddr：指向socket地址结构的指针

addrlen：servaddr结构的大小

4. `listen`函数

函数原型

```c
#include<sys/socket.h>
int listen(int fd, int backlog);
```

函数作用

监听socket连接，成功返回0, 失败返回-1

函数参数解释

fd：由`socket`函数返回的socket描述符

backlog：对应socket排队的最大连接数

5. `accept`函数

函数原型

```c
#include<sys/socket.h>
int accept(int fd, struct sockaddr cliaddr, socklen_t *addrlen);
```

函数作用

用于从已完成连接队列列队头返回下一个已完成连接

函数参数解释

fd：由`socket`函数返回的socket描述符

cliaddr：客户端对应的socket地址结构

addrlen：对应客户端socket地址结构长度



---

that's all