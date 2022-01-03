#### Arch Linux上安装Wireshark会遇到的问题

1. 安装

   ```bash
   sudo pacman -S wireshark-qt
   ```

   

2. 问题

   在arch linux上安装Wireshark后使用普通用户去启动它时会看不到网卡信息?可以通过以下步骤得到解决

   ```bash
   1. sudo gpasswd -a [当前电脑用户] wireshark // 将当前用户加入到wireshark用户组
   2. sudo chgrp wireshark /usr/bin/dumpcap // 将dumpcap改成属于wireshark组
   3. sudo chmod 4755 /usr/bin/dumpcap // 修改dumpcap权限
   ```



---

that‘s all

