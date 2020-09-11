# webpack 知识点

* `webpack`默认的配置文件是 `webpack.config.js`， 可以使用 `webapck --config 指定的文件`。

* `webpack`配置文件组成

  ```js
  module.exports = {
    entry: './src/index.js',	// webpack 入口文件
    output: './dist/main.js',	// 构建后输出的位置
    mode: 'production',				// 构建的环境
    module: {		
      // 规则 使用loader处理匹配到的文件
      rules: [{test: /\.txt$/, use: 'raw-loader'}]
    },
    plugins: [ // 插件 处理 loader 完成后的东西， 如压缩 文件指纹，将文件插入到html中
      new HtmlwebpackPlugin({
        template: './src/index.html'
      })
    ]
  }
  ```

* `Entry`有单入口和多入口，单入口 `entry`是一个字符串。多入口是一个对象

  ```js
  module.exports = {
    // entry: './src/index.js', // 单入口
    entry: {	// 多入口方式
      app: './src/app.js',
      admin: './src/admin.js'
    }
  }
  ```

* `output`指定构建后文件的输出位置。`output`的值一个对象。

  ```javascript
  const path = require('path')
  module.exports = {
     entry: {	// 多入口方式
      app: './src/app.js',
      admin: './src/admin.js'
    },
    
    output: {
      path: path.join(__dirname, 'dist'),
      filename: '[name].js'	// 单入口时 可以写死。 多入口 可以使用 `[name]`占位符 动态生成文件名称
    }
  }
  ```

* `mode` 指定构建时候的环境， 值有`production`、`development`、`none`, 指定不通的值，`webpack`会做对应的优化。



## 前端常用loader解析

* 使用`babel-loader`解析`es6`语法和`react`语法。还需要新建一个`.babelrc`文件

  ```javascript
  // webpack.config.js
  const path = require('path');
  
  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.join(__dirname, 'dist')
    },
    
    module: {
      rules: [
        { test: /\.js$/, use: 'babel-loader'}
      ]
    }
  }
  ```

  

  ```javascript
  // .babelrc
  {
    "presets": [
      "@babel/preset-env",  // es6 babel preset
      "@babel/preset-react"	// react preset
    ],
    "plugins": [
      "@babel/proposal-class-properties"
    ]
  }
  ```

* 使用`css-loader`和`style-loader`解析`CSS`。`css-loader` 用于加载`.css`文件。并转换成 `commonjs`对象。 `style-loader`将样式通过 `<style>`插入到`head`中 。 这里不会生成 `.css`文件，要生成 `.css` 需要使用`plugin`将`CSS`抽取出生成。

  ```javascript
  const path = require('path');
  
  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.join(__dirname, 'dist')
    },
    
    module: {
      rules: [
        { test: /\.js$/, use: 'babel-loader'},
        { text: /\.css$/, use: ['style-loader', 'css-loader']}
      ]
    }
  }
  ```

* 使用`less-loader` 解析`less`。`less`文件需要 `less`解析器，所以需要同时安装`less`模块。`less-loader`处理完成后 丢给`css-loader`处理。

  ```javascript
  const path = require('path');
  
  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.join(__dirname, 'dist')
    },
    
    module: {
      rules: [
        { test: /\.js$/, use: 'babel-loader'},
        { text: /\.css$/, use: ['style-loader', 'css-loader']},
        { text: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader']}
      ]
    }
  }
  ```

* 使用`url-loader`解析图片文件和字体,可指定文件超出大小，自动转base64。

  ```javascript
  const path = require('path');
  
  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.join(__dirname, 'dist')
    },
    
    module: {
      rules: [
        { test: /\.js$/, use: 'babel-loader'},
        { test: /\.css$/, use: ['style-loader', 'css-loader']},
        { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader']},
        { test: /\.(jpg|jpeg|png|gif)$/, use: [{
          loader: 'url-loader',
          options: {
            limit: 10240
          }
        }]}
      ]
    }
  }
  ```

