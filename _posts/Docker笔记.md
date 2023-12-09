---
title: Docker笔记
date: 2023-04-13 00:04:40
tags:
declare: true
toc: 1
---
- **镜像相关命令：**
   - `docker images`: 列出本地所有镜像。
   - `docker search <image_name>`: 在 Docker Hub 上搜索镜像。
   - `docker pull <image_name>`: 下载镜像到本地。
   - `docker rmi <image_name>`: 删除本地的镜像。
   - `docker build -t <image_name> <path_to_dockerfile>`: 根据 Dockerfile 构建镜像。

- **容器相关命令：**
   - `docker ps`: 列出正在运行的容器。
   - `docker ps -a`: 列出所有容器，包括停止的。
   - `docker run <options> <image_name>`: 运行一个容器。
   - `docker exec -it <container_id_or_name> <command>`: 在运行中的容器中执行命令。
   - `docker stop <container_id_or_name>`: 停止运行中的容器。
   - `docker start <container_id_or_name>`: 启动已经停止的容器。
   - `docker restart <container_id_or_name>`: 重启容器。
   - `docker rm <container_id_or_name>`: 删除停止的容器。
   - `docker logs <container_id_or_name>`: 查看容器的日志。
   - `docker stats`: 查看容器的资源占用情况。
   - `docker stats <container_id_or_name>`: 查看指定容器的资源占用情况。

例如：`docker run -d -it --name kodbox -p 10080:80 -v /data/docker/kodbox:/var/www/html --restart=always tznb/kodbox:1.15`
   - `-d`: 这个选项表示在后台（detached mode）运行容器，即容器将在后台运行，而不会占用当前终端。
   - `-it`: 这两个选项结合在一起，表示为容器分配一个交互式的终端（pseudo-TTY）。即使在后台运行，也可以通过 `docker exec` 进入容器的终端。
   - `--name kodbox`: 用于指定容器的名称为 "kodbox"。
   - `-p 10080:80`: 这个选项指定将容器的 80 端口映射到主机的 10080 端口。这样，通过访问 `http://localhost:10080`，你就可以访问到容器内部的 Web 服务。
   - `-v /data/docker/kodbox:/var/www/html`: 这个选项用于将主机上的 `/data/docker/kodbox` 目录挂载到容器内的 `/var/www/html` 目录。这个挂载操作可以用来持久化存储容器内的数据。
   - `--restart=always`: 这个选项指定容器在退出时总是自动重新启动，即使手动停止容器，Docker 也会尝试重新启动它。
   - `tznb/kodbox:1.15`: 这是要运行的 Docker 镜像的名称及标签。在这里使用的是 `tznb/kodbox` 镜像的版本标签为 `1.15`。

总体而言，这个命令的目的是在 Docker 中以后台方式启动一个名为 "kodbox" 的容器，将容器的 80 端口映射到主机的 10080 端口，同时将主机上的 `/data/docker/kodbox` 目录挂载到容器内的 `/var/www/html` 目录，确保容器在退出时会自动重新启动。


- **容器网络与端口相关命令：**
   - `docker network ls`: 列出 Docker 网络。
   - `docker port <container_id_or_name>`: 显示容器的端口映射。
   - `docker inspect <container_id_or_name>`: 查看容器的详细信息，包括网络配置。

- **Docker Compose 相关命令：**
   - `docker-compose up`: 启动容器组。
   - `docker-compose down`: 停止并删除容器组。
   - `docker-compose ps`: 列出容器组中的容器状态。
   - `docker-compose logs`: 查看容器组中所有容器的日志。

- **Docker 导入导出相关命令：**
   - `docker save <image_name> > <image_name>.tar`: 将镜像导出到文件。
   - `docker load < <image_name>.tar`: 从文件导入镜像。
   - `docker export <container_id_or_name> > <container_id_or_name>.tar`: 将容器导出到文件。
   - `docker import <container_id_or_name>.tar`: 从文件导入容器。

- **Dockerfile 的构建**

构建一个简单的 Node.js 应用的 Docker 镜像时，以下是一个完整的 Dockerfile 示例，每个步骤都有注释：

