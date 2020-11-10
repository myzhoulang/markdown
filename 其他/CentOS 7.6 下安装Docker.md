<h1 align="center">CentOS 7.6 下安装Docker和启动配置</h1>

## 安装环境

* Linux 系统内核 3.10 (使用 `uname -r`查看)
* 
* curl 文件传输工具 （没有curl 可使用 `yum -y install curl`）

## 步骤

1. 使用`curl -sSL https://get.docker.com/ | sh`安装`docker`

2. 使用`docker -v`查看`docker`版本。

3. 设置国内镜像源。修改 /etc/docker/daemon.json。如果没有这个文件，就创建一个。

   ```bash
   # vi /etc/docker/daemon.json
   {
       "registry-mirrors": ["http://hub-mirror.c.163.com"]
   }
   ```

4. 使用`systemctl start docker`启动`docker`。

5. 使用 `systemctl enable docker`将`docker`加入开启启动。

## 安装docker-compose

1. 使用`sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`安装`docker-compose`。
2. 使用`sudo chmod +x /usr/local/bin/docker-compose`对二进制文件应用可执行权限。

## Docker学习

* [Docker 官网](https://www.docker.com/)
* [Docker 文档](https://docs.docker.com)
* [Docker 官方镜像库](https://hub.docker.com/)