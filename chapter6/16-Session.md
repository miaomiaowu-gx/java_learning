## 第十六节 Session


1）概念：服务器端会话技术，在一次会话的多次请求间共享数据，将数据保存在服务器端的对象中。HttpSession。

2）快速入门

1. 获取 HttpSession 对象：`HttpSession session = request.getSession();`

2. 使用 HttpSession 对象：
  * `Object getAttribute(String name)`
  * `void setAttribute(String name, Object value)`
  * `void removeAttribute(String name)`  
	
3）原理：Session 的实现是依赖于 Cookie 的。

<img src="./img6/80-Session-principle.png" width=700>

### 16.1 Session 细节

**1）当客户端关闭后，服务器不关闭，两次获取 session 是否为同一个？**
​
* 默认情况下。不是同一个。
​
* 如果需要相同，则可以创建 Cookie，键为 JSESSIONID，设置最大存活时间，让 cookie 持久化保存。

```java
HttpSession session = request.getSession();
Cookie c = new Cookie("JSESSIONID",session.getId());
c.setMaxAge(60*60);
response.addCookie(c);
```

**2）客户端不关闭，服务器关闭后，两次获取的 session 是同一个吗？**

不是同一个，但是要确保数据不丢失。tomcat 自动完成以下工作：

* session 的钝化：在服务器正常关闭之前，将 session 对象系列化到硬盘上。

* session的活化：在服务器启动后，将 session 文件转化为内存中的 session 对象并删除对应的 session 文件。


### 16.2 案例 验证码







