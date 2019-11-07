# Docker
* [Docker 官网](https://www.docker.com/)
* [Docker 文档](https://docs.docker.com)
* [Docker 官方镜像库](https://hub.docker.com/)
## Docker Todo
- [ ] 使用`yum`安装`Docker`。
- [ ] 镜像上传
- [ ] 如何制作自己的镜像
- [x] 常规安装



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