# Javascript基础 Part-1



常用命令
<!--more-->

## script标签
```js
//建议放在</body>之前,即页面的最后面
<script type="text/javascript"></script>
```
其中type="text/javascript"为html5的新标准,可以省略为了
向下兼容,应该加上

## 获取元素

### 第一种方式

```js
//getElementById
document.getElementById('btn')
/*
通过id获取元素,只有document下有
遇到第一个元素就返回
*/
```

### 第二种方式
>选择器querySelector:  IE8以下不能用
```js
ducument.querySelector('btn')
/*
通过元素选择器,选择器可以通过class,id来选择
#box 表示通过 id 选择
.box 表示通过 class 选择
遇到第一个元素就返回
*/
```

## 寻找父级元素
```html
<div class="wrap">
	<div>父级元素下的子元素</div>
</div>
<script>
document.querySelector('.wrap').querySelector('div')//第一种方式
ducument.querySelector('.wrap div')//第二种方式,包含选择器
</script>
```

## script放置的位置
```js
//等待页面加载完成再执行script脚本(不推荐这样的方法)
windows.onload = function(){
	console.log("窗口加载完成")
}
```

## 给元素绑定点击事件
```html
<button id="btn">按钮</script>
<script>
document.getElementById('btn').onclick = function(){
	ducument.getElementById('btn').style.background = 'red';
}
</script>
```

## 变量存储
```js
var btn = ducument.getElementById("btn");
btn.onclick = function(){
	//do something
}
```

## cssText
```js
 box.onclick = function(){
     box.style.cssText = 'width:300px; height:300px; background:green;'
 }
```

## display属性
| 属性 | 作用 |
| ---- | ---- |
|none|此元素不会被显示|
|block|此元素将显示为块级元素，此元素前后会**带有换行符**|
|inline|默认-此元素会被显示为内联元素，元素前后**没有换行符**|
|inline-block|行内块元素|

## innerHTML
替换掉标签内的全部内容
```js
var list = document.getElementById('list');
list.innerHTML = '<li>' + 'hello' + '<li>'
```

## 控制标签样式
```js
var shit = document.getElementById('list');
shit.style.display = 'block';
```

## js-a标签不执行跳转
```html
<a id = 'next' href="javascript:;">
```

## 获取全部div
```js
var divs = document.querySelectorAll('div') 
//返回值为数组
```

## 多选框开关切换
```js
var btns = document.querySelectorAll('.option a');
//为所有选项增加事件
for(var i=0; i<btns.length; i++){
    btns[i].abc = false //自定义属性
    btns[i].onclick = function(){
        if(this.abc){ //判断当前组件
            this.className = '';
        }
        else{
            this.className = 'checked';
        }
        this.abc = !this.abc;
    }
}
```

## 单选框实现
```js
var btns = document.querySelectAll('.option a');
for(var i=0; i<btns.length; i++){
    btns[i].onclick = function(){
        for(var j=0; j<btns.length; j++){
            btns[j].className = '';
        }
        this.className = 'checked';
    }
}
```

## this指向

在全局情况下调用this,为window

在函数下为调用这个函数的元素或对象

## classList

### 原有class基础上增加
```js
var div = document.querySelector('div');
div.onclick = function(){
    div.classList.add('now');
}
```

### 删除
```js
classList.remove('now');
```

### 判断是否存在某种class
```js
this.classList.contains('abc'); //返回true/false
```

### 切换class
```js
var div = document.querySelector('div');
div.onclick = function(){
    div.classList.toggle('now');
}
// 点一下有now的样式,再点一下样式消除
```

## js中的正则表达式
正则的三个模式:
 - i 执行对大小写不敏感的匹配
 - g 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）
 - m 执行多行匹配

字符串replace正则替换
```js
var txt = str.replace(/microsoft/g,"Runoob");
```

## 判断数组或对象中key是否存在
```js
ary.hasOwnProperty(key);
obj.hasOwnProperty(key);
```

## 在开发环境中添加npm包

```bash
cnpm i electron --save-dev
```

## 生成9*9乘法表

```js
var box = document,getElementById('box');
var colors = ['red', 'blue', 'yellow', 'green',...];
var str = '';
for(var i=1; i<5; i++){
    for(var j=1; j<=i; j++){
        str+= '<div style="background:'+ colors[i-1]+'>'+j+'*'+i+'='+j*i+'</div>'";
    }
    str += '<br/>';
}
box.innerHTML = str;
```

## 背景图移动鼠标显示

```js
var wrap = document,querySelector(.wrap)
for(var i=0; i<10; i++){
    for(var j=0; j<10; j++){
        wrap.innerHTML += '<div style="left:'+60*i+'px; top:'+60*j+'px; background-position:-'50*i'px -'50*j+'px"</div>'
    }
}
var divs = wrap.querySelectAll('div');
for(var i=0; i<divs.length; i++){
    divs[i].onmouseover = function(){
        this.style.backgroundImage = 'url(img.jpg)';
    }
}
```

## 选项卡实现

```html
<div class='wrap'>
    <nav id='nav'>
        <a href="javascript:;" class="active">我的行程</a>
        <a href="javascript:;">消息中心</a>
        <a href="javascript:;">角色管理</a>
        <a href="javascript:;">定时任务补偿</a>
    </nav>
    <div class="boards">
        <div class="active">我的行程</div>
        <div>消息中心</div>
        <div>角色管理</div>
        <div>定时任务补偿</div>
    </div>
</div>
<script>
	var btns = document.querySelectorAll('#nav a');
    var divs = document.querySelectorAll('.board div');
    for(var i=0; i<btns.length; i++){
        btns[i].index = i;
       	btns[i].onclick = function(){
            for(var j=0; j<btns.length; j++){
                btns[j].className = '';
            }
            this.className = 'active';
            
            for(var k=0; k<btns.length; k++){
                divs[k].className = '';
            }
            div[this.index].className = 'active';
        }
    }
</script>
```
