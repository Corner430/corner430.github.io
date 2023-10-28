---
title: Docker笔记
date: 2023-04-13 00:04:40
tags:
declare: true
toc: 1
---
### 1. Docker 架构
- Docker 包括三个基本概念:
  - **镜像（Image）**：Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。
  - **容器（Container）**：镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。
  - **仓库（Repository）**：仓库可看成一个代码控制中心，用来保存镜像。<!--more-->

Docker 使用客户端-服务器 (C/S) 架构模式，使用远程API来管理和创建Docker容器。
Docker 容器通过 Docker 镜像来创建。
容器与镜像的关系类似于面向对象编程中的对象与类。

### 2. Docker 容器使用
```shell
docker                      #查看到 Docker 客户端的所有命令选项。
docker command --help       #更深入的了解指定的 Docker 命令使用方法。
docker pull ubuntu          #如果我们本地没有 ubuntu 镜像，我们可以使用 docker pull 命令来载入 ubuntu 镜像
docker run -it ubuntu /bin/bash     #使用 ubuntu 镜像启动一个容器，参数为以命令行模式进入该容器
docker ps -a                #查看所有的容器命令
docker start b750bbbcfd88   #启动一个已停止的容器
docker run -itd --name ubuntu-test ubuntu /bin/bash     #通过 -d 指定容器的运行模式为后台运行，加了 -d 参数默认不会进入容器，想要进入容器需要使用指令 docker exec
docker stop <容器 ID>       #停止一个容器
docker restart <容器 ID>    #重启容器
docker attach <容器 ID>     #进入容器，但是如果从这个容器退出，会导致容器的停止。
docker exec -it <容器 ID> /bin/bash     #进入容器，如果从这个容器退出，容器不会停止
docker export 1e560fca3906 > ubuntu.tar     #导出本地某个容器
cat docker/ubuntu.tar | docker import - test/ubuntu:v1      #从容器快照文件中再导入为镜像
docker import http://example.com/exampleimage.tgz example/imagerepo     #也可以通过指定 URL 或者某个目录来导入
docker rm -f 1e560fca3906   #删除容器，删除容器时，容器必须是停止状态。
docker rmi <镜像名>         #删除镜像
docker container prune      #清理掉所有处于终止状态的容器
-P                          #将容器内部使用的网络端口随机映射到我们使用的主机上。
-p                          #指定端口映射，左侧的为本地主机端口，右侧为容器内部端口
docker port <容器 ID> or <容器 name>    #查看指定 （ID 或者名字）容器的某个确定端口映射到宿主机的端口号
docker logs -f [ID或者名字]  #查看容器内部的标准输出。-f让 docker logs 像使用 tail -f 一样来输出容器内部的标准输出。
docker top [ID或者名字]      #查看容器内部运行的进程
docker inspect [ID或者名字]  #查看 Docker 的底层信息。它会返回一个 JSON 文件记录着 Docker 容器的配置和状态信息。
docker ps -l                #查询最后一次创建的容器
docker images               #列出本地主机上的镜像（REPOSITORY：表示镜像的仓库源；TAG：镜像的标签；IMAGE ID：镜像ID；CREATED：镜像创建时间；SIZE：镜像大小）
docker run -it <REPOSITORY:TAG> #指定特定的镜像进行运行，不指定标签的情况下默认使用ubuntu:latest 镜像。
docker search <名字>        #从 Docker Hub 网站来搜索镜像
```

-------------------------------------------

### 3. 镜像使用
当我们从 docker 镜像仓库中下载的镜像不能满足我们的需求时，我们可以通过以下两种方式对镜像进行更改。
- 从已经创建的容器中更新镜像，并且提交这个镜像
- 使用 Dockerfile 指令来创建一个新的镜像

