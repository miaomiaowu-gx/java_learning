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


### 14.3 案例

#### 14.3.1 案例1 重定向

重定向：资源跳转的方式

代码实现：

```java
/**
 * 重定向，项目发布在 /day15 下
 */
@WebServlet("/responseDemo1")
public class ResponseDemo1 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //访问 [/responseDemo1]，会自动跳转到 [/responseDemo2] 资源
        
        //方法一:设置状态码与响应头
        //1. 设置状态码为302
        response.setStatus(302);
        //2.设置响应头location
        response.setHeader("location","/day15/responseDemo2");

        //方法二：简单的重定向方法
        response.sendRedirect("/day15/responseDemo2");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request,response);
    }
}
```
|特点|重定向 redirect|转发 forward|
|:--:|:--:|:--:|
|地址栏|发生变化|地址栏路径不变|
|访问|可以访问其他站点(服务器)的资源|只能访问当前服务器下的资源|
|请求|两次请求。不能使用 request 对象来共享数据。|一次请求，可以使用 request 对象来共享数据。|

路径分为相对路径与绝对路径

1）相对路径：通过相对路径不可以确定唯一资源（不推荐使用）
*  不以 `/` 开头，以 `.` 开头路径。如：`./index.html`
* 规则：找到当前资源和目标资源之间的相对位置关系
   * ` ./`：当前目录
   * `../`：后退一级目录







#### 14.3.2 案例2 输出字符数据







#### 14.3.3 案例3 输出字节数据









#### 14.3.4 案例4 验证码







### 14.4 ServletContext






### 14.5 案例 文件下载