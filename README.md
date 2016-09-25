## 安装React，webpack

```
npm install react --save-dev
npm install webpack --seve-dav
```

新建一个名为webpack.config.js的文件，它应该长这个样：

```
var path = require('path');

module.exports = {
  entry: path.resolve(__dirname, './app/main.js'),
  output: {
    path: path.resolve(__dirname, './build'),
    filename: 'bundle.js',
  }
};

```

其中entry指定了webpack的入口程序，好比c++和java中的main程序一样，我们把最终要插入到页面指定位置的react模板写入main.js中：

```
import React from 'react';
import ReactDOM from 'react-dom';
import ProductBox from './components/productBox.js'

ReactDOM.render(<ProductBox />, document.getElementById('content'));
```

## 安装并启用`webpack-dev-server`
`webpack-dev-server`允许我们可以把本地项目跑在像nginx那样的web服务器上，更重要的是我们可以在package.json文件内定义scripts，同时修改webpack的配置文件来达到类似BrowserSync（即文件修改能被监听，并自动刷新浏览器）的效果！

我们先打开package.json，并找到scripts代码块。在没引入`webpack-dev-server`之




## Webpack Loader
讲到这里，我们基本上就可以迅速搭建一个简单的web项目，但不得不提的是webpack loader。它是我个人认为相比于其他模块加载更牛X的地方，将它用于react的开发，结合react与生俱来的优越性能，两者天衣无缝的配合简直就是黄金组合。

总的来说webpack loader可以实现：

可以将React JSX语法转为js语句
React开发中支持ES6语法
支持通过import来直接引入css、less、sass甚至是图片
支持css中引用的图片大小在某一大小范围之内直接转为BASE64格式
。。。。。等等等

### 安装加载器
```
npm install babel-loader css-loader style-loader --save-dev
```

### 安装babel的转码规则

```
npm install babel-preset-es2015 babel-preset-react --save-dev
```

```
var webpack = require('webpack');

module.exports = {
    entry: './js/app.js',
    output: {
        path: __dirname,
        filename: 'bundle.js'
    },
    module: {
        loaders: [
            {
                test: /\.jsx?$/,
                loader: 'babel',
                query: {
                    presets: ['react', 'es2015']
                }
            },
            {
                test: /\.css$/,
                loader: 'style!css'
            }
        ]
    },
    plugins: [
        new webpack.optimize.UglifyJsPlugin({
            compress: {
                warnings: false
            }
        })
    ]
}
```

另外这里使用到了 webpack 的一个内置插件 `UglifyJsPlugin`，通过他可以对生成的文件进行压缩。详细的介绍请看

如果没有这个插件，编译会出现问题？
上一篇文章 里是使用 webpack 进行 ES6 开发，其实不管是 ES6 也好，React 也好，webpack 起到的是一个打包器的作用，配置项和这里大致相似，就不再赘述。

不同的是在 babel-loader 里增加了 react 的转码规则。


## Couldn't find preset "react" relative to directory

### 方式一(不推荐)
```
#npm remove babel-core babel-loader babel-preset-react –save-dev
//首先移除原先安装的babel
#npm install babel-core babel-loader babel-preset-react –g
//全局安装
```

### 方式二(不推荐)
```
# npm remove webpack –g
//移除原先的webpack
# npm install webpack –save-dev
//将webpack安装到本地位置——也就是项目目录下的node_modules中
# ln –s /项目根目录/node_modules/webpack/bin/webpack.js /usr/bin/webpack
//为了使用webpack方便，在这里我们在/usr/bin目录下建立软连接（也就是快捷方式）
//这样我们就可以在任意位置直接使用webpack命令了。
```

### 方式三(推荐)

```
module:{
loaders: [
	{
	test: /\.js$/,
	loader: 'babel',
	exclude:/node_modules/,
	query:{
		presets:['react']}
	}
]

}
```

## Couldn't find preset "es2015" relative to directory

解决方案，在`package.conf`中加入
```
"babel": {
	"presets": [
	"es2015",
	"react"
	]
},

```
