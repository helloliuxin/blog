

## Webpack

webpack的核心思想：**一切皆为模块**



### 为什么需要构建工具

- 转换ES6语法
- 转换JSX
- CSS前缀不全/预处理器
- 压缩混淆
- 图片压缩



### 前端构建演变之路

![image-20200423005931379](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200423005931379.png)



### 初识webpack: webpack配置组成

 webpack 默认配置文件： webpack.config.js

可以通过webpack --config 指定配置文件

![image-20200423010604710](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200423010604710.png)

![image-20200423010639116](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200423010639116.png)

![image-20200423011018269](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200423011018269.png)

 ![image-20200423012822698](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200423012822698.png)



### 环境搭建： 安装webpack和webpack-cli

1. 前提条件，安装Node.js和NPM

   检查是否安装成功： node -v , npm -v

2. 创建空目录和 pack.json

   - mkdir my-project
   - cd my-project
   - npm init -y

3. 安装webpack 和 webpack-cli

   ```js
   npm install webpack webpack-cli --save-dev
   ```

   检查是否安装成功： ./node_modules/.bin/webpack -v



### 运行webpack

- 不使用npm script 运行webpack

  需要切换到node_modules/.bin 目录下  使用webpack启动

  ```
  D:\webpack_project\my_project\node_modules\.bin>webpack
  ```

  > 注意：在window系统下，entry路径为../../src/index.js

- 通过npm script 运行webpack

  在pack.json下的“scripts" 下加“ build”: ”webpack"命令

  通过`npm run build`运行构建

  原理：模块局部安装会在node_modules/.bin目录创建软连接



### 核心概念之Entry

Entry用来指定webpack的打包入口



#### 理解依赖图的含义

![image-20200425190012627](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200425190012627.png)



#### Entry的用法

1. 单入口：entry是一个字符串

   ```js
   module.exports = {
       entry: './path/to/my/entry/file.js'
   }
   ```

2. 多入口： entry是一个对象

   ```js
   module.exports = {
       entry: {
           app: './src/app.js',
           adminApp: './src/adminApp.js'
       }
   }
   ```

   

### 核心概念之Output

Output用来告诉webpack如何将编译后的文件输出到磁盘

#### Output的用法

1. 单入口配置

   ```js
   module.exports = {
       entry: './path/to/my/entry/file.js',
       output: {
           filename: 'bundle.js',
           path: __dirname + '/dist'
       }
   }
   ```

2. 多入口配置

   ```js
   // 通过占位符[name]确保文件名称的唯一
   module.exports = {
       entry: {
           app: './src/app.js',
           adminApp: './src/adminApp.js'
       },
       output: {
           filename: '[name].js',
           path: __dirname + '/dist'
       }
   }
   
   ```

   



### 核心概念之Loaders

webpack开箱即用只支持`JS`和`JSON`两种文件类型，通过Loaders去支持其它文件类型并且把他们转化成有效的模块，并且可以添加到依赖图中。



本身是一个函数，接受源文件作为参数，返回转换的结果。



#### 常见的Loaders有哪些

![image-20200425191339613](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200425191339613.png)



#### Loaders的用法

**test ** 指定匹配规则， **use** 指定使用的loader名称

```js
const path = require('path');

module.exports = {
  entry: './src/index.js', // 入口文件
  output: {
    path: path.join(__dirname, 'dist'), // 输出路径
    filename: 'bundle.js' //指定打出输出的文件名
  },
  module: {
      rules: [
          {
              text: /\.txt$/,
              use: 'raw-loader'
          }
      ]
  }
  mode: 'production' // 生产模式
}
```



### 核心概念之Plugins

插件用于bundle文件的优化，资源管理和环境变量注入

作用于整个构建过程



#### 常见的Plugins有哪些

![image-20200425192418976](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200425192418976.png)

#### Plugins的用法

```js
const path = require('path');

module.exports = {
    output: {
        filename: 'bundle.js'
    },
    plugins: [
        // 放到plugins数组里
        new HtmlWebpackPlugin({
            template: './src/index.html'
        })
    ]
}
```



### 核心概念之Mode

Mode用来指定当前的构建环境是: production、 development 还是 none

设置mode可以使用webpack内置的函数，默认值为production



#### Mode的内置函数功能

![image-20200425192825865](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200425192825865.png)



### 解析ES6和React JSX

1. 解析ES6

```js
npm i @babel/core @babel/preset-env babel-loader -D
```

![image-20200425195756817](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200425195756817.png)



在.babelrc文件下，增加ES6的babel preset配置

![image-20200425200436882](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200425200436882.png)



2. 解析React JSX

```js
npm i react react-dom @babel/preset-react -D
```

