## 第四节 响应数据和结果视图

#### 4.1 搭建环境

参考第一节 SpringMVC 入门环境搭建

1. 在 pom.xml 导入相关坐标。

2. 在 web.xml 中配置前端控制器以及解决中文乱码的过滤器。

3. 在 src->main 下创建 java 文件夹、resources 文件夹，并将两者设置为对应的 Root 目录。

4. 在 resources 文件夹下创建 springmvc.xml 配置文件，配在配置文件中开启注解扫描、配置视图解析器对象并开启 SpringMVC 框架注解的支持。

5. 在 webapp 文件夹下删除原 index.jsp，然后创建新的 index.jsp 文件。

6. 在 webapp 文件夹下创建目录 pages，并在其下创建 success.jsp 页面。


#### 4.2 返回值分类

实体类

```java
public class User implements Serializable{
    private String username;
    private String password;
    private Integer age;
    ...
}    
```


##### 4.2.1 字符串

**超链接页面**

```html
<a href="user/testString" >testString</a>
```

**控制器**

```java
@Controller
@RequestMapping("/user")
public class UserController {
    /**
     * 返回String
     */
    @RequestMapping("/testString")
    public String testString(Model model){
        System.out.println("testString方法执行了...");
        // 模拟从数据库中查询出User对象
        User user = new User();
        user.setUsername("美美");
        user.setPassword("123");
        user.setAge(30);
        // model对象：将user存储到request域中
        model.addAttribute("user",user);
        return "success";
    }
}
```

**在跳转页面 success.jsp 中读取数据，根据返回的字符串值 success 跳转到指定页面。**

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h3>执行成功</h3>
    ${user.username}
    ${user.password}
</body>
</html>
```

##### 4.2.2 void 

**超链接页面**

```html
<a href="user/testVoid">testVoid</a>
```

**控制器**


* **转发**

```java
@Controller
@RequestMapping("/user")
public class UserController {
    /**
     * 请求转发一次请求，不用编写项目的名称
     */
    @RequestMapping("/testVoid")
    public void testVoid(HttpServletRequest request, HttpServletResponse response) throws Exception {
        System.out.println("testVoid方法执行了...");
        // 编写请求转发的程序
        request.getRequestDispatcher("/WEB-INF/pages/success.jsp").forward(request,response);
        return;
    }
}
```

* **重定向**

```java
@Controller
@RequestMapping("/user")
public class UserController {
    /**
     * 重定向是两次请求，需要提供项目名称
     */
    @RequestMapping("/testVoid")
    public void testVoid(HttpServletRequest request, HttpServletResponse response) throws Exception {
        System.out.println("testVoid方法执行了...");
        // 重定向：index.jsp在webapp文件夹下，该方式不能直接访问WEB-INF下的页面
        response.sendRedirect(request.getContextPath()+"/index.jsp");
        return;
    }
}
```


* **直接响应**

```java
@Controller
@RequestMapping("/user")
public class UserController {

