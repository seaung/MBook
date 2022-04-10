##### Docker的安装

1. Archlinux下安装docker

   ```bash
   sudo pacman -S docker // 使用pacman包管理工具安装docker
   
   sudo groupadd docker // 创建docker用户组
   
   sudo usermod -aG docker ${USER} // 添加docker到用户组中
   ```

2. Ubuntu下安装docker

   ```bash
   # 卸载旧版本的docker
   sudo apt-get remove docker \
                  docker-engine \
                  docker.io
   
   # 更新apt源并安装相对应的软件工具
   sudo apt-get update
   sudo apt-get install \
       apt-transport-https \
       ca-certificates \
       curl \
       gnupg \
       lsb-release
       
   # 添加软件源的GPG密钥
   curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   
   # 向sources.list文件添加docker软件源
   echo \
     "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \
     $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
     
   # 安装docker
   sudo apt-get update
   
   sudo apt-get install docker-ce docker-ce-cli containerd.io
   ```

##### 参考链接

[Docker安装参考链接](https://docs.docker.com/engine/install/)

[Ubuntu下的安装](https://docs.docker.com/engine/install/ubuntu/)



---

that's all

