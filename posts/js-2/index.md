# Javascript基础 Part-2



常用命令
<!--more-->

## Symbol数据类型
ES 6 引入了一个新的数据类型 Symbol

为了说明 Symbol 的作用，我们先来描述一个使用场景。

我们在做一个游戏程序，用户需要选择角色的种族。

```js
var race = {
  protoss: 'protoss', // 神族
  terran: 'terran', // 人族
  zerg: 'zerg' // 虫族
}
function createRole(type){
  if(type === race.protoss){创建神族角色}
  else if(type === race.terran){创建人族角色}
  else if(type === race.zerg){创建虫族角色}
}
```

那么用户选择种族后，就需要调用 createRole 来创建角色：
```js
// 传入字符串
createRole('zerg') 
// 或者传入变量
createRole(race.zerg)
```

一般传入字符串被认为是不好的做法，所以使用 createRole(race.zerg) 的更多。

如果使用 createRole(race.zerg)，那么聪明的读者会发现一个问题：race.protoss、race.terran、race.zerg 的值为多少并不重要。

改为如下写法，对 createRole(race.zerg) 毫无影响：

```js
var race = {
  protoss: 'askdjaslkfjas;lfkjas;flkj', // 神族
  terran: ';lkfalksjfl;askjfsfal;skfj', // 人族
  zerg: 'qwieqwoirqwoiruoiwqoisrqwroiu' // 虫族
}
```

也就是说：**race.zerg 的值是多少无所谓，只要它的值跟 race.protoss 和 race.terran 的值不一样就行。**

Symbol 的用途就是如此：Symbol 可以创建**一个独一无二的值**（但并不是字符串）。

用 Symbol 来改写上面的 race：
```js
var race = {
  protoss: Symbol(),
  terran: Symbol(),
  zerg: Symbol()
}

race.protoss !== race.terran // true
race.protoss !== race.zerg // true
```

你也可以给每个 Symbol 起一个名字：
```js
var race = {
  protoss: Symbol('protoss'),
  terran: Symbol('terran'),
  zerg: Symbol('zerg')
}
```

不过这个名字跟 Symbol 的值并没有关系，你可以认为这个名字就是个注释。

如下代码可以证明 Symbol 的名字与值无关：

```js
var a1 = Symbol('a')
var a2 = Symbol('a')
a1 !== a2 // true
```

## 数据类型转换

```js
var a = '123';
var b = parseInt(a);
//从左向右,遇到非数字字符串时停止转换.把后面的字符串扔掉
var a = '123abc123';
var b = parseInt(a); // b = 123
//如果首字符时非数字字符,则转换为NaN(Not a number)
parseFloat(a)
Number(a)//只要字符串中有非数字字符,直接转为NaN,既可以转整数,又可以转为小数
```

## 最大值最小值

- 无穷大: Infinity

- 无穷小: -Infinity

## 隐式类型转化

没有主动的对值进行类型转换,但是因为某些操作,导致JS自动进行转换,这样的转换是在JS内部自动实现的

具体参考ECMAScripts官方文档

```js
alert(1 + '10') //输出110--->将1转换为'1',字符串拼接
alert(true + false) //输出为1--->1+0
alert(null - 10) //输出为-10--->0-10
```

## switch语句

### 穿透

```js
switch(value){
    case 1:
    case 2:
    case 3:
    case 4:
        console.log("输入小于4")
    case 5:
    case 6:
        console.log("输入大于4")
}
```

### 范围分支

```js
switch(true){
    case score >= 90:
        console.log("优秀")
    case score >= 60:
        console.log("良好")
    case score < 60:
        console.log("不及格")
    default:
        console.log("输入有误")   
}
```

## Function

### 函数声明

```js
function fn(){
    alert(2)
}
```

### 函数表达式

```js
var fn = function(){
    alert(2)
}
```

### 函数调用
```js
//事件调用函数
document.onclick = fn;
//非事件调用函数
fn()
```

## 不定参数

```js
// 关键字 arguments
function sum(){
    for(var i=0; i<arguments.length; i++){
        console.log(arguments[i])
    }
}

sum(1,2,3,4,5)
```

## 获取行间/非行间样式

```js
var wid = document.querySelector('div'); //行间样式
/*
getComputedStyle(obj)[attr]  非行间样式
*/
var obj = document.querySelector('div');
var wid2 = getComputedStyle(obj)['width'];

/*
兼容性问题 IE8 不支持
处理兼容性问题:
obj.currentStyle.attr
*/
function getStyle(obj, attr){
    if(obj.currentStyle){
        return obj.currentStyle[attr]
    }
    else{	//非 IE 浏览器
        return getConputedStyle(obj)[attr]
    }
}
/* 改为三目运算符 */
return obj.currentrStyle ? obj.currentStyle[attr] : getConputedStyle(obj)[attr]
 // return case ? true : false
```

## 预解析

```js
/* 
声明变量
声明函数
*/
var a;	//undefined 变量声明 会放在最上面
console.log(a);
a = 12;

fn()	//123
function fn(){	//函数声明提前
    console.log("123")
}

fn() // undefined
var fn = function(){	//只将fn声明提前,函数体没有预编译
    console.log(123)
}
```

## 变量定义

```js
function fn(){
    a = 5;
}
fn()
console.log(a)	//a为全局变量

//在函数中的变量,如果外层有定义,则指向定义
//如果外层没有定义,则默认作为window变量保存
```

## 闭包

