#### Ubuntu18.04配置静态IP地址

1. 使用vim打开并编辑/etc/netpalan/00-installer-config.yaml文件

   ```bash
   sudo vim /etc/netpalan/00-installer-config.yaml # 需要root权限
   ```

2. 增加如下配置

   ```yaml
   network:
     enternets:
       ens33:
         dhcp4: false
         addresses: [192.168.10.222/24] # 这里可以填写多个ip地址，ip间用分号分割
         gateway4: 192.168.10.238
         nameservers:
           addresses: [8.8.8.8, 1.1.1.1]
      version: 2
   ```

3.  重启网络使配置生效

   ```bash
   sudo netplan apply
   ```

   

---

that's all