##### tshark工具使用
1. 查看tshark版本信息

```bash
TShark (Wireshark) 3.6.2 (Git v3.6.2 packaged as 3.6.2-2)

Copyright 1998-2022 Gerald Combs <gerald@wireshark.org> and contributors.
License GPLv2+: GNU GPL version 2 or later <https://www.gnu.org/licenses/gpl-2.0.html>
This is free software; see the source for copying conditions. There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

Compiled (64-bit) using GCC 11.2.0, with libpcap, with POSIX capabilities
(Linux), with libnl 3, with GLib 2.71.2, with zlib 1.2.11, with Lua 5.2.4, with
GnuTLS 3.7.3 and PKCS #11 support, with Gcrypt 1.9.4, with MIT Kerberos, with
MaxMind DB resolver, with nghttp2 1.43.0, with brotli, with LZ4, with Zstandard,
with Snappy, with libxml2 2.9.12, with libsmi 0.4.8.

Running on Linux 5.15.0-57-generic, with Intel(R) Core(TM) i5-4300U CPU @
1.90GHz (with SSE4.2), with 7828 MB of physical memory, with GLib 2.72.4, with
zlib 1.2.11, with libpcap 1.10.1 (with TPACKET_V3), with c-ares 1.18.1, with
GnuTLS 3.7.3, with Gcrypt 1.9.4, with nghttp2 1.43.0, with brotli 1.0.9, with
LZ4 1.9.3, with Zstandard 1.4.8, with libsmi 0.4.8, with LC_TYPE=en_US.UTF-8,
binary plugins supported (0 loaded).
```

2. 列出当前系统存在的网络接口

```bash
1. wlp3s0
2. any
3. lo (Loopback)
4. enp0s25
5. bluetooth0
6. bluetooth-monitor
7. nflog
8. nfqueue
9. dbus-system
10. dbus-session
11. ciscodump (Cisco remote capture)
12. dpauxmon (DisplayPort AUX channel monitor capture)
13. randpkt (Random packet generator)
14. sdjournal (systemd Journal Export)
15. sshdump (SSH remote capture)
16. udpdump (UDP Listener remote capture)
```

3. 捕获指定网卡流量

```bash
sudo tshark -i wlp3s0
Running as user "root" and group "root". This could be dangerous.
Capturing on 'wlp3s0'
 ** (tshark:41186) 20:30:35.509817 [Main MESSAGE] -- Capture started.
 ** (tshark:41186) 20:30:35.509918 [Main MESSAGE] -- File: "/tmp/wireshark_wlp3s0N4IZZ1.pcapng"
    1 0.000000000 192.168.2.124 → 192.168.2.207 SSH 286 Server: Encrypted packet (len=220)
    2 0.001405394 192.168.2.207 → 192.168.2.124 TCP 66 44676 → 22 [ACK] Seq=1 Ack=221 Win=500 Len=0 TSval=3786928960 TSecr=1750560903
    3 0.692810006 e0:ef:02:1c:03:84 → Broadcast    ARP 60 Gratuitous ARP for 192.168.2.1 (Request)
    4 0.736488024 192.168.2.124 → 192.168.2.207 SSH 334 Server: Encrypted packet (len=268)

# -i 后面是需要指定捕获的网卡名称
```

4. 捕获指定网卡流量并保存为pcap格式的文件

```bash
tshark -i wlp3s0 -w w3a.pcap

tshark -i wlp3s0 -w w3a.pcap
Capturing on 'wlp3s0'
 ** (tshark:44072) 20:44:32.361556 [Main MESSAGE] -- Capture started.
 ** (tshark:44072) 20:44:32.361665 [Main MESSAGE] -- File: "w3a.pcap"
```

5. 读取pcap数据包文件

```bash
tshark -r w3a.pcap 
1 0.000000000 192.168.2.124 → 192.168.2.207 SSH 182 Server: Encrypted packet (len=116)
2 0.000061743 192.168.2.124 → 192.168.2.207 SSH 226 Server: Encrypted packet (len=160)
3 0.002084167 192.168.2.207 → 192.168.2.124 TCP 66 44680 → 22 [ACK] Seq=1 Ack=117 Win=501 Len=0 TSval=3787765799 TSecr=1751397755
4 0.002084418 192.168.2.207 → 192.168.2.124 TCP 66 44680 → 22 [ACK] Seq=1 Ack=277 Win=500 Len=0 TSval=3787765799 TSecr=1751397755
5 0.748663076 192.168.2.124 → 192.168.2.207 SSH 110 Server: Encrypted packet (len=44)
```


Note: 使用root权限启动会报出如下错误信息，我们可以根据提示消息进行解决
```bash
Please check to make sure you have sufficient permissions.

On Debian and Debian derivatives such as Ubuntu, if you have installed Wireshark from a package, try running

    sudo dpkg-reconfigure wireshark-common

selecting "<Yes>" in response to the question

    Should non-superusers be able to capture packets?

adding yourself to the "wireshark" group by running

    sudo usermod -a -G wireshark {your username}

and then logging out and logging back in again.

```

解决方案:
```bash
sudo dpkg-reconfigure wireshark-common # 运行此命令是会弹出一个对话的界面，我们选择Yes即可
sudo usermod -a -G wireshark $USER  # 将wireshark添加到当前用户组中,操作完成后需要注销登录或者重启系统
```


Note: 使用sudo启动tshark时会报出如下类似没有权限之类的错误
```bash
sudo tshark -i wlp3s0 -w w3a.pcap
Running as user "root" and group "root". This could be dangerous.
Capturing on 'wlp3s0'
tshark: The file to which the capture would be saved ("w3a.pcap") could not be opened: Permission denied.
```

解决方案:
```bash
sudo setcap 'CAP_NET_RAW+eip CAP_NET_ADMIN+eip' /usr/bin/dumpcap #为dumpcap设置网络权限
sudo usermod -aG wireshark $USER
```


---
that's all
