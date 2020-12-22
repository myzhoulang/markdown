# gitlab-runner

## 安装

### Linux 方式安装

> 在[官方指定的](https://gitlab-runner-downloads.s3.amazonaws.com/latest/index.html)或[清华源](https://mirrors.tuna.tsinghua.edu.cn/gitlab-runner/)找到对应的二进制文件或包进行下载

#### 下载二进制安装

1. 下载二进制文件到本地

```shell
# Linux x86-64
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64"

# Linux x86
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-386"

# Linux arm
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-arm"

# Linux arm64
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-arm64"

# Linux s390x
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-s390x" 
```

2. 授予执行权限

```shell
sudo chmod +x /usr/local/bin/gitlab-runner
```

3. 创建`gitlab`用户

```shell
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```

4. 安装并运行服务

```shell
# 以 gitlab-runner 用户安装 工作目录在 /home/gitlabe-runner
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start
```



#### 更新

	1. 停止`gitlab-runner`服务

```shell
sudo gitlab-runner stop
```

2. 下载二进制文件以替换`Runner`的可执行文件

```shell
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64"
```

3. 重新授予执行权限

```shell
sudo chmod +x /usr/local/bin/gitlab-runner
```

	4.  启动服务

```shell
sudo gitlab-runner start
```



### 使用包管理工具

#### 安装

1. 下载对应的包(以 `CentOS` 为列)

```shell
# 从官方下载
curl -LJO "https://gitlab-runner-downloads.s3.amazonaws.com/latest/rpm/gitlab-runner_amd64.rpm"

# 从清华源下载
curl -LJO "https://mirrors.tuna.tsinghua.edu.cn/gitlab-runner/yum/el7/gitlab-runner-10.0.0-1.x86_64.rpm"
```

2. 使用对应的包管理器

```shell
rpm -i gitlab-runner-10.0.0-1.x86_64.rpm
```

3. 启动服务

```shell
systemctl start gitlab-runner # 启动
systemctl status gitlab-runner # 查看状态
```



#### 更新

1. 和上面一样下载对应的包
2. 执行升级

```shell
rpm -Uvh gitlab-runner-10.0.0-1.x86_64.rpm
```

### Docker 安装

1. 首次在本地创建 3 个目录，做`docker`容器的的映射

```shell
cd /gitlab-runner
mkdir config data logs
```

2. 拉取镜像

```shell
docker pull gitlab-runner
```

3. 使用 `docker`容器启动

```shell
docker run -itd -v /gitlab-runner/config:/etc/gitlab-runner gitlab-runner
```

4. 查看运行的服务

```shell
# 首先使用 docker ps 知道对应的容器 id
docker ps

# 进入容器内部执行bash
docker exec -it <容器ID> bash

# 执行 gitlab-runner -h 查看帮助
gitlab-runner -h

```



## `gitlab-runner`命令

### 启动

```shell
gitlab-runner --debug <command> # debug 模式
gitlab-runner run # 普通用户模式 配置文件在 ~/.gitlab-runner/config.toml
sudo gitlab-runner run # 超级用户模式 配置文件在 /etc/gitlab-runner/config.toml
```

### 注册

```shell
gitlab-runner register # 注册runner 默认使用交互模式
gitlab-runner list # 列出保存在配置文件中的所有运行程序
gitlab-runner verify # 检查注册的runner 是否可以连接
gitlab-runner unregister # 取消已注册的runner
gitlab-runner unregister --all-runners # 取消所有
```

### 管理

```shell
gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
# --user指定将用于执行构建的用户
#`--working-directory  指定将使用**Shell** executor 运行构建时所有数据将存储在其中的根目录

gitlab-runner uninstall # 停止运行服务并从服务中卸载 gitlab runner
gitlab-runner start # 启动 gitlab runner 服务
gitlab-runner stop # 停止 gitlab runner 服务
gitlab-runner restart # 重启 gitlab runner 服务
gitlab-runner status # 显示 gitlab runner 服务状态
```



## `gitlab-runner` 注册

### 注册项目级的`runner`

1. 进入具体项目 -> setting -> CI/CD -> Runners -> Specific Runner 找到 `runner` 注册时需要的`URL`和`token`
2. 在 `gitlab-runner`服务的机子上注册

#### 普通安装的`gitlab-runner`注册

```shell
gitlab-runner register
# 执行后会有交互方式注册
# 把在gitlab中的信息对应输入即可
# 需要注意的是输入 tags 的时候需要记住
# 后期在 gitlab-ci.yml 文件中需要指定对应的 tag, 这样 gitlab-runner 才知道需要运行哪一个 runner
# 最后在选择执行器的时候，可以选 shell 或 docker, 其他执行器没有选过
```

 #### `docker` 安装的 `gitlab-runner` 注册

```shell
# 进去容器内部并启动 shell
docker exec -it <容器ID> /bin/bash
# 执行注册，注册流程和普通注册是一样的
gitlab-runner register


```

注册完成之后就可以在 `gitlab`中看到这个项目有对应激活的`runner`。

## `gitlab-ci.yml`语法








