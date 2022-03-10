# 依赖注入



在知乎上看到了一篇讲 依赖注入 非常好的文章,在这里分享一下
<!--more-->


## 原始代码

```python
class iPhone6 extends Iphone
{
    function read($user="某人"){
        echo $user."打开了知乎然后编了一个故事 \n";
    }
    function play($user="某人"){
        echo $user."打开了王者农药并送起了人头 \n";
    }
    function grab($user="某人"){
        echo $user."开始抢红包却只抢不发 \n";
    }
}

class Ming extends Person
{
    private $_name;
    private $_age;
    public function  __construct(){
        $this->_name = '小明';
        $this->_age = 26;
    }
    function read(){
        //……  省略若干代码
        (new iPhone6())->read($this->_name); //逛知乎
    }
    function  play(){
        //……  省略若干代码
        (new iPhone6())->play($this->_name);//玩农药
    }
    function  grab(){
        //……  省略若干代码
        (new iPhone6())->grab($this->_name);//抢红包
    }
}
```

输出:

```python
$ming = new Ming();  //小明起床
$ming->read();
$ming->play();
$ming->grab();

# 小明打开了知乎然后编了一个故事 
# 小明打开了王者农药并送起了人头 
# 小明开始抢红包却只抢不发
```

但是这里出现了一个问题，如果小明更换手机，那么需要将所有的`iphone6`更换为`iphone13`

此时为`过度耦合`

那么如何`解耦`呢，这个时候就需要转换控制权，将手机的控制权不放在小明的手里，`控制反转`

## new code

```python
class Ming extends Person
{
    private $_name;
    private $_age;
    private $_phone; //将手机作为自己的成员变量
    public function  __construct($phone){
        $this->_name = '小明';
        $this->_age = 26;
        $this->_phone = $phone;
        echo "小明起床了 \n";
    }
    function read(){
        //……  省略若干代码
        $this->_phone->read($this->_name); //逛知乎
    }
    function  play(){
        //……  省略若干代码
        $this->_phone->play($this->_name);//玩农药
    }
    function  grab(){
        //……  省略若干代码
        $this->_phone->grab($this->_name);//抢红包
    }
}

$phone = new IphoneX(); //创建一个iphoneX的实例
if($phone->isBroken()){//如果iphone不可用，则使用旧版手机
    $phone = new Iphone6();
}
$ming = new Ming($phone);//小明不用关心是什么手机，他只要玩就行了。
$ming->read();
$ming->play();
$ming->grab();
```

## 总结

如果一个类A 的功能实现需要借助于类B，那么就称类B是类A的**依赖**，如果在类A的内部去实例化类B，那么两者之间会出现较高的**耦合**，一旦类B出现了问题，类A也需要进行改造，如果这样的情况较多，每个类之间都有很多依赖，那么就会出现牵一发而动全身的情况，**程序会极难维护**，并且很容易出现问题。要解决这个问题，就要把A类对B类的控制权抽离出来，交给一个第三方去做，把控制权反转给第三方，就称作**控制反转（IOC Inversion Of Control）**。**控制反转是一种思想**，是能够解决问题的一种可能的结果，而**依赖注入（Dependency Injection）**就是其最典型的实现方法。由第三方（**我们称作IOC容器**）来控制依赖，把他通过**构造函数、属性或者工厂模式**等方法，注入到类A内，这样就极大程度的对类A和类B进行了**解耦**。
