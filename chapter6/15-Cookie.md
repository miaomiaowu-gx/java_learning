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



### 15.2.2 Coolie

### 15.3 JSP 