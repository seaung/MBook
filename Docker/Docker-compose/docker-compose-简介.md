##### Docker-compose简介

Docker compose是docker官方编排项目之一，负责快速的部署分布式应用。compose的定位是定义和运行多个Docker容器的应用。



##### Docker-compose两个重要的概念

1. 服务：

   一个应用的容器，实际上可以包括若干个运行相同镜像的容器实例。

2. 项目：

   由一组关联的应用容器组成的一个完整的业务单元(例如：一个Django开发的web项目和mysql服务等)，在的docker-compose.yml文件中定义。





---

that's all