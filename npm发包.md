# 使用npm publish发包到npm

1. 需要到[`npmjs.com`](https://https://www.npmjs.com/signup)注册一个`npm` 账号。

2. 需要将 `npm registry`切换到 `registry.npmjs.org`。

   ```bash
   npm config set registry https://registry.npmjs.org
   ```

3. 在项目根目录下，打开终端。如果第一次发包需要添加用户执行 `npm adduser`。如果不是第一次发包，执行`npm login`登录，登录时的用户名前面需要加`~`。且注册时候填写的邮箱已被认证

4. 登录成功后使用`npm publish`发布。



**注意**：

* 发包的时候注意项目名称，不能和`npmjs`已有的包名称相同。如果不能改名称，可以考虑发作用域包。

  package.json:

  ```json
  {
    "name": "@username/名称"
  }
  ```

   **如果使用相同的作用域包，在每次npm init的时候自动加上作用域，可以配置 `npm config set scope username`**

* 作用域包默认是私有的，而发布私有的包是需要付费用户。所以需要指定发布公共作用域包。

  ```bash
  npm publish --access=public
  ```

* 在每次修改代码后需要发包的时候 ，需要修改版本号。可以使用 `npm version` 命令

  ```bash
  npm version 
  ```

  