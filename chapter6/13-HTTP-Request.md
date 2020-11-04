## 第十三节 HTTP & Request

### 13.1 HTTP

#### 13.1.1 概念 

概念：Hyper Text Transfer Protocol 超文本传输协议，定义了客户端与服务器端通信时发送数据的格式。

特点：
1. 基于 TCP/IP 的高级协议
2. 默认端口号: 80
3. 基于请求/响应模型的:一次请求对应一次响应
4. 无状态的：每次请求之间相互独立，不能交互数据
	
历史版本：
* 1.0：每一次请求响应都会建立新的连接，即一个网页如果有多个图像资源（以及其他资源）时，会建立不同的连接。
* 1.1：复用连接，一次连接结束后不会马上释放，在一段时间内有数据发送时，仍使用该连接传输数据。对缓存支持的比较好。
* 还有其他区别，此处不在列出

#### 13.1.2 HTTP 请求消息

HTTP 请求消息由一下四个部分组成：

**1）请求行**

```markdown
请求方式 请求url 请求协议/版本
GET /login.html	HTTP/1.1
```

* HTTP 协议有 7 中请求方式，常用的有 2 种
   * GET：
	1. 请求参数在请求行中，在 url 后。
	2. 请求的 url 长度有限制的
	3. 不太安全
   * POST：
	1. 请求参数在请求体中
	2. 请求的 url 长度没有限制的
	3. 相对安全

**2）请求头**：客户端浏览器告诉服务器一些信息

```
//键值对形式
请求头名称: 请求头值
```

* 常见的请求头：
	1. User-Agent：浏览器告诉服务器访问时使用的浏览器版本信息，可以在服务器端获取该头的信息，解决浏览器的兼容性问题。
	2. Referer：告诉服务器，当前请求从哪里来？
		* 作用：1）防盗链 2）统计工作

**3）请求空行**：空行，用于分割 POST 请求的请求头和请求体。

**4）请求体(正文)**：用于封装 POST 请求消息的请求参数，GET 请求的请求体为空。


【一个请求消息】

```
POST /login.html	HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:60.0) Gecko/20100101 Firefox/60.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Referer: http://localhost/login.html
Connection: keep-alive
Upgrade-Insecure-Requests: 1

username=zhangsan	
```

### 13.2 Request



 