#### 3.1 更新镜像
- 更新镜像之前，我们需要使用镜像来创建一个容器。
- 在运行的容器内使用 `apt-get update` 命令进行更新。
- 在完成操作之后，输入 exit 命令来退出这个容器。
- 此时 ID 为 e218edb10161 的容器，是按我们的需求更改的容器。我们可以通过命令 `docker commit` 来提交容器副本。
```shell
docker commit -m="has update" -a="corner" e218edb10161 corner/ubuntu:v2
sha256:70bf1840fd7c0d2d8ef0a42a817eb29f854c1af8f7c59fc03ac7bdee9545aff8
```
各个参数说明：
  - -m: 提交的描述信息
  - -a: 指定镜像作者
  - e218edb10161：容器 ID
  - corner/ubuntu:v2: 指定要创建的目标镜像名
可以使用 `docker images` 命令来查看我们的新镜像 `corner/ubuntu:v2`
`docker run -t -i corner/ubuntu:v2 /bin/bash  #根据镜像启动新容器`

#### 3.2 构建镜像
我们使用命令 `docker build` ， 从零开始来创建一个新的镜像。为此，我们需要创建一个 Dockerfile 文件，其中包含一组`指令`来告诉 Docker 如何构建我们的镜像。
```shell
corner@corner:~$ cat Dockerfile 
FROM    centos:6.7
MAINTAINER      Fisher "fisher@sudops.com"

RUN     /bin/echo 'root:123456' |chpasswd
RUN     useradd corner
RUN     /bin/echo 'corner:123456' |chpasswd
RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
EXPOSE  22
EXPOSE  80
CMD     /usr/sbin/sshd -D
```
- 每一个指令都会在镜像上创建一个新的层，每一个指令的前缀都必须是大写的。
- 第一条FROM，指定使用哪个镜像源
- RUN 指令告诉docker 在镜像内执行命令，安装了什么
- 然后，我们使用 Dockerfile 文件，通过 docker build 命令来构建一个镜像。

```shell
corner@corner:~$ docker build -t corner/centos:6.7 .
Sending build context to Docker daemon 17.92 kB
Step 1 : FROM centos:6.7
 ---&gt; d95b5ca17cc3
Step 2 : MAINTAINER Fisher "fisher@sudops.com"
 ---&gt; Using cache
 ---&gt; 0c92299c6f03
Step 3 : RUN /bin/echo 'root:123456' |chpasswd
 ---&gt; Using cache
 ---&gt; 0397ce2fbd0a
Step 4 : RUN useradd corner
......
```
- -t ：指定要创建的目标镜像名
- . ：Dockerfile 文件所在目录，可以指定Dockerfile 的绝对路径

#### 3.3 设置镜像标签
我们可以使用 docker tag 命令，为镜像添加一个新的标签。
`docker tag 860c279d2fec runoob/centos:dev`
docker tag 镜像ID，这里是 860c279d2fec ,用户名称、镜像源名(repository name)和新的标签名(tag)。
使用 docker images 命令可以看到，ID为860c279d2fec的镜像多一个标签。

### 4. Docker 容器连接
可以指定容器绑定的网络地址，比如绑定 127.0.0.1。还可以指定/udp，默认是/tcp
`docker run -d -p 127.0.0.1:5001:5000/udp training/webapp python app.py`

#### 4.1 Docker容器互联
端口映射并不是唯一把 docker 连接到另一个容器的方法。
docker 有一个连接系统允许将多个容器连接在一起，共享连接信息。
docker 连接会创建一个父子关系，其中父容器可以看到子容器的信息。



### 5. 问题集
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
```shell
sudo usermod -aG docker your-user   #Add the user to the docker group to avoid permission issues

---------------------
若上述方案失败，如下：

sudo groupadd docker     #添加docker用户组
sudo gpasswd -a $USER docker     #将登陆用户加入到docker用户组中
newgrp docker     #更新用户组
docker ps    #测试docker命令是否可以使用sudo正常使用

---------------------------------
- 

```
  


### References
[Docker 教程](https://www.corner.com/docker/docker-tutorial.html)
[Use the Docker command line](https://docs.docker.com/engine/reference/commandline/cli/)
[26 Most Common Docker Commands with Examples](https://geekflare.com/docker-commands/)