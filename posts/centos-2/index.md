# curl常用命令


curl常用命令
<!--more-->

# 命令格式

```bash
curl [option] [url]
```

## 保存网页

```bash
curl -o baidu.html http://www.baidu.com
```

## 使用代理

```bash
curl -x 192.168.100.100:1080 http://www.baidu.com
```

## 发送POST请求

```bash
curl -X POST http://www.baidu.com
```

## 只显示 HTTP 头

```bash
curl -I http://www.codebelief.com 
```

## 自定义User-Agent

```bash
curl -A “Mozilla/5.0 (Android; Mobile; rv:35.0) Gecko/35.0 Firefox/35.0” http://www.baidu.com 
```

## 自定义 header

```bash
curl -H “Referer: www.example.com” -H “User-Agent: Custom-User-Agent” http://www.baidu.com 
```

## POST 请求

POST 请求，-d 用于指定发送的数据，-X 用于指定发送数据的方式：

```bash
curl -d “userName=tom&passwd=123456” -X POST http://www.example.com/login
```

在使用 -d 的情况下，如果省略 -X，则默认为 POST 方式：

```bash
curl -d “userName=tom&passwd=123456” http://www.example.com/login
```

常见参数

```bash
-A/--user-agent <string>              设置用户代理发送给服务器
-b/--cookie <name=string/file>    cookie字符串或文件读取位置
-c/--cookie-jar <file>                    操作结束后把cookie写入到这个文件中
-C/--continue-at <offset>            断点续转
-D/--dump-header <file>              把header信息写入到该文件中
-e/--referer                                  来源网址
-f/--fail                                          连接失败时不显示http错误
-o/--output                                  把输出写到该文件中
-O/--remote-name                      把输出写到该文件中，保留远程文件的文件名
-r/--range <range>                      检索来自HTTP/1.1或FTP服务器字节范围
-s/--silent                                    静音模式。不输出任何东西
-T/--upload-file <file>                  上传文件
-u/--user <user[:password]>      设置服务器的用户和密码
-w/--write-out [format]                什么输出完成后
-x/--proxy <host[:port]>              在给定的端口上使用HTTP代理
-#/--progress-bar                        进度条显示当前的传送状态
-L, --location                               自动跳转
```
