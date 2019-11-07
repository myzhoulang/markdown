# Git 最基础配置
> 使用`git config` 配置变量，可以有3种不同级别的配置，分别为 *系统级*、*用户级*、*单个仓库级*。默认是`local`
> 配置。

* 系统级（system）
针对的是系统所有的登陆用户，

> 案例：设置用户名和email

```bash
git config --system user.name 'xxx'
git config --system user.email'xxx@xx.xx'
```

* 用户级（global）（常用的）
针对的是当前系统登陆用户的所有仓库

> 案例：设置用户名和email

```bash
git config --global user.name 'xxx'
git config --global user.email 'xxx@xx.xx'
```

* 单个仓库级别（local）(基本很少用)
只针对某个仓库

> 案例：设置用户名和email

```bash
git config --local user.name 'xxx'
git config --local user.email 'xxx@xx.xx'
```

> 可以使用 `git config --list [--local| --system| --global]` 查看当前级别下的配置  
> 优先级：local > global > system  

## 常用设置
* user.name   （操作的用户）
* user.email   （操作的用户email）
* core.editor  （输入信息时候调用的文本编辑器，默认：vim）