    /**
     * 是void
     */
    @RequestMapping("/testVoid")
    public void testVoid(HttpServletRequest request, HttpServletResponse response) throws Exception {
        System.out.println("testVoid方法执行了...");
        // 设置中文乱码
        response.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=UTF-8");
        // 直接会进行响应
        response.getWriter().print("你好");
        return;
    }
}
```


##### 4.2.3 ModelAndView 


**超链接页面**

```html
<a href="user/testModelAndView" >testModelAndView</a>
```

**控制器**

```java
@Controller
@RequestMapping("/user")
public class UserController {
    @RequestMapping("/testModelAndView")
    public ModelAndView testModelAndView(){
        // 创建ModelAndView对象（由SpringMvc直接提供）
        ModelAndView mv = new ModelAndView();
        System.out.println("testModelAndView方法执行了...");
        // 模拟从数据库中查询出User对象
        User user = new User();
        user.setUsername("小凤");
        user.setPassword("456");
        user.setAge(30);

        // 把user对象存储到mv对象中，也会把user对象存入到request对象
        mv.addObject("user",user);
        // 跳转到哪个页面
        mv.setViewName("success");
        return mv;
    }
}
```

#### 4.3 forward 和 redirect 关键字进行页面跳转

用关键字，不能使用视图解析器，因此路径需要自己写正确！


**超链接页面**

```html
<a href="user/testForwardOrRedirect">testForwardOrRedirect</a>
```        


**控制器**

* **forward 转发**

```java
@Controller
@RequestMapping("/user")
public class UserController {
    System.out.println("testForwardOrRedirect方法执行了...");
    // 请求的转发
    return "forward:/WEB-INF/pages/success.jsp";
}
```

* **redirect 重定向**

```java
@Controller
@RequestMapping("/user")
public class UserController {
    System.out.println("testForwardOrRedirect方法执行了...");
    // 重定向：index.jsp 在根目录下，可访问
    return "redirect:/index.jsp";
}
```

#### 4.4 ResponseBody 响应 json 数据


##### 4.4.1 过滤静态资源

【问题描述】

在浏览器访问如下页面

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script src="js/jquery.min.js"></script>
    <script>
        // 页面加载，绑定单击事件
        $(function(){
            $("#btn").click(function(){
                alert("hello btn");
            });
        });
    </script>
</head>
<body>
    <button id="btn">发送ajax的请求</button>
</body>
</html>
```

点击按钮 “发送ajax的请求”，并不会弹出 alert 信息。

【分析原因】

```xml
<!--配置前端控制器-->
<servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

如上显示，web.xml 中配置的前端控制器拦截了对 JQuery 的访问 (js/jquery.min.js)。

【解决方案】

在 springmvc.xml 配置文件中设置静态资源不拦截，`**` 表示所有文件。

```xml
<!--前端控制器，哪些静态资源不拦截-->
<mvc:resources location="/css/" mapping="/css/**"/>
<mvc:resources location="/images/" mapping="/images/**"/>
<mvc:resources location="/js/" mapping="/js/**"/>
```

其中目录结构如下：

```markdown
src
 |___main
   |___webapp
     |___css(文件夹内含有.css文件)
     |___images(文件夹内含有各种图像文件)
     |___js(文件夹内含有各种.js文件)
     |___WEB-INF
     | |___web.xml(前端控制器、中文乱码过滤器)
     | |___pages
     |   |___success.jsp
     |___index.jsp(默认的主页面)    
```        


##### 4.4.2 发送 ajax 的请求并响应 json 格式数据  

🍒 1 导入 jackson 坐标，将接收到的 json 数据封装到 JavaBean 中。

```xml
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.9.0</version>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-core</artifactId>
  <version>2.9.0</version>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-annotations</artifactId>
  <version>2.9.0</version>
</dependency>
```

🍒 2 **访问页面**

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script src="js/jquery.min.js"></script>
    <script>
        // 页面加载，绑定单击事件
        $(function(){
            $("#btn").click(function(){
                // 发送ajax请求
                $.ajax({
                    // 编写json格式，设置属性和值
                    url:"user/testAjax",
                    contentType:"application/json;charset=UTF-8",
                    data:'{"username":"hehe","password":"123","age":30}',
                    dataType:"json",
                    type:"post",
                    success:function(data){
                        // data服务器端响应的json的数据，进行解析
                        alert(data);
                        alert(data.username);
                        alert(data.password);
                        alert(data.age);
                    }
                });
            });
        });
    </script>
</head>
<body>
    <button id="btn">发送ajax的请求</button>
</body>
</html>
```

🍒 3 **控制器**

```java
@Controller
@RequestMapping("/user")
public class UserController {
    /**
     * 模拟异步请求响应
     * @RequestBody：把接收到的 json 串封装为 User对象（jackson 包）
     * @ResponseBody：将返回的  User对象转换为 json 串，并响应
     */
    @RequestMapping("/testAjax")
    public @ResponseBody User testAjax(@RequestBody User user){
        System.out.println("testAjax方法执行了...");
        // 客户端发送ajax的请求，传的是json字符串，后端把json字符串封装到user对象中
        System.out.println(user);
        // 做响应，模拟查询数据库
        user.setUsername("haha");
        user.setAge(40);
        // 做响应
        return user;
    }
}
```










