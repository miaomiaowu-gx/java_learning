## 第十四节 Response

### 14.1 HTTP 协议 - 响应消息

响应消息：服务器端发送给客户端的数据，数据格式如下：

**1）响应行**

1. 组成：协议/版本 响应状态码 状态码描述
2. 响应状态码：服务器告诉客户端浏览器本次请求和响应的一个状态，状态码都是 3 位数字，共有 5 类：
* 1xx：服务器就收客户端消息，但没有接受完成，等待一段时间后，发送 1xx 多状态码。
* 2xx：成功。代表：200。
* 3xx：重定向。代表：302(重定向)，304(访问缓存)。
* 4xx：客户端错误。
   * 404（请求路径没有对应的资源）。
   * 405：请求方式没有对应的doXxx方法。
* 5xx：服务器端错误。代表：500(服务器内部出现异常)。
					

**2） 响应头**：由键值对 `头名称：值` 组成

* Content-Type：服务器告诉客户端本次响应体数据格式以及编码格式
* Content-disposition：服务器告诉客户端以什么格式打开响应体数据
   * `in-line`：默认值，在当前页面内打开
   * `attachment;filename=xxx`：以附件形式打开响应体。文件下载

**3）响应空行**

**4）响应体**：传输的数据

**响应字符串格式**

```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Length: 101
Date: Wed, 06 Jun 2018 07:08:42 GMT

<html>
    <head>
    	<title>$Title$</title>
    </head>
    <body>
    	hello , response
    </body>
</html>
```

### 14.2 Response 对象

功能：设置响应消息
1. 设置响应行
    * 格式：HTTP/1.1 200 ok
    * 设置状态码：`setStatus(int sc)` 
2. 设置响应头：`setHeader(String name, String value)` 

3. 设置响应体：
    * 3.1 获取输出流
        * 字符输出流：`PrintWriter getWriter()`
        * 字节输出流：`ServletOutputStream getOutputStream()`
    * 3.2 使用输出流，将数据输出到客户端浏览器

### 14.3 案例1 重定向







### 14.4 案例2 输出字符数据







### 14.5 案例3 输出字节数据









### 14.6 案例4 验证码







### 14.7 ServletContext










### 14.8 案例 文件下载