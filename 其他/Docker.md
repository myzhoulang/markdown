# Docker
* [Docker 官网](https://www.docker.com/)
* [Docker 文档](https://docs.docker.com)
* [Docker 官方镜像库](https://hub.docker.com/)
## Docker Todo
- [ ] 使用`yum`安装`Docker`。
- [ ] 镜像上传
- [ ] 如何制作自己的镜像
- [x] 常规安装



## 什么是`Docker`

> `Docker`是一个开发、发布、运行应用程序的发布平台。它有三大核心：**镜像**、**容器**、**仓库**。 
>
> * 镜像是有文件和文件夹组成，包含了启动容器所需要的系统文件、应用程序的代码文件、程序运行的配置文件等，镜像是`Docker`运行的先决条件，容器都是基于镜像运行的。
> * 容器是镜像的运行实体。运行着真正的应用程序，容器本质上就是主机上运行的一个进程，容器具有自己的独立命名空间隔离和资源限制，容器内部是无法访问主机或其他容器的任何信息或资源。
> * 仓库是使用来存放和发布镜像的。仓库可分为共有仓库和私有仓库。

## 镜像基本命令

* 拉取镜像

  > `docker pull`是从远程仓库拉取镜像到本地
  >
  > `docker pull nginx:latest`：从远程镜像仓库中拉取`latest`版本的`nginx`镜像

* 查看本地镜像

  > `docker images`：列出本地所有镜像
  >
  > `docker image ls nginx` 或者`docker images | grep nginx`：查看本地是否有`nginx`镜像

* 删除本地镜像

  > `docker image rm`： 从本地删除指定镜像
  >
  > `docker image rm nginx:latest`：删除本地`latest`版本的`nginx`镜像

* 构建镜像

  > 镜像的构建有两种方式
  >
  > 1. 基于已运行的容器提交为镜像
  >
  >    ```shell
  >    $ docker commit busybox busybox:hello
  >    ```
  >
  > 2. 基于`DockerFile`文件构建
  >
  >    ```shell
  >    $ docker build -t mybusybox .
  >    ```
  >
  >    

### Centos 安装 Docker 下常规安装
1. 使用 `uname -r` 查看当前`Liunx`系统内核版本。*需要3.10及以上*。
2. 使用`docker container logs [容器名]`查看对应的容器日志。
3. 使用 `which curl`命令查看是否安装`curl`命令。如果没有安装 使用 `yum -y install curl`安装。
4. 使用`curl -sSL https://get.docker.com/ | sh`安装`docker`。
5. 使用`docker -v`查看`docker`版本。
6. 使用`systemctl start docker`启动`docker`。
7. 使用 `systemctl enable docker`将`docker`加入开启启动。
8. 使用`ps -aux | grep docker`查看`docker`运行状态。
9. 使用`docker ps -a`查看`docker`下的容器运行状态。
10. 使用`docker images`查看本地的镜像。
11. 使用`docker pull [镜像名]`下载镜像到本地,使用 `docker pull [镜像名]:[tag]`下载指定tag的镜像，删除也是一样。
12. 使用`docker rmi [镜像名 | 镜像ID]`删除本地主机的镜像。
13. 使用`docker inspect [镜像名]:[tag]?`查看镜像信息.
14. 使用`docker ps -a`查看当前主机的容器
15. 容器创建
```shell
	# docker run 启动容器
	# docker run -d 以守护进程方式启动
	# docker run -d -p [主机(宿主)端口]:[容器端口] 进行容器与宿主机的端口映射 
	# docker run -d --name [name] 命名容器名称
	# docker run -d -v [主机目录]:[容器目录] 进行主机与容器的目录映射
	# docker run -d --restart=always 容器退出重启容器 容器重启策略还有其他方式
	# docker run -d nginx:latest	使用 nginx 最新版本启动容器

	# 启动容器
  docker run -d -p 5000:5000 --name nginx -v /data:/data --restart=always nginx:latest
	# 以交互模式启动一个容器,在容器内执行/bin/bash命令。
	docker run -it nginx:latest /bin/bash

```

 容器常用命令：
	* run/create       创建容器
	* start                  启动容器
	* stop/kill           停止容器
	* restart              重启容器
	* pause              暂停容器
	* unpause         恢复容器
	* logs                 查看容器日志
	* stats                查看容器
	* top
	* port                 查看容器端口映射
	* exec                执行容器内部操作
	* diff
	* inspect            查看容器/镜像信息
	* update            更新容器配置
	* cp                     宿主机和容器之前的文件拷贝
	* rm                     删除容器
	* save
	* load
	* search
	* push
	* tag
	
	```
	SENTRY_IMAGE=getsentry/sentry: ./install.sh
	```

```shell
$ docker ps -aq						# 查看所有容器
$ docker stop $(docker ps -aq)		# 停所有容器
$ docker rm $(docker ps -aq)			# 删除所有容器
```

### 安装 docker-compose
```shell
# 第一步安装docker-compose
1. sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 对二进制文件应用可执行权限：
2. sudo chmod +x /usr/local/bin/docker-compose
```

#Docker



curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose