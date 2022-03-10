# SSM初识



突然准备接一个项目的运维了...技术栈是SSM</br>
五一这几天一直在搬家,今天开始搞吧</br>
Java学学学
<!--more-->

# IDEA设置

## 设置IDE编码方式

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/1.png">

## 显示行号和空格

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/2.png">

## 设置最大空行数量

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/3.png">

## 添加main函数快捷键

psvm

## 调试过程快捷键

f7逐条执行,进入方法中的方法内部

f8逐条执行,跳过方法中方法

f9直接运行到下一个断点

## IDEA开启时不打开上一次的项目

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/4.png">

## 添加第三方插件

WEB-INF下增加文件夹lib

将jar包添加到lib文件夹下

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/5.png">

## 配置生效

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/6.png">

## 创建Servlet

### 配置IDEA

点击项目中iml类型的文件

增加代码

```xml
<sourceRoots>
            <root url="file://$MODULE_DIR$/src" />
            <root url="file://$MODULE_DIR$/src/main/java" />
</sourceRoots>
```

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/7.png">

### 新建Servlet

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/8.png">

### 添加Tomcat依赖

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/9.png">

### 编写返回报文

```java
public class HelloServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doPost(request, response);
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.getWriter().write("hello,tronwei");
    }
}
```

## 插件运行Tomcat

```xml
<plugins>
	<plugin>
    	<groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
        	<port>9990</port>
            <path>/</path>
        </configuration>
    </plugin>
</plugins>
```

可以选择执行命令运行tomcat

```bash
tomcat7:run
```

## 创建聚合工程

电商系统,其中有`聚合后台子模块`和`聚合前台子模块`

前后台系统中都有`dao`,`service`,`web`三层结构

# Spring mvc框架

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/10.png">

M是Model（中文名称是数据模型），一般是实体类，可以被多个视图共用；V是View（中文名称是视图），可以是JSP、ASP等动态页面；C是Controller（中文名称是控制器），用于接收视图发起的请求或返回已处理的内容到视图。程序员要使用MVC开发Web应用程序，就必须遵守MVC规定的设计模式。

视图就是JSP页面，JSP页面发送请求到Controll类，也就是MVC的控制器，Controll类收到视图发出的请求后，会对请求进行分发，并调用相关的业务类对请求进行处理；POJO类（实体类，也就是MVC的数据模型）是业务类要处理的数据对象，处理的数据对象可以由控制器返回到视图。

# SSM框架概念

SSM是三个开发框架的集成，第一个字母S是指Spring开发框架，第二个字母S是指Spring  MVC开发框架，第三个字母M是指Mybatis数据库开发框架。实际上Spring  MVC是Spring框架的扩展，是属于Spring框架的一部分，因此应该是两个开发框架的集成。SSM现在已经成为主流的Web应用程序开发框架

# Spring Boot

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/11.png">

# Tomcat

## 文件的结构目录

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/12.png">

## 应用部署目录

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/13.png">

## 框架机制

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/14.png">

## 原理：

Tomcat主要组件

- 服务器Server

- 服务Service

- 连接器Connector

- 容器Container

**连接器Connector和容器Container是Tomcat的核心**

一个Container容器和一个或多个Connector组合在一起，加上其他一些支持的组件共同组成一个Service服务，有了Service服务便可以对外提供能力了，但是Service服务的生存需要一个环境，这个环境便是Server，Server组件为Service服务的正常使用提供了生存环境，Server组件可以同时管理一个或多个Service服务

## 两大组件：

### Connector

  一个Connecter将在某个指定的端口上侦听客户请求，接收浏览器的发过来的 tcp 连接请求，创建一个 Request 和 Response 对象分别用于和请求端交换数据，然后会产生一个线程来处理这个请求并把产生的  Request 和 Response  对象传给处理Engine(Container中的一部分)，从Engine出获得响应并返回客户。 Tomcat中有两个经典的Connector，一个直接侦听来自Browser的HTTP请求，另外一个来自其他的WebServer请求

HTTP/1.1 Connector在端口8080处侦听来自客户Browser的HTTP请求，AJP/1.3 Connector在端口8009处侦听其他Web  Server（其他的HTTP服务器）的Servlet/JSP请求。

**Connector 最重要的功能就是接收连接请求然后分配线程让  Container 来处理这个请求，所以这必然是多线程的，多线程的处理是 Connector 设计的核心**

### Container

Container是容器的父接口，该容器的设计用的是典型的责任链的设计模式，它由四个自容器组件构成，分别是Engine、Host、Context、Wrapper。这四个组件是负责关系，存在包含关系。通常一个Servlet  class对应一个Wrapper，如果有多个Servlet定义多个Wrapper，如果有多个Wrapper就要定义一个更高的Container，如Context。 

Context 还可以定义在父容器 Host 中，Host 不是必须的，但是要运行 war 程序，就必须要 Host，因为 war 中必有 web.xml  文件，这个文件的解析就需要 Host 了，如果要有多个 Host 就要定义一个 top 容器 Engine 了。而 Engine  没有父容器了，一个 Engine 代表一个完整的 Servlet 引擎。

