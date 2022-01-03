#### Docker命令

docker命令由一系列的docker子命令组成，例如下面所示

```bash
docker subcommand [option [options]...]
```

#### 使用Docker镜像命令

1. 搜索镜像

```bash
docker search [images:tag][options]

docker search ubuntu:18.04
```

2. 拉取镜像

```bash
docker pull [images:tag][options]

docker pull ubuntu:18.04
```

3. 列出镜像

```
docker imges [options]

docker imges ls
```

4. 删除镜像

```
docker rmi [options]

docker rmi ubuntu:18.04
```

#### 操作容器

1. 启动一个容器

```bash
docker run [options]

docker run -i -t ubuntu /bin/bash
```

2. 退出一个容器

```bash
docker stop [container name [container id]]

docker stop container_name
```

3. 进入容器

```bash
docker attach [options]

docker attach container_name
```





---

that‘s all