* webpack 构建 library 配置

  ```js
  const path = require('path');
  
  module.exports = {
    mode: 'production',
    entry: './src/index.js',
    output: {
      path: path.resolve(__dirname, 'dist'),
      filename: 'track.js',
      library: 'Track',
      globalObject: 'this',
      libraryTarget: 'umd'
    }
  };
  ```

  

* 文件监听

  > 监听源代码，自动重新构建。有两种方式可以配置。
  >
  > 在`webpack.config.js`中配置`watch`属性。还可以配置`watchOptions`选项。

  ```js
  module.export = {
      watch: true,
      // 配合 watch 使用， 只有 watch: true 时才生效
      watchOptions: {
          // 不需要监听的文件或文件夹 默认为空
          ignored: /node_modules/,
          // 监听到变化后等待多长时间采取执行 默认 300ms
          // 在这个时间段更改的文件将会被合并成一次执行
          aggregateTimeout:300,
          // 检测文件变动的频率 默认每秒检测一次 单位ms
          poll: 1000
      }
  }
  ```

  

  1. `webpack` 启动命令时，带上 `--watch` 参数。
  2. 在`webpack.config.js`中设置`watch`属性为`true`。

* 热更新

  > 使用`webpack-dev-server`和`HotModuleReplacementPlugin`插件实现热更新.
  >
  > `webpack-dev-server` 会启动一个资源服务器.
  >
  > 它能观测文件变化执行编译过程, 而且它是不输出文件到磁盘,而是写入内存.
  >
  > 并且还可以自动刷新浏览器.
  >
  > `HotModuleReplacementPlugin`

  `package.json`

  ```json
  
  {
      "dev": "webpack-dev-server --open"
  }
  ```

  

  ```js
  // 引入 webpack HotModuleReplacementPlugin 插件时在 webpack 里面.
  const webpack = require('webpack');
  module.export = {
      plugins: [
          // 在devServer 中指定 hot 为 true时, 可以不用添加以下插件
          // https://www.webpackjs.com/configuration/dev-server/#devserver-hot
          new webpack.HotModuleReplacementPlugin()
      ],
      devServer: {
          contentBase: './dist', 	// 指定启动服务的根目录
          hot: true				// 开启热更新
      }
  }
  ```

  问: 

  1. `HotModuleReplacementPlugin`的作用?
  2. 热更新原理

* `Hash` 、`Chunkhash`、 `Contenthash` 什么时候会改变和使用场景

  > `webpack`中所有的`hash`的值是一样的，只要文件有任何变化，下次构建生成的`hash`就会改变。如果使用`hash`作为文件指纹。当某一个文件发生改变，其他未任何修改的文件指纹都会改变，这样就没有缓存了。
  >
  > `Chunkhash`是一个`chunk`的`hash`,一个或多个模块产生一个`chunk`,每一个`chunkhash`都是不一样的。`chunk`中的任何一个模块发生改变，`Chunkhash`就会发生变化。
  >
  > `Contenthash`是具体一个模块的`hash`，一个`chunk`中可能有各种类型的模块，如`css`、图片、`js`，他们的加载方式是分开的。当某一类型的模块发生改变，如果使用`chunkhash`,其他未修改的模块的`hash`就会发生变化，这样就达不到缓存效果
  >
  > * **`file-loader`中的 `[hash]`占位符是文件内容的`hash`，默认md5生成**

  

* 代码压缩

