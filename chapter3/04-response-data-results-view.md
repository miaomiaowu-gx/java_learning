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

#### 4.3 forward 转发

用关键字，不能使用视图解析器，因此路径需要自己写正确！


**超链接页面**


**控制器**

```java
@Controller
@RequestMapping("/user")
public class UserController {

}
```





#### 4.4 Redirect 重定向

**超链接页面**


**控制器**

```java
@Controller
@RequestMapping("/user")
public class UserController {

}
```





#### 4.5 ResponseBody 响应 json 数据