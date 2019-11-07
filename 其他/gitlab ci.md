# gitlab ci
## FAQ
* ContainerStart: Error response from daemon: Cannot link to a non running container:XXX， mount: permission denied (are you root?)
	1. 进入容器 `docker exec -it [容器ID] /bin/bash` 
	2. 找到 `/etc/gitlab-runner/config.toml`
	3. 修改 [[runners]]  > [runners.docker] > privileged 将值改为 `true`
	4. 重新启动 `gitlab-runner`

* 使用docker 容器安装gitlab-runnger 在macOS上，使用/Users/Shared代替/srv。

```docker  
	docker run -d --name gitlab-runner --restart always \
  	-v /Users/Shared/gitlab-runner/config:/etc/gitlab-runner \
  	-v /var/run/docker.sock:/var/run/docker.sock \
  	gitlab/gitlab-runner:latest
```

* 进入gitlab-runner 容器中注册gitlab 对应的项目

```bash
	docker run --rm -t -i -v /Users/Shared/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner register
```