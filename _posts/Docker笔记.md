---
title: Docker笔记
date: 2023-04-13 00:04:40
tags:
declare: true
toc: 1
---
1. **镜像相关命令：**
   - `docker images`: 列出本地所有镜像。
   - `docker search <image_name>`: 在 Docker Hub 上搜索镜像。
   - `docker pull <image_name>`: 下载镜像到本地。
   - `docker rmi <image_name>`: 删除本地的镜像。
   - `docker build -t <image_name> <path_to_dockerfile>`: 根据 Dockerfile 构建镜像。

2. **容器相关命令：**
   - `docker ps`: 列出正在运行的容器。
   - `docker ps -a`: 列出所有容器，包括停止的。
   - `docker run <options> <image_name>`: 运行一个容器。
   - `docker exec -it <container_id_or_name> <command>`: 在运行中的容器中执行命令。
   - `docker stop <container_id_or_name>`: 停止运行中的容器。
   - `docker start <container_id_or_name>`: 启动已经停止的容器。
   - `docker restart <container_id_or_name>`: 重启容器。
   - `docker rm <container_id_or_name>`: 删除停止的容器。
   - `docker logs <container_id_or_name>`: 查看容器的日志。

例如：`docker run -d -it --name kodbox -p 10080:80 -v /data/docker/kodbox:/var/www/html --restart=always tznb/kodbox:1.15`
   - `-d`: 这个选项表示在后台（detached mode）运行容器，即容器将在后台运行，而不会占用当前终端。
   - `-it`: 这两个选项结合在一起，表示为容器分配一个交互式的终端（pseudo-TTY）。即使在后台运行，也可以通过 `docker exec` 进入容器的终端。
   - `--name kodbox`: 用于指定容器的名称为 "kodbox"。
   - `-p 10080:80`: 这个选项指定将容器的 80 端口映射到主机的 10080 端口。这样，通过访问 `http://localhost:10080`，你就可以访问到容器内部的 Web 服务。
   - `-v /data/docker/kodbox:/var/www/html`: 这个选项用于将主机上的 `/data/docker/kodbox` 目录挂载到容器内的 `/var/www/html` 目录。这个挂载操作可以用来持久化存储容器内的数据。
   - `--restart=always`: 这个选项指定容器在退出时总是自动重新启动，即使手动停止容器，Docker 也会尝试重新启动它。
   - `tznb/kodbox:1.15`: 这是要运行的 Docker 镜像的名称及标签。在这里使用的是 `tznb/kodbox` 镜像的版本标签为 `1.15`。

总体而言，这个命令的目的是在 Docker 中以后台方式启动一个名为 "kodbox" 的容器，将容器的 80 端口映射到主机的 10080 端口，同时将主机上的 `/data/docker/kodbox` 目录挂载到容器内的 `/var/www/html` 目录，确保容器在退出时会自动重新启动。


3. **容器网络与端口相关命令：**
   - `docker network ls`: 列出 Docker 网络。
   - `docker port <container_id_or_name>`: 显示容器的端口映射。
   - `docker inspect <container_id_or_name>`: 查看容器的详细信息，包括网络配置。

4. **Docker Compose 相关命令：**
   - `docker-compose up`: 启动容器组。
   - `docker-compose down`: 停止并删除容器组。
   - `docker-compose ps`: 列出容器组中的容器状态。
   - `docker-compose logs`: 查看容器组中所有容器的日志。

5. 问题集
- 安装完 docker 后，执行docker相关命令，出现：

```bash
"Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.26/images/json: dial unix /var/run/docker.sock: connect: permission denied"
```

原因如下：

> Manage Docker as a non-root user

> The docker daemon binds to a Unix socket instead of a TCP port. By default that Unix socket is owned by the user root and other users can only access it using sudo. The docker daemon always runs as the root user.

> If you don’t want to use sudo when you use the docker command, create a Unix group called docker and add users to it. When the docker daemon starts, it makes the ownership of the Unix socket read/writable by the docker group

大概的意思就是：docker进程使用Unix Socket而不是TCP端口。而默认情况下，Unix socket属于root用户，需要root权限才能访问。
  - 方案一：使用sudo获取管理员权限，运行docker命令
  - 方案二
  docker守护进程启动的时候，会默认赋予名字为docker的用户组读写Unix socket的权限，因此只要创建docker用户组，并将当前用户加入到docker用户组中，那么当前用户就有权限访问Unix socket了，进而也就可以执行docker相关命令

`sudo usermod -aG docker your-user   #Add the user to the docker group to avoid permission issues`

---------------------
若上述方案失败，如下：

```bash
sudo groupadd docker     #添加docker用户组
sudo gpasswd -a $USER docker     #将登陆用户加入到docker用户组中
newgrp docker     #更新用户组
docker ps    #测试docker命令是否可以使用sudo正常使用
```