> `js`在`mode`设置成`production`的时候会自动采用`uglifyjs-webpack-plugin`插件压缩代码
>
> ```js
> module.exports = {
>   mode: "production"
> }
> ```
>
> 
>
> `css`文件可以采用第三方的`optimize-css-assets-webpack-plugin`插件压缩
>
> ```js
> const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin')
> module.exports = {
>   plugins: [
>     new OptimizeCssAssetsWebpackPlugin({
>       assetNameRegExp: /\.css$/g,
>       cssProcessor: require("cssnano"),
>     })
>   ]
> }
> ```
>
> 
>
> `html`可以采用`html-wbepack-plugin`插件压缩,`html-wbepack-plugin`不仅可以压缩,还可以生成`html`文件，还可以将指定的`chunk`插入到`html`文件中。
>
> ```js
> const HtmlWebpackPlugin = require("html-webpack-plugin")
> module.exports = {
>   plugins: [
>     new HtmlWebpackPlugin({
>       template: path.join(__dirname, "src/index.html"),
>       filename: "index.html",
>       chunks: ["index"],
>       inject: true,
>       minify: {
>         html5: true,
>         collapseWhitespace: true,
>         preserveLineBreaks: false,
>         minifyCSS: true,
>         minifyJS: true,
>         removeComments: false,
>       },
>     }),
>   ]
> }
> ```
>
> 

* 自动清理构建的目录

  > 使用`clean-webpack-plugin`插件清除构建的目录
  >
  > ```js
  > const { CleanWebpackPlugin }= require('clean-webpack-plugin')
  > module.exports = {
  >   plugins: [
  >     new CleanWebpackPlugin()
  >   ]
  > }
  > ```
  >
  > 

* 使用`postcss-loader`和`autoprefixer`自动补全`css3`前缀

> ```js
> module.exports = {
>   module: {
>     rules: [
>       {test: /\.less$/, use: [
>         	"style-loader",
>           "css-loader",
>           "less-loader",
>         {
>           loader: "postcss-loader",
>           plugins: () => [
>             require("autoprefixer")({browsers: ["last 2 version"]})
>           ]
>         }
>       ]}
>     ]
>   }
> }
> ```
>
> 或者在`package.json`中设置浏览器, 在`webpack.config.js`中直接使用
>
> ```json
> {
>   "browserslist": [
>     "last 2 versions",
>     "> 1%"
>   ],
> }
> ```
>
> ```js
> module.exports = {
>   module: {
>     rules: [
>       {test: /\.less$/, use: [
>         	"style-loader",
>           "css-loader",
>           "less-loader",
>         {
>           loader: "postcss-loader",
>           options: {
>             plugins: [require("autoprefixer")]
>           }
>         }
>       ]}
>     ]
>   }
> }
> ```
>
> 

* 使用`px2rem-loader`移动端 `px` 转 `rem`, 还需要使用 `lib-flexible`动态计算根节点的字体大小

> ```js
> module.exports = {
>   module: {
>     rules: [
>       {test: /\.less$/, use: [
>         	"style-loader",
>           "css-loader",
>           "less-loader",
>         {
>           loader: "postcss-loader",
>           options: {
>             plugins: [require("autoprefixer")]
>           }
>           
>         },
>         {
>           loader: "px2rem-loader",
>           options: {
>             remUnit: 75,		//对应设计稿 1rem = 75
>             remPrecesion: 8 
>           }
>         }
>       ]},
>     ]
>   }
> }
> ```
>
> 



* 使用`raw-loader`内联一些资源

  > ```html
  > <head>
  >   <script><%= require('raw-loader!./node_module/lib-flexible/flexible') %></script>
  > </head>
  > ```
  >
  > 

* 多页面打包

  > 多页面打包首先需要在一个目录下根据一些规则创建目录，然后使用 `glob`库去读取这些目录，然后动态生成 `entry`和`htmlWebpackPlugin`。

* 分离公共包

  > 分离公共包有两种方式，一种是使用`html-webpack-externals-plugin`将公共包提取，一种使用`splitChunk`选项进行公共包提取
  >
  > * `html-webpack-externals-plugin`
  >
  >   ```js
  >   module.exports = {
  >     plugins: [
  >       new HtmlWebpackExternalsPlugin({
  >         
  >       })
  >     ]
  >   }
  >   ```

* `Tree shaking`

  > 1. `Tree shanking`是什么
  >
  >    `Tree shaking`（摇树优化）在打包构建过程中移除那些未引用的代码。从而减少打包后的文件大小。
  >
  > 2. `Tree shanking`原理
  >
  > 3.  如何开启`Tree shanking`
  >
  > 