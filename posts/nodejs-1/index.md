# Webpack基础概念



JS模块化打包

<!--more-->

# 全局变量的约束

因为在js中如果需要使用全局变量,则需要将变量命名为window.varible
而在引入的包中,全局变量会失控,因此引入了webpack的概念
只将特定的变量进行暴露

```html
<script src="a.js"></script>
<script src="b.js"></script>
```

## model.js-作为一个模块
```js
//node写法
var msg = "yo";
module.exports = {msg: msg};	//变量暴露

//es6写法
var msg = "yo";
export {msg};
```

## entry.js-入口文件
```js
//node写法
var msg = require('./model.js').msg;
console.log(msg); //输出 yo

//es6写法
import {msg} from './model';
console.log(msg);
```

## webpack打包
```js
webpack b.js bundle.js //只需要输入 入口文件和出口文件名称即可
//然后在html文件中只需要引入bundle.js
```

## npm

### 自定义命令

```bash
"scripts" :{
	"dev": "webpack-dev-server",
	"build": "eslint ./src && webpack"
}
```

### 安装包

```bash
npm install xxx --save-dev
# --save 保存到package.json文件中
# -dev 作为开发环境的依赖
npm install --only=dev
# 只安装dev环境下的依赖
npm install # 默认安装生产环境下的依赖
```

### 卸载重装

```bash
rm -rf node_modules && npm install
```

# webpack基本构成

webpack作为模块化最关键的一环,可以将所有文件都模块化处理

## 全局安装

```bash
npm install webpack-cli -g # 全局安装
```

##  文件组成

### index.html

```html
<script src='./dist/bundle.js'></script>
```

### app.js

```js
import moduleLog from './moduleLog'
document.write('入口文件')
moduleLog()
```

### webpack.config.js

```js
const path = require('path')
const UglifyJSPlugin = require('uglifyjs-webpack-plugin')
module.exports = {
    entry = 'app.js' #入口文件
    output = {
    	path : path.join(__dirname, 'dist'), # 输出文件路径
        filename: 'bundle.js' # 输出文件名称
	},
    devServer: {
        port: 3000, # dev模式下的端口
        publicPath: './dist' # 缓存文件存储路径
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    # 有先后顺序
                    'style-loader' # 加载到页面中
                    'css-loader' # 读取css文件
                ]
            }
        ]
    },
    plugins: [
        new UglifyJSPlugin()
    ]
}
```

## 加载css文件

使用`css-loader`来实现,并且独立于webpack存在,需要安装

```bash
npm install css-loader --save -dev
npm install style-loader --save -dev
```

### style.css

```css
body{
    color: 'red'
}
```

### app.js

```js
import './style.css'
```

## plugin

压缩js文件体积`uglyfyjs`插件

```bash
npm install uglify-webpack-plugin
```



## 调试

webpack-dev-server 不会生成bundle.js,只存在于缓存中

动态实现重新打包,可全局安装,也可以直接执行文件

```bash
./node_modules/.bin/webpack-dev-server
```

# webpack构建工程

## 初始化项目

```bash
npm init -y # -y默认配置
```

## 安装react

```bash
npm install react react-dom
```

## 安装webpack

```bash
npm install webpack webpack-cli -g
```

## 创建文件 

### src/App.jsx

```jsx
import React from 'react'
import ReactDom from 'react-dom'

//ES6 箭头函数
const App = () => {
    return (
    	<div>
        	<h1>我没学过React</h1>
        </div>
    )
}

export default App
React.render(<App/>, document.getElementById('app'))
```

### index.html

```html
<body>
    <div id="app"></div>
</body>
```

## 安装babel

将ES6变为版本较低的代码

```bash
cd ..
mkdir babel-demo
cd babel-demo
npm init -y
## 使用场景: babel代码编译=>编译为低版本代码
npm install @babel/core @babel/cli -g
```

### 创建ES6测试代码

```js
//test.js
[1,2,3].map((item)=>{
    console.log(item)
})
```

