##### close函数
```c
int close(int sockfd)
```

close函数作用:
	对已连接的套接字执行`close`操作，若成功则返回0,失败则返回-1


close函数的注意事项:
	这个函数会对套接字引用技术减一，一旦发现套接字引用计数为零，就会对套接字进行彻底的释放，并且会关闭TCP两个方向的数据流


##### close函数具体是如何关闭两个方向的数据流的?
1. 在输入方向：系统内核将该套接字设置为不可读，任何读操作都会返回异常

2. 在输入方向：系统内核尝试将发送缓冲区的数据发送给对端，并最后向对端发送一个FIN报文，接下来如果再对该套接字进行写操作会返回异常


##### shutdown函数
```c
int shutdown(int sockfd, int howto)
```

shutdown函数作用：
	对已连接的套接字执行shutdown操作，若成功返回0,失败则返回-1


howto参数选项:
	1. `SHUT_RD`选项: 关闭连接的"读"这个方向,对该套接字进行读操作直接返回EOF,从数据角度上看，套接字上接收缓冲区已有的数据将被丢弃
	如果在有新的数据流到达，会对数据流进行ACK,然后悄悄丢弃，也就是说，对端还是会接收到ACK,这种情况下根本不知道数据已经被丢弃了。

	2. `SHUT_WR`选项: 关闭连接的"写"这个方向，这个就常被称为半关闭的连接。此时不管套接字引用计数的值是多少，都会直接关闭连接的写方向。套接字上发送缓冲区已有的数据将被立即发送出去，并发送一个FIN报文给对端应用程序，如果对套接字进行写操作会报错。
	3. `SHUT_RDWR`选项：相当于`SHUT_WR`和`SHUT_RD`操作各一次，关闭套接字的读和写两个方向


##### `SHUT_RDWR`和close函数的差别
1. close函数会关闭连接并释放所有连接对应用的资源，而shutdown并不会释放掉套接字和所有资源

2. close函数存在引用计数的概念，并不一定导致该套接字不可用，shutdown函数则不管引用计数，直接使得该套接字不可用，如果有个别的进程企图使用该套接字，将会受到影响。

3. close函数的引用计数导致不一定会发出FIN结束报文，而shutdown则总是会发出FIN结束报文，这在我们打算关闭连接通知对端的时候，是非常重要的。


---
that's all
