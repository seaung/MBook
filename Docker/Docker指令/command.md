#### Docker命令

docker命令由一系列的docker子命令组成，例如下面所示

```bash
docker subcommand [option [options]...]
```

#### 使用Docker搜索镜像命令

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

2. 启动，重启和启动退出一个容器

```bash
docker start [container id | container name] # 启动一个已经启动过的容器

docker restart [container id | container name]

docker stop [container id | container name]
```

3. 进入容器

```bash
docker attach [options]

docker attach container_name

docker exec -i -t [container id | container name] [command]
```

4. 导出和导入容器镜像

```bash
docker export [container id | container name] > export_name.[tar|zip...]

docker import [container id | container name | container url]
```

5. 删除和清理器器

```bash
docker container rm [container id | container name]
# -f 选项可删除正在运行中的容器

docker container prune # 清理所有处于终止状态的容器
```



---

that‘s all