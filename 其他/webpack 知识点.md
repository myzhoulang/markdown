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

  

  ```json
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

  