```dockerfile
# 使用官方 Node.js 镜像作为基础镜像
FROM node:14

# 设置镜像作者信息
LABEL maintainer="your_name@example.com"

# 创建并设置工作目录
WORKDIR /app

# 将当前目录下的 package.json 和 package-lock.json 复制到工作目录
COPY package*.json ./

# 安装应用程序的依赖
RUN npm install

# 将当前目录下的所有文件复制到工作目录
COPY . .

# 暴露应用程序运行的端口
EXPOSE 3000

# 设置环境变量
ENV NODE_ENV=production

# 定义构建参数
ARG user

# 输出构建参数值
RUN echo "Build argument 'user': $user"

# 容器启动时执行的命令
CMD ["npm", "start"]
```

在构建过程中，Dockerfile 会：

1. 使用 Node.js 14 作为基础镜像。
2. 设置镜像作者信息。
3. 在 `/app` 目录下创建工作目录，并将当前目录下的 `package.json` 和 `package-lock.json` 复制到工作目录。
4. 运行 `npm install` 安装应用程序的依赖。
5. 将当前目录下的所有文件复制到工作目录。
6. 暴露容器运行的端口为 3000。
7. 设置环境变量 `NODE_ENV` 为 `production`。
8. 定义构建参数 `user`，并在构建时输出其值。
9. 在容器启动时执行 `npm start`。

可以使用以下命令构建和运行这个 Docker 镜像：

```bash
docker build -t mynodeapp:latest --build-arg user=myuser .
docker run -p 8080:3000 mynodeapp:latest
```

----------------------------------------------------

- **Docker Compose 的常用命令**
   - `docker-compose up`: 构建并启动容器组。
   - `docker-compose down`: 停止并移除容器组。
   - `docker-compose ps`: 显示容器组中容器的状态。
   - `docker-compose logs`: 查看容器组的日志输出。
   - `docker-compose exec`: 在容器中执行命令。
   - 例如：`docker-compose exec [service_name] [command]`
   - `docker-compose build`: 仅构建服务，不启动容器。
   - `docker-compose pull`: 从仓库拉取服务的镜像，但不启动容器。
   - `docker-compose restart`: 重启容器组。
   - `docker-compose stop`: 停止容器组，但不移除容器。
   - `docker-compose start`: 启动已经停止的容器组。
   - `docker-compose pause`: 暂停容器组。
   - `docker-compose unpause`: 恢复暂停的容器组。
   - `docker-compose rm`: 移除已经停止的容器组。
   - `docker-compose kill`: 强制停止容器组。
   - `docker-compose images`: 显示容器组的镜像列表。
   - `docker-compose config`: 验证 docker-compose.yml 文件配置。

---------------------------------------------

- **Docker Compose 的使用**
Docker Compose 是一个用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，可以使用 YAML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YAML 文件配置中创建并启动所有服务。

Docker Compose 的使用步骤如下：

1. 使用 Dockerfile 定义应用程序的环境。
2. 使用 docker-compose.yml 定义构成应用程序的服务，这样它们可以在隔离环境中一起运行。
3. 最后，执行 docker-compose up 命令来启动并运行整个应用程序。

**使用 Dockerfile 定义应用程序的环境**

Dockerfile 是用于定义 Docker 镜像的文件。在 Dockerfile 中，可以指定镜像的依赖项、环境变量和其他配置。

例如，以下 Dockerfile 定义了一个简单的 Node.js 应用程序的镜像：

```
FROM node:16-alpine

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

**使用 docker-compose.yml 定义服务**

docker-compose.yml 文件是用于定义 Docker Compose 应用程序的文件。在 docker-compose.yml 文件中，可以指定应用程序中的每个服务的名称、镜像、端口映射、网络连接等。

以下 docker-compose.yml 文件定义了一个由两个服务组成的应用程序：

```
version: '3'

services:
  web:
    image: node:16-alpine
    container_name: web
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3000:3000"

  redis:
    image: redis
    container_name: redis
    ports:
      - "6379:6379"
```

在该文件中，`web` 服务是基于 `node:16-alpine` 镜像构建的。它映射了主机端口 3000 到容器端口 3000。`redis` 服务是基于 `redis` 镜像构建的。它映射了主机端口 6379 到容器端口 6379。

**使用 docker-compose up 命令启动应用程序**

要启动 Docker Compose 应用程序，请使用 docker-compose up 命令。该命令将从 docker-compose.yml 文件中创建和启动所有服务。

例如，要启动上述应用程序，请执行以下命令：

```
docker-compose up
```

该命令将启动两个容器：一个运行 Node.js 应用程序，另一个运行 Redis 数据库。

------------------------------------------------------

- 问题集
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