```js
// search.js
import React from 'react';
import ReactDOM from 'react-dom';

class Search extends React.Component {

  render() {
    return <div>Search Text</div>
  }
}

ReactDOM.render(
  <Search />,
  document.getElementById('root')
)

```

在.babelrc文件下增加React的 babel preset配置

```js
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}
```



### 解析CSS、Less、 Sass

1. 解析CSS

   ```JS
   npm i style-loader css-loader -D
   
   
   
   ```

   `css-loader`用于加载.css文件，并且转换为commonjs对象

   `style-loader`将样式通过<style>标签插入到head中

   ```js
   module.exports = {
     entry: {
       index: './src/index.js',
       search: './src/search.js'
     },
     output: {
       path: path.join(__dirname, 'dist'), // 输出路径
       filename: '[name].js' // 文件别名
     },
     module: {
       rules: [
         {
           test: /\.js$/,
           use: 'babel-loader'
         },
         {
           test: /\.css$/,
           use: [
             'style-loader',
             'css-loader'
           ]
         }
       ]
     },
     mode: 'production'
   }
   ```

   > loader的调用时链式调用，调用顺序是从右到左的，所以我们要先写style-loader再写css-loader

   

2. 解析less

  `less-loader`用于将less转换为css

```
npm i less less-loader -D
```

```js
{
        test: /\.less$/,
        use: [
          'style-loader',
          'css-loader',
          'less-loader'
        ]
      }

```

3. 解析sass (同理参考less)



### 解析图片和字体

1. 使用file-loader处理图片和字体

   ```js
   npm i file-loader -D
   ```

   ```js
   	{
           test: /.(png|jpg|gif|jpeg)$/,
           use: 'file-loader'
         },
         {
           test: /.(woff|woff2|eot|ttf|otf)$/,
           use: 'file-loader'
         }
   ```

   

2. url-loader也可以处理图片和字体

   > 用url-loader处理图片和字体可以设置较小资源自动base64

   ```
   npm i url-loader -D
   ```

   ```js
   {
   	test: /.(png|jpg|gif|jpeg)$/,
   	use: [
   		{
   			loader: 'url-loader',
   			options: {
   				limit: 10240
   			}
   		}
   	]
   }
   ```

   

### webpack中的文件监听

文件监听是在发现源码发生变化时，自动重新构建出新的输出文件



webpack开启监听模式，有两种方式：

- 启动webpack命令时，带上--watch参数
- 在配置webpack.config.js中设置watch: true



webpack中的文件监听使用

![image-20200425213516808](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200425213516808.png)

运行命令

```js
npm run watch
```



文件监听的原理分析

![image-20200425213941592](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200425213941592.png)



### 热更新：webpack-dev-server

```js
npm i webpack-dev-server -D
```

![image-20200425222345859](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200425222345859.png)

![image-20200425222514193](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200425222514193.png)

热更新：使用webpack-dev-middleware

![image-20200425222608378](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200425222608378.png)

热更新的原理分析

![image-20200425222636202](C:\Users\liuxin\AppData\Roaming\Typora\typora-user-images\image-20200425222636202.png)



**HotModuleReplacementPlugin做什么的**

这里面的热更新有最核心的是 HMR Server 和 HMR runtime。

HMR Server 是服务端，用来将变化的 js 模块通过 websocket 的消息通知给浏览器端。

HMR Runtime是浏览器端，用于接受 HMR Server 传递的模块数据，浏览器端可以看到 .hot-update.json 的文件过来。

继续回到这个问题：老师HotModuleReplacementPlugin是做什么用的？

webpack 构建出来的 bundle.js 本身是不具备热更新的能力的，HotModuleReplacementPlugin 的作用就是将 HMR runtime 注入到 bundle.js，使得bundle.js可以和HMR server建立websocket的通信连接



**webpack-dev-server和hot-module-replacement-plugin之间的关系**

需要从两者的功能上来分析说明：

webpack-dev-server(WDS)的功能提供 bundle server的能力，就是生成的 bundle.js 文件可以通过 localhost://xxx 的方式去访问，另外 WDS 也提供 livereload(浏览器的自动刷新)。

hot-module-replacement-plugin 的作用是提供 HMR 的 runtime，并且将 runtime 注入到 bundle.js 代码里面去。一旦磁盘里面的文件修改，那么 HMR server 会将有修改的 js module 信息发送给 HMR runtime，然后 HMR runtime 去局部更新页面的代码。因此这种方式可以不用刷新浏览器。

单独写两个包也是出于功能的解耦来考虑的。简单来说就是：hot-module-replacement-plugin 包给 webpack-dev-server 提供了热更新的能力



**热更新的两个阶段**

热更新分两个阶段，启动阶段还是依赖磁盘文件去编译。更新阶段是直接内存增量更新的