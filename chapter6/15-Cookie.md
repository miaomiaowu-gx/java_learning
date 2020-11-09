## 第十五节 Coolie

### 15.1 会话技术

1. 会话：一次会话中包含多次请求和响应。
   * 一次会话：浏览器第一次给服务器资源发送请求，会话建立，直到有一方断开为止。
2. 功能：在一次会话的范围内的多次请求间，共享数据
3. 方式：
	1. 客户端会话技术：Cookie
	2. 服务器端会话技术：Session

### 15.2 Coolie

概念：客户端会话技术，将数据保存到客户端

### 15.2.1 Coolie 快速入门

步骤：

1. 创建 Cookie 对象，绑定数据： `new Cookie(String name, String value)` 

2. 发送 Cookie 对象：`response.addCookie(Cookie cookie)` 

3. 获取 Cookie，拿到数据：`Cookie[]  request.getCookies()`

```java
/**
 * Cookie快速入门
 */

@WebServlet("/cookieDemo1")
public class CookieDemo1 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.创建Cookie对象
        Cookie c = new Cookie("msg","hello");
        //2.发送Cookie
        response.addCookie(c);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

```java
@WebServlet("/cookieDemo2")
public class CookieDemo2 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       //3. 获取Cookie
        Cookie[] cs = request.getCookies();
        //获取数据，遍历Cookies
        if(cs != null){
            for (Cookie c : cs) {
                String name = c.getName();
                String value = c.getValue();
                System.out.println(name+":"+value);
            }
        }
    }
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

当使用同一个浏览器分别访问 `/cookieDemo1` 与 `/cookieDemo2` 时，会输出 msg:hello。



### 15.2.2 Coolie 原理

基于响应头 Set-Cookie 和请求头 Cookie 实现

<img src="./img6/78-Cookie-priciple.png" width=400>

### 15.2.3 Cookie 细节

1）一次可不可以发送多个 cookie?
可以，可以创建多个 Cookie 对象，使用 response 调用多次 addCookie 方法发送 cookie 即可。

```java
//1.创建Cookie对象
Cookie c1 = new Cookie("msg","hello");
Cookie c2 = new Cookie("name","zhangsan");
//2.发送Cookie
response.addCookie(c1);
response.addCookie(c2);
```

2）cookie 在浏览器中保存多长时间？

1. 默认情况下，当浏览器关闭后，Cookie 数据被销毁。（即 Cookie 数据保存在浏览器内存中）

2. 持久化存储：`setMaxAge(int seconds)`
   * 正数：将 Cookie 数据写到硬盘的文件中。持久化存储。并指定 cookie 存活时间，时间到后，cookie 文件自动失效。
   * 负数：默认值，浏览器关闭后，Cookie 数据被销毁。
   * 零：删除 cookie 信息。
   
```java
//1.创建Cookie对象
Cookie c1 = new Cookie("msg","setMaxAge");
//2.设置cookie的存活时间
//将cookie持久化到硬盘，30秒后会自动删除cookie文件
c1.setMaxAge(30); 
//c1.setMaxAge(0); //删除Cookie
//3.发送Cookie
response.addCookie(c1);
```

3）cookie 能不能存中文？

* 在 tomcat 8 之前 cookie 中不能直接存储中文数据。需要将中文数据转码，一般采用URL编码(%+两个十六进制，如 %E3)。

* 在 tomcat 8 之后，cookie 支持中文数据。特殊字符还是不支持，建议使用URL编码存储，URL解码解析

4）cookie 共享问题？




### 15.2.4 Cookie 

### 15.3 JSP 