```js
/*
一个函数内嵌套子函数,子函数可以调用父函数的局部作用域
*/
function fn(){
    var a = 1;
    function fn2(){
        a++;
        console.log(a)
    }
    fn2()
}
fn()
/*
一个函数调用另一个函数的局部变量
*/

var list = document.querySelectorAll('li');
for(var i=0; i<list.length; i++){
    (function(index){
        console.log(index)
        lis[index].onclick = function(){
            alert(index);
        }
    })(i) 	//闭包函数调用
}
```

## 函数下的this指向

```js
function fn(){
    console.log(this) //window
}

document.onclick = function(){
    console.log(this)	//#document
}

//改变函数下的this指向
/*
call 可以改变this指向
call(obj,传递的值)
obj: 要指向的对象
传递的值: 传参
*/
fn.call(document.body, 12)

/*
apply(指向的位置, 传参数组)
*/
fn.apply(document, [1,2,3])
```

## bind

```js
/*
bind 本身不会执行函数,会返回一个新的函数体
*/
var newfn = fn.bind(document);
newfn()	//直接执行newfn,就是在执行绑定的函数

/*
call和apply的区别
 - bind 是一个方法,执行完毕,会返回一个函数
 - call/apply 会立即调用函数本身
 - bind 返回的是一个新的函数,改变的是新函数下的this指向
*/

newfn(1,2,3) //传递参数
```

 ## 定时器

```js
//每隔一段时间或延迟一段时间,执行一段指定的代码
/*
延迟定时器: setTimeout
间隔定时器: setInterval
*/

/*
setTimeout(code, time, )
 - code: 延迟一段时间后 需要执行的代码
 	- function
 	- 字符串(不建议)
 - time: 延迟的事件(单位ms	1s=1000ms)
 - parma1...parmaN 作为参数传入code中(IE9存在兼容问题)
*/

setTimeout(function(){
    console.log(123)
}, 1000)

setTimeout("alert(123)", 1000)

setTimeout(function(x,y){
    console.log(x+y)
}, 1000, 1, 2) //x=1, y=2


```

### 消除延迟代码

```js
/*
每个定时器都会有一个返回值,这个返回值就是id(正整数)
*/
var time1 = 0;
var time2 = 0;
time1 = setTimeout(function(){
    console.log(123)
}, 1000)
time2 = setTimeout(function(){
    console.log(456)
}, 1000)
console.log(time1) //1
console.log(time2) //2

clearTimeout(time1) //清除定时器 参数为id
```

## 间隔定时器

```js
/*
setInterval 参数和setTimeout相同
移动的div
*/
var btn = document.querySelectorAll('button');
var div = document.querySelectorAll('div');
var timer = null;
var num = 0;

btn[0].onlcick = function(){
    div.style.background = 'url(img/download.gif)';
    // 设定初始位置
    num = parseInt(getStyle(div).left)
    timer = setInterval(function(){
        num++;
        div.style.left = num+'px';
    }, 16)
}

// 行间样式 兼容性
function getStyle(obj){
    if(obj.currentStyle){
        return currentStyle
    }
    else{
        return getComputedStyle(obj)
    }
}

//清除间隔定时器
clearInterval(timer)
```

### this指向性问题

```js
/*
定时器是js本身定义好的函数,因此this指向的是window
因为定时器都是通过window调用的
*/
var div = document.querySelectorAll('div');
var _this = null;
div.onclick = function(){
    _this = this;
    window.setTimeout(function(){
        _this.style.cssText = 'width:300px; height:300px';
    })
}
```

### 作用域

```js
for(var i=0; i<5; i++){
    setTimeout(function(){
        console.log(i) // 5 5 5 5 5 
    })
}
/*
在局部作用域中没有找到i,因此到全局作用域中寻找,此时for循环结束,i=5
setTimeout函数执行会比for循环慢
*/
```

## 延迟定时器中函数调用

### 存在问题

```js
setTimeout(fn("hello"), 1000)

function fn(txt){
    console.log(txt)
}
/*
此时没有产生延迟效果,因为函数直接激活
*/
```

### 解决方案1-匿名函数

```js
/* 解决方案1: 使用匿名函数包装 */
setTimeout(function(){
    fn("hello")
}, 1000)
```

### bind改变作用域指向

**bind方法 小于IE9不能使用**

```js
/* 解决方案2: bind*/
var obj = {
    age: 18,
    getAge: function(){
        console.log(this.age)
    }
}
obj.getAge() //18
var getNewAge = obj.getAge;
getNewAge()	//undefined   因为函数中的this指向的是window

var getNewAge = obj.getAge.bind(obj);
getNewAge() //18   bind改变this指向

/* bind传递参数 */
bind(obj, auguments...)
```

### 解决方案2-bind

```js
setTimeout(fn.bind(null, "hello"), 1000)
```

## 导航栏实现

```js
var nav = document.querySelector('#nav');
//一级导航
var aLi = nav.querySelectorAll('li');
//二级导航
var subNav = nav.querySelectorAll('.float_layer'); 
var timer = null;

for(var i=0; i<aLi.length; i++){
    aLi[i].onmouseover = function(){
        //console.log(this.index)
        clearTimeout(timer)
        for(var j=0; j<subNav.length; j++){
            subNav[j].style.display = 'none';
        }
        subNav[this.index].style.display = 'block';
    }
    aLi[i].onmouseout = function(){
        var _this = this;	//定义指向
        timer = setTimeout(function(){
            subNav[_this.index].style.display = 'none';
        }, 1000)
    }
}
```