### 安装转换规则

```bash
npm install @babel/preset-env #集成包
```

### 指定规则编译

```bash
babel test.js --preset=@babel/preset-env
```

### 配置文件(package.json)

```json
# package.json文件中
"babel": {
    "presets": ["@babel/preset-env"]
}
# babel test.js
```

### 配置文件(.babellrc)

```bash
# .babellrc文件
{
	"preset": ["@babel/preset-env"]
}
```

## webpack.config.js

```js
const HtmlWebPackPlugin = require('html-webpack-plugin')
const path = require('path')
const webpack = require('webpack')

module.exports = {
    resolve: {
        # 设置文件后缀
        extension: ['.wasm', '.jsx','js','json']
    },
    # 设置入口文件
    entry: path.resolve(__dirname, 'src/index.jsx')
    module: {
        # 利用正则不去解析npm库中的Jquery(省时间)
        noParse: /node_module\/(jquery\.js)/,
        rules: [
            {
                test: /\.jsx?/, #正则
                exclude: /node_modules/, #排除文件,文件过大
                include: xxx # 指定目录
                use: {
                loader: 'babel-loader'	
                option: {
                	babelrc: false
                	preset:[
                # 转译react
                require.resolve('@babel/preset-react'),
            	# 转译ES6
            	[require.resolve('@babel/preset',{module:false})]
            	],
                # 使用缓存
                cacheDirectory: true,
            	}
            	}
            }
        ]
    },
    plugins: [
        new HtmlWebPackPlugin({
            template: path.resolve(__dirname, 'src/index.html'),
            filenameL 'index.html'
        }),
        # HMR热更新
        new webpack.HotReplacementPlugin()
    ],
    devServer: {
        hot: true
    }
}
```

### 安装转译规则

```bash
npm install @babel/preset-env @babel/preset-react
```

### 安装html-plugin

```bash
npm install html-webpack-plugin 
```

## index.jsx

```js
import App from './App'

if(module.hot){
    module.hot.accept(error =>{
        if(error){
            console.log("热更新出错")
        }
    })
}
```



# 性能调优

- 打包结果的优化
- 构建过程的优化
- Tree-shaking

## 压缩 tenser

在`webpack.config.js`中添加

```bash
const TenserPlugin = require('tenser-webpack-plugin')
# 是uglyfy的分支
module.exports={
	optimization: {
		minimizer: [new TenserPlugin({
			cache: true	#使用缓存加快构建速度
			paralle: true, #开启多线程压缩
			tensorOptions: {
				compress: {
					unused: true, #去除没有用过的代码
					drop_debugger: true, #删除调试代码
					drop_console: true, #删除控制台显示代码
					dead_code: true #删除无用代码
				}
			}
		})]
	}
}
```

## 压缩结果分析器

`webpack bundle analyzer`--可视化查看文件大小

```bash
npm install webpack bundle analyzer
```

在`webpack.config.js`中添加

```js
const BundleAnalyzerPlugin = require('webpack bundle analyzer').BundleAnalyzerPlugin

plugin: [
    new BundleAnalyzerPlugin()
]
```

## 多线程处理

webpack是单线程处理,使用`HappyPack`或`thread-loader`来实现多线程

### thread-loader

在`webpack.config.js`中添加

```js
# 放在所有loader之前
module: {
    rules: [
        test: /\/js$/,
        include: path.resolve('src'),
        use: [
            'thread-loader'
        ]
    ]
}
```

### HappyPack

```js
const HappyPack = require('happypack')
# 根据cpu的数量来创建一个线程池
const happyThreadPool.ThreadPool({size: OscillatorNode.cpus().length})

# 在plugins中添加
plugins: [
    new HappyPack({
        id: 'jsx',
        threads: happyThreadPool,
        # 要查看loader是否支持happypack
        loaders: ['babel-loader', '...']
    })
]
```

## Tree-shaking

消除无用的JS代码


