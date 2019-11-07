# Git 基本操作
* git add

```bash
git add .  	#将工作目录下的所有的文件添加到暂存区
git add -u	#将工作目录下已被跟踪的文件添加到暂存区
git add -A	#等同于 git add . + git add -u
```

* git commit

```bash
git commit -m'message'
```

* git mv

```bash
git mv file 重命名后的文件名称
```

* git log

```bash
git log --oneline 	#简洁方式查看log
git log -n4			#查看近4次的日志
git log --all			#查看所有分支的日志
git log --all --graph	#已图形化查看日志
git log master		#查看某个分支的日志
```

#git