- Engine 容器 Engine 容器比较简单，它只定义了一些基本的关联关系
- Host 容器 Host 是 Engine 的子容器，一个 Host 在 Engine  中代表一个虚拟主机，这个虚拟主机的作用就是运行多个应用，它负责安装和展开这些应用，并且标识这个应用以便能够区分它们。它的子容器通常是  Context，它除了关联子容器外，还有就是保存一个主机应该有的信息。
- Context 容器 Context 代表  Servlet 的 Context，它具备了 Servlet 运行的基本环境，理论上只要有 Context 就能运行 Servlet 了。简单的 Tomcat 可以没有 Engine 和 Host。Context 最重要的功能就是管理它里面的 Servlet 实例，Servlet 实例在 Context 中是以 Wrapper 出现的，还有一点就是 Context 如何才能找到正确的 Servlet 来执行它呢？ Tomcat5 以前是通过一个 Mapper 类来管理的，Tomcat5 以后这个功能被移到了 request  中，在前面的时序图中就可以发现获取子容器都是通过 request 来分配的。
- Wrapper 容器 Wrapper 代表一个  Servlet，它负责管理一个 Servlet，包括的 Servlet 的装载、初始化、执行以及资源回收。Wrapper  是最底层的容器，它没有子容器了，所以调用它的 addChild 将会报错。 Wrapper 的实现类是  StandardWrapper，StandardWrapper 还实现了拥有一个 Servlet 初始化信息的  ServletConfig，由此看出 StandardWrapper 将直接和 Servlet 的各种信息打交道。

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/15.png">

Catalina负责管理Server,而server表示整个服务器,server下面有多个服务service,每个服务都包含多个连接器组件conductor和一个容器组件container

一个连接器组件主要包含着一个connector和多个processor,connector负责监听客户请求,然后交给processor来寻找container处理

和连接器关联的容器一般指的是Engine,Host,Context,Wrapper也都是容器,这里是层层嵌套的关系,最底层的Wrapper包裹着Servlet,最后请求都会传递到Servlet来执行

在Tomcat启动的时候,会初始化一个Catalina实例

Catalina负责的是解析Tomcat的配置文件,以此来创建服务器server组件,并根据命令对其进行管理

Server服务器表示整个Catalina Servlet容器以及其他组件,负责组装并启动Servlet引擎,Tomcat连接器,server通过实现lifecycle接口,提供了一种优雅的启动和关闭整个系统的方式

service服务是server内部的组件,一个server包含多个service,他将若干个connector组件绑定在一个Container(Engine)上

Connector连接器,处理与用户端的通信,他负责接收客户请求,然后转向相关的容器进行处理,然后向客户返回相应的结果,连接器包含四个重要的部分

- 连接器类HttpConnector

- 支撑类HttpProcessor

- 请求类HttpRequest

- 响应类HttpResponse


Container容器负责处理用户的Servlet请求,并返回对象给Web用户的模块

Tomcat中定义了四种容器

1. Engine表示整个Catalina的servlet引擎,一个引擎包含若干个Host
2. Host表示一个虚拟主机,一个主机包含若干个Context
3. Context表示一个Web应用,一个上下文包含若干个Wrapper
4. Wrapper表示一个独立的servlet,包装器作为容器的最底层,不能包含子容器

# Servlet

## 基本概念

处理请求和发送响应的过程是由一种叫做Servlet的程序来完成的，并且Servlet是为了解决实现动态页面而衍生的东西。理解这个的前提是了解一些http协议的东西，并且知道B/S模式(浏览器/服务器)。

B/S:浏览器/服务器。 浏览器通过网址来访问服务器，比如访问百度，在浏览器中输入www.baidu.com，这个时候浏览器就会显示百度的首页

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/16.png">

## tomcat和servlet的关系

Tomcat 是Web应用服务器,是一个Servlet/JSP容器. Tomcat 作为Servlet容器,负责处理客户请求,把请求传送给Servlet,并将Servlet的响应传送回给客户.而Servlet是一种运行在支持Java语言的服务器上的组件. Servlet最常见的用途是扩展Java Web服务器功能,提供非常安全的,可移植的,易于使用的CGI替代品.

从http协议中的请求和响应可以得知，浏览器发出的请求是一个请求文本，而浏览器接收到的也应该是一个响应文本。但是在上面这个图中，并不知道是如何转变的，只知道浏览器发送过来的请求也就是request，我们响应回去的就用response。忽略了其中的细节，现在就来探究一下。

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/ssm-1/17.png">

- Tomcat将http请求文本接收并解析，然后封装成HttpServletRequest类型的request对象，所有的HTTP头数据读可以通过request对象调用对应的方法查询到。

- Tomcat同时会要响应的信息封装为HttpServletResponse类型的response对象，通过设置response属性就可以控制要输出到浏览器的内容，然后将response交给tomcat，tomcat就会将其变成响应文本的格式发送给浏览器

Java Servlet API  是Servlet容器(tomcat)和servlet之间的接口，它定义了serlvet的各种方法，还定义了Servlet容器传送给Servlet的对象类，其中最重要的就是ServletRequest和ServletResponse。所以说我们在编写servlet时，需要实现Servlet接口，按照其规范进行操作。
