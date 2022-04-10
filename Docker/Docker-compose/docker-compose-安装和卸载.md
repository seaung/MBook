##### 安装Docker-compose

docker-compose的安装非常的简单，官方为我们提供了一个自动化的安装脚本，或者我们也可以使用curl命令来进行安装。由于我使用的是Linux平台的系统所以就以Linux为例。打开你的终端，键入以下命令即可完成安装

```bash
# 从github上将docker-compose命令下载到本的/usr/local/bin目录下
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 由于没有执行权限所以还要赋予docker-compose可执行的权限，命令如下
sudo chmod +x /usr/local/bin/docker-compose
```

Note: 如果你使用docker-compose失败了，可能的原因有两个

1. 下载安装docker-compose过程中，网络环境不好，导致下载的docker-compose不完整，重新下载即可
2. 找不到docker-compose的执行路径，检查docker-compose的安装路径是否正确

创建一个软链接即可

```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

##### 升级的docker-compose

```bash
docker-compose migrate-to-labels
```

##### 卸载docker-compose

如果你使用的curl命令进行安装的请使用如下命令

```bash
sudo rm /usr/local/bin/docker-compose
```

如果你使用pip进行安装的请使用如下命令

```bash
pip uninstall docker-compose
```



##### 参考链接

[Docker-compose安装参考链接](https://docs.docker.com/compose/install/)





---

that's all