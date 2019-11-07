# Express.js
## 安装`express`

```bash
npm i express -save	#使用npm安装
yarn add express     #使用yarn安装
```

## Hello World 程序
> 使用`express`创建一个简单的hello world 程序, app.js 文件内容如下：

```js
const express = require('express'); // 导入express
const app = express();			  // 使用express 函数创建app程序

// 配置 / 路由， 发送 Hello World 给客户端，并关闭 http 链接
app.get('/', (req, res) => res.send('Hello World'));

// 启动服务器并设置应用程序监听3000端口
app.listen(3000, () => console.log('Server ready'))
```

> 运行上面的程序，打开终端输入：

```bash
node app.js
```

## 设置模板引擎
> express 可以自定义设置模板引擎。可以使用 `app.engine('模板文件后缀', 解析模板的引擎)`。可以使用`app.set('view engine', 模板文件后缀)`设置当渲染时候不写文件后缀的时候，会默认找这个后缀的文件。

```js
app.engine('pug', require('pug').__express)
app.set('view engine', 'pug')
```

## 视图和静态文件
> 对express 的视图文件和静态文件做设置。可以使用express的static 中间件处理。相当于给所有的静态文件创建了一个路由。**注意的是在模板中是不需要加上public这个一层目录的**

```js
app.use(express.static(__dirname + '/public')) // public为静态文件存放的目录
```

## 请求对象（request）
> 每一个请求都会带一个请求对象，这个对象包含一些请求信息，如: 请求url,参数，请求方法…
> 	- [x] params                           (路由参数)
> 	- [x] query                              (查询字符串参数)
> 	- [x] body                               (post请求参数)
> 	- [x] route                               (匹配路由信息)
> 	- [x] cookies                           (请求的cookies)
> 	- [x] headers                          (请求报头)
> 	- [x] accepts([types])            (客户接受的MIME类型)
> 	- [x] ip                                     (客户端IP地址)
> 	- [x] path                                (请求路径 不包含协议、主机、端口、查询字符串)
> 	- [x] host                                 (客户端主机名)
> 	- [x] protocol                           (客户端请求协议)
> 	- [x] secure                              (是否安全请求（https请求）)
> 	- [x] signedCookies                (签名cookie)
> 	- [x] xhr                                    (是否是XmlHttpRequest请求)

## 获取查询字符串
> 查询字符串是url中`?`后面的内容。常用于`get`请求。使用请求对象中的`query`属性，该属性会返回一个对象。如果没有查询字符串则会返回一个空对象`{}`  

```js
const experss = require('express');
const app = express();

ap.get('/', (req, res) => {
	// example: /?name=zhoulang&age=10
	console.log(res.query) // { name: 'zhoulang', age: 10}
})
```

## 获取路由参数
> 使用`req.params` 返回一个对象，分别是路由参数中定义的。

	- [x] 路由参数获取
	- [ ] 路由参数值类型设置 
	- [ ] 路由正则表达式设置

```js
app.get('/groups/:gname/users/:name', (req, res) => {
  // example: /groups/1/users/a
	console.log(res.params) // {gname:'1',name:'a'}
})
```

## 获取body请求体
> 使用**post** 请求的媒体类型有：
> 	* application/json  (ajax）
> 	* x-www-form-urlendcoded （form表单）
> 	* multipart/form-data （文件上传）

> express 4.x 要获取 **post** 请求的**body**体需要 `body-parser` 中间件解析编码体。**代码中要在请求代码之前使用**。

```js
const express = require('express');
const bodyParser = require('body-parser');
const app = express();

// 要在post请求之前调用
app.use(bodyParser())

app.post('/users', (req, res) => {
	console.log(req.body); // example: {name: 'abc', age: 1111}
  res.json(req.body);
})
```

## 获取json数据
> 对使用客户端使用 ajax 这类的请求，需要使用 `body-parser` 中间件的 `json` 方法处理。
> json 中间件可以配置一些属性。

	* inflate
	* limit           (控制请求体的大小，默认：100kb)
	* reviver       (一个function 可以修改解析生成的原始值)
	* strict          (启用或禁用仅接受数组和对象;禁用时将接受JSON.parse接受的任何内容)
	* type   ?
	* verify  ?

```js
app.use(bodyParser.json());
```

## 获取表单提交的数据
> 对使用像表单提交（content-type: ‘x-www-form-urlendcoded’）请求的数据 需要使用 `body-parser` 中间件的 `urlencoded` 方法处理。

```js
// extend: false value值可以是字符串或数组
// extend: true  value值可以是任意类型
app.use(bodyParse.urlencoded({extend: false}))
```

## 处理文件上传
> 要处理文件上传，客户端需要指定类型`multipart/form-data"` 。express4.x 需要安装第三方包。`Formidable`是一个比较好的选择。

```js
const formidable = require('formidable');

app.post('/upload', (req, res) => {
	const form = new formidable.IncomingForm();

	form.parse(req, (err, fields, files) => {
		console.log(files) // 文件数组对象
	})
})
```
