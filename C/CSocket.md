##### IPV4 Socket结构地址

```c
#include <netinet/in.h>

struct in_addr {
    in_addr_t s_addr;
};

struct sockaddr_in {
    uint8_t 		sin_len;
    sa_family_t 	sin_family;
    in_port_t 		sin_port;
    struct in_addr 	sin_addr;
    char 			sin_zero[8];
}
```

结构成员介绍

sin_len: 这个字段是为了增加对OSI协议的支持而随4.3BSD-Reno添加的

sin_family: 一个无符号短整型，socket家族

sin_port: 一个无符号短整型，端口好

sin_addr: 目标ip地址

sin_zero: 预留字段

##### 通用socket结构

```c
#include <sys/socket.h>

struct sockaddr {
    uint8_t 		sa_len;
    sa_family_t 	sa_family;
    char 			sa_data[14];
};
```

##### 字节排序函数

大端：将高序字节存储在起始地址的称之为大端

小端：一种将低字节序存储在起始地址的称之为小端

```c
#include <netinet/in.h>
uint16_t htons(uint16_t host16bitvalue);
uint32_t htonl(uint32_t host32bitvalue);

uint16_t ntohs(uint16_t net16bitvalue);
uint32_t ntohl(uint32_t net32bitvalue);
```





---

that's all