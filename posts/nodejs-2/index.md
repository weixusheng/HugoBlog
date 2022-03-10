# nodejs入门



Mosh/初识nodejs
<!--more-->

# path模块
```js
const path = require('path') //built in module
//定义为常量不会被之后命名的变量改变
var pathObj = path.parse(__filename);
//返回一个文件路径的对象
```

# OS模块
```js
const os = require('os')
var totalMemory = os.totalmem(); //获取系统内存
var freeMemory = os.freemem(); //获取系统空余内存
console.log('Total mem' + totalMemory)
console.log('Total freemem' + freeMemory)
```

## Template string 打印
```js
/*
${variable_name}
*/
console.log(`total mem: ${totalMemory}`)
console.log(`total mem: ${freeMemory}`)
```

# fs模块
```js
/* 
同步函数
*/
const files = fs.readdirSync('./'); //同步函数
console.log(files);

/*
异步函数
*/
const fs = require('fs');
fs.readdir('./', function(err, files){  //异步回调函数
    if(err) console.log('Error', err)
    else console.log('Result', files)
})
```

# Event模块
**events模块是node的核心模块，几乎所有常用的node模块都继承了events模块，比如http、fs等**
## 实例化event对象
```js
const EventEmitter = require('events'); 
//eventemitter是一个class
const emitter = new EventEmitter(); //实例化对象
//emitter.addListener 和 emitter.on 都是监听

//register a listener
emitter.on('message logged', function(arg){ //arg传参
    console.log('listener called', arg)
})
```

## ES6 arrow function
**arrow function 省略function关键字 用 => 表示函数**

```js
emitter.on('messgae logged', (arg) => {
    console.log('listener called', arg)
}))
```

## 激活event
```js
emitter.emit('message logged', {id: 1, url: 'https://'}) //making a noise, raise an event
```

# 模块化文件
## app.js
```js
const logger = require('./logger_2');
//console.log(logger);
logger.log('message');
```

## logger.js
```js
var url  = 'http://mylogger.io/log';

function log(message){
    console.log(message);
}
console.log(__filename)
console.log(__dirname)

module.exports.log = log;//暴露函数
/*
也可以直接暴露对象
module.exports = log

在app.js中使用
log('message');
*/
```
**在nodejs中,模块文件都是被看作为一个函数包(模块包装函数)**

```js
// module wrapper function 

(function(exports, require, module, __filename, __dirname){
    ...code
})
```

# Event+模块
## app.js
```js
const EventEmitter = require('events');
const Logger = require('./logger_3');

const logger = new Logger(); //类实例化对象
//注册(监听)函数
logger.on('messagelogged', (arg) => {
    console.log('listener called', arg)
})

logger.log('message');
```

## logger_3.js
```js
const EventEmitter = require('events');

//定义类 Logger包含所有EventEmitter的属性
class Logger extends EventEmitter{
    log(message){
        console.log(message);
        //激活函数
        this.emit('messagelogged', {id: 1, url: 'https'})
    }
}

module.exports = Logger;	//返回类
```

# http模块
```js
const http = require('http');

// 路由
const server = http.createServer((req, res) => {
    if(req.url === '/'){
        res.write('hello world')
        res.end()
    }
    if(req.url === '/api/course'){
        res.write(JSON.stringify([1,2,3]))
        res.end()
    }
});

/* 监听连接和关闭状态
server.on('connection', (socket) => {
    console.log('new connection')
})
server.on('close', () => {
            console.log('TCP server closed');
})
*/

server.listen(3000)
console.log('listening on port 3000')
```
