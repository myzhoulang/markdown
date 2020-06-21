

# webpack 自定义loader

### 自定义`loader`编写格式

> 一个`loader`就是一个`node`的模块，将源代码写入，返回处理后的代码。
>
> **格式：**
>
> ```javascript
> module.exports = function (source){
>   // 处理逻辑
>   return source
> }
> ```
>
> 

### 自定义`loader`的输入输出

> `webpack`使用`loader`的时候，通过`options`处理参数给`loader`处理函数。在`loader`中使用`loader-utils`这个包的`getOptions`方法来接受参数。
>
> **获取参数:**
>
> ```javascript
> const loaderUtils = require("loader-utils");
> module.exports = function(content){
>   const { params } = loaderUtils.getOptions(this)
> }
> ```
>
> **`webpack.config.js`中传递参数：**
>
> ```javascript
> module.exports = {
>   // .....
>   module: {
>     rules: [
>       {test: /\.js$/, use: [{loader: 'a-loader', options: { params: "params"}}]}
>     ]
>   }
> }
> ```
>
> 使用`this.emitFile`将`loader`处理后的内容写入到文件中， 可以通过`loader-utils`中的`interpolateName`方法生成文件的存放路径。然后将内容写入。
>
> ```javascript
> const loaderUtils = require('loader-utils');
> 
> module.exports = function(content){
>   const url = loaderUtils.interpolateName(this, "[hash].[ext]", {
>     content
>   });
>   
>   this.emitFile(url, content);
> }
> ```
>
> 

### 同步`loader`和异步`loader`

> 对于同步的`loader`, 可以调用`callback`方法。 一些`loader`在处理的过程中需要一些异步操作，处理这类`loader`需要在函数中先调用`async`方法，在调用`callback`方法。或者返回一个`promise`。`callback`方法的第一个参数为发生的错误对象。后面的参数是处理结果，可以是多个。最终以数组的方式接受。



### `loader` 调试

> 使用[`loader-runner`](https://github.com/webpack/loader-runner)这个包进行调试。

### 其他

> 默认 `loader` 是缓存的, 即输入的内容和参数一样且当前`loader`没有依赖。 也可以手动关闭缓存，使用`this.cacheable(false)`。

