##### Nmap简介

Nmap是由Gordon Lyon设计并实现的，于1997年开始发布。Nmap最初设计的目的是只希望打造一款强大的端口扫描器。但是，随着时间的推移，Nmap的功能越來越丰富。现在Nmap具备以下功能。

1. 主机发现功能：向目标主机发送特制的数据包组合，然后根据目标的反应来确定它是否处于开机并连接到网络的状态。
2. 端口扫描：向目标主机发送特制的数据包组合，然后根据目标的反应来判断它是否开放。
3. 服务及版本检测：向目标主机的目标端口发送特制的数据包组合，然后根据目标的反应来检测它运行服务的类型和版本。
4. 操作系统检测：向目标主机发送特制的数据包组合，然后根据目标的反应来检测它的操作系统类型和版本。

##### Nmap安装

Nmap支持的系统平台是比较全面的，这包括你能在Mac osx,Linux或windows下进行Nmap的安装。Nmap不仅能在黑乎乎的终端使用，还提供了图形化的界面来操作Nmap(zeNmap)

Nmap的安装比较简单，下载对应平台的安装包进行安装即可，这里就不再累述。它的下载地址为: https://Nmap.org/download.html

##### Nmap使用语法格式

```bash
nmap [options] <IP ADDRESSES>
```

##### Nmap常用用法

1. 扫描连续范围内的主机

```bash
# nmap <IP 地址范围>
nmap 192.168.1.1-255
```

2. 扫描整个子网

```bash
# nmap <IP/掩码位数>
nmap 192.168.1.203/24
```

3. 扫描多个不连续的主机

```bash
# nmap <IP> <IP2> <IP3> ...
nmap 192.168.2.111 192.168.7.33
```

4. 排除指定目标

```bash
# nmap <IP> --exclude [target]
nmap 192.168.1.0/24 --exclude 192.168.1.33
```

5. 对一个文本文件中的ip地址列表进行扫描

```bash
# nmap -iL filename
nmap -iL iplist.txt
```

6. 随机确定扫描目标

```bash
# nmap -iR [target number]
nmap -iR 100
```





---

that's all