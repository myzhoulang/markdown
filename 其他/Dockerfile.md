<h1 align="center">Dockerfile</h1>

> Dockerfile 由一行行命令语句组成，注释以 `#`开头。Dockerfile 分为4部分：基础镜像，维护者信息，镜像操作指令，容器启动时执行的指令。每执行一个指令，都会创建一个镜像层并对镜像进行提交。导致构建的镜像臃肿。

## 指令

* FROM

> `FROM` 是需要基于基础的镜像。`FROM`是必须的并且必须是第一条指令。推荐使用官方的镜像。
>
> 格式： `FROM <image>`或者`FROM <image>:<tag>`。没有`tag`默认是` latest`。
>
> 案例：`FROM ubuntu:18.04 `

* RUN

> `RUN`是用来执行命令行命令的。`RUN`指令有两种格式。由于每一次执行都会添加一个层，为了减少层，可以使用一个 `RUN`	执行多个命令。多个命令之间使用`&&`分割。为了可读性，可以将每一个命令换行，每一行后面加上换行符（\）即可。有时候构建需要一些额外的资源，这些资源并不会被删除，会一直跟着镜像。导致镜像很大。为减少镜像大小，可在最后删除掉这些无关的文件，只保留正在需要的东西。
>
> 格式1：`RUN wget -O - https://some.site | wc -l > /number `
>
> 格式2：`RUN ["可执行文件", "参数1", "参数2"]`
>
> 案例：`RUN mkdir -p app && apt-get update && npm install`

* CMD

> `CMD`指令用于指定容器启动时需要运行的命令。
>
> 格式：`CMD ["可执行文件", "参数1", "参数2"]`
>
> 案例：`CMD ['npm', 'app']`
>
> **注意：使用`docker run`命令可以覆盖 `CMD` 指令 ，假设`Dockerfile` 中的 `CMD`为`CMD ["npm", 'app']`, 当执行 `docker run -it app /bin/bash`的时候，并不会执行 `npm app` 而是执行 `/bin/bash`**。

* EXPOSE

> `EXPOSE`指令告诉Docker该容器内的应用程序将会使用容器的指定端口。可以使用多个`EXPOSE`指令向外部公开多个端口。
>
> 格式：`EXPOSE <port>`
>
> 案例：`EXPOSE 3000`

* ENV

> `ENV`指令用来在镜像构建过程中设置环境变量。这些环境变量也会被持久保存到创建的容器中。
>
> 可以通过`docker run -e "NODE_ENV=test"`传递环境变量。
>
> 格式：`ENV <key> <value>`
>
> 案例： `ENV NODE_ENV test `

* ADD

> `ADD`指令与`COPY`格式和性质基本一致。`ADD`的源路径可以是一个`URL`。尽可能使用 `copy`。
>
> 只有当 `COPY`达不到要求的时候才使用。类如源路径是远程的链接地址。

* COPY

> `COPY`指令将从构建上下文目录中的文件复制到新的一层镜像内的位置。
>
> 格式： `COPY <原路径> <目标路径> `
>
> 案例： `COPY package*.json /app/`

* ENTRYPOINT

> `ENTRYPOINT`指令和`COPY`类似。区别在于`ENTRYPOINT`不会在容器启动时被覆盖。`docker run `命令行中指定的参数都会被当做参数传递给 `ENTRYPOINT`。

* VOLUME

> `VOLUME`指令用来创建一个可以从本地或其他容器挂载的挂载点，用于数据持久化。
>
> 格式：`VOLUME [主机目录]`
>
> 案例：`VOLUME ['/data', '/logs']`

* USER

> `USER`指令指定镜像会以什么样的用户去运行。可以通过`docker run -u nginx`覆盖该指令。不指定将会以`root`用户运行。
>
> 格式：`USER <用户>`
>
> 案例：`USER nginx`

* WORKDIR

> `WORKDIR`指令是在从该镜像创建容器时。在容器内部创建一个工作目录，`ENTRYPOINT`和`CMD`	指定的程序会在这个目录下执行。在`docker run `时候可以使用`-w`覆盖工作录。`WORKDIR`指令可以为 `Dockerfile`中其他指令设置工作目录。
>
> 格式：`WORKDIR <path>`
>
> 案例：`WORKDIR /usr/src/api`

* ONBUILD

> `ONBUILD`指令式当以当前镜像为基础镜像，去构建下一级镜像的时候被执行。
>
> 格式：`ONBUILD <其他指令>`
>
> 案例： `ONBUILD COPY ./package.json /app`

* MANITAINER

> `MANITAINER`指令指定该镜像的作者信息。姓名和联系方式
>
> 格式：`MANITAINER <姓名> <联系方式>`
>
> 案例： `MANITAINER James "james@example.com"`

* LABEL

> `LABEL`指令用于为该镜像添加一些元数据，元数据以键值对的形式展示。
>
> 格式：`LABEL <key=value> <key1=value1>...`
>
> 案例：`LABEL location="New Youk" type="Data Center"`