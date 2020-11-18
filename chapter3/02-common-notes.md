## 第二节 常用注解

#### 2.1 RequestParam 注解

**作用**：把请求中指定名称的参数给控制器中的形参赋值。

**属性**：
* value：请求参数中的名称。
* required：请求参数中是否必须提供此参数。默认值：true。表示必须提供，如果不提供将报错。

**使用示例**：

* **jsp 中示例代码**

```html
<a href="anno/testRequestParam?name=哈哈">RequestParam</a>
```

* **控制器中示例代码**


```java
@Controller
@RequestMapping("/anno")
public class AnnoController {
    @RequestMapping("/testRequestParam")
    public String testRequestParam(@RequestParam(name="name") String username){
        System.out.println("执行了...");
        System.out.println(username);
        return "success";
    }
}
```

**分析**：超链接的 url 中传递的参数名 name 与 Controller 中 testRequestParam 方法的参数名 username 不一致，无法赋值。使用 `@RequestParam` 注解 “ 标明匹配关系 ”。


#### 2.2 RequestBody 注解

**作用**：用于获取请求体内容。直接使用得到是 key=value&key=value... 结构的数据。get 请求方式不适用，因为 get 请求方式没有请求体。

**属性**：required：是否必须有请求体。默认值是 true。当取值为 true 时，get 请求方式会报错。如果取值为 false，get 请求得到是 null。

**使用示例**：

* **jsp 中示例代码**

```html
<form action="anno/testRequestBody" method="post">
    用户姓名：<input type="text" name="username" /><br/>
    用户年龄：<input type="text" name="age" /><br/>
    <input type="submit" value="提交" />
</form>
```

* **控制器中示例代码**


```java
@Controller
@RequestMapping("/anno")
public class AnnoController {
    /**
     * 获取到请求体的内容
     */
    @RequestMapping("/testRequestBody")
    public String testRequestBody(@RequestBody String body){
        System.out.println("执行了...");
        System.out.println(body);
        return "success";
    }
}
```

**分析**：如果不添加 @RequestBody 注解，默认去寻找名为 body 的参数。


#### 2.3 PathVariable 注解

**作用**：用于绑定 url 中的占位符。例如：请求 url 中 `/delete/{id}`，这个 {id} 就是 url 占位符。url 支持占位符是 spring3.0 之后加入的。是 springmvc 支持 rest 风格 URL 的一个重要标志。

**属性**：

* value：用于指定 url 中占位符名称。
* required：是否必须提供占位符。

**使用示例**：

* **jsp 中示例代码**

```html
<a href="anno/testPathVariable/10">testPathVariable</a>
```

* **控制器中示例代码**


```java
@Controller
@RequestMapping("/anno")
public class AnnoController {
    @RequestMapping(value="/testPathVariable/{sid}", method=RequestMethod.GET)
    public String testPathVariable(@PathVariable(name="sid") String id){
        System.out.println("执行了...");
        System.out.println(id);
        return "success";
    }
}
```

**分析**：注意 Url 的输入方式 `/{sid}` 对应着 `/10`，而不是 `sid=10` 方式。

**该注解的作用**：

<img src="./img3/05-restfule.png" width=600>


#### 2.4 HiddentHttpMethodFilter 过滤器(了解)


**作用**：由于浏览器 form 表单只支持 GET 与 POST 请求，而 DELETE、 PUT 等 method 并不支持， Spring3.0 添加了一个过滤器，**可以将浏览器请求改为指定的请求方式，发送给控制器方法**，使得支持 GET、 POST、 PUT 与 DELETE 请求。

**使用方法**：

* 第一步：在 web.xml 中配置该过滤器。

* 第二步：请求方式必须使用 post 请求。

* 第三步：按照要求提供 `_method` 请求参数，该参数的取值就是需要的请求方式。


**使用示例**：

* **jsp 中示例代码**

```html
<!-- 更新 -->
<form action="springmvc/testRestPUT/1" method="post">
    用户名称： <input type="text" name="username"><br/>
    <!-- 定义隐藏域传输要设置的请求方式 -->
    <input type="hidden" name="_method" value="PUT">
    <input type="submit" value="更新">
</form>
```

* **控制器中示例代码**

```java
// put 请求：更新
@RequestMapping(value="/testRestPUT/{id}",method=RequestMethod.PUT)
public String testRestfulURLPUT(@PathVariable("id")Integer id,User user){
    System.out.println("rest put "+id+","+user);
    return "success";
}
```


#### 2.5 RequestHeader 注解



**作用**：用于获取请求消息头。

**属性**：

* `value`：提供消息头名称
* `required`：是否必须有此消息头

**使用示例**：

* **jsp 中示例代码**

```html
<a href="anno/testRequestHeader">RequestHeader</a>
```

* **控制器中示例代码**

```java
@Controller
@RequestMapping("/anno")
public class AnnoController {
    /**
     * 获取请求头的值
     */
    @RequestMapping(value="/testRequestHeader")
    public String testRequestHeader(@RequestHeader(value="Accept") String header){
        System.out.println("执行了...");
        System.out.println(header);
        return "success";
    }
}
```

**分析**：在实际开发中一般不怎么用。


#### 2.6 CookieValue 注解


**作用**：用于把指定 cookie 名称的值传入控制器方法参数。

**属性**：

* `value`：指定 cookie 的名称。
* `required`：是否必须有此 cookie。

**使用示例**：


* **jsp 中示例代码**

```html
<a href="anno/testCookieValue">CookieValue</a>
```


* **控制器中示例代码**

```java
@Controller
@RequestMapping("/anno")
public class AnnoController {
    /**
     * 获取Cookie的值
     */
    @RequestMapping(value="/testCookieValue")
    public String testCookieValue(@CookieValue(value="JSESSIONID") String cookieValue){
        System.out.println("执行了...");
        System.out.println(cookieValue);
        return "success";
    }
}
```


#### 2.7 ModelAttribute 注解 

**作用**：该注解是 SpringMVC4.3 版本以后新加入的。它可以用于修饰方法和参数。

* 出现在方法上，表示当前方法会在控制器的方法执行之前，先执行。它可以修饰没有返回值的方法，也可以修饰有具体返回值的方法。

* 出现在参数上，获取指定的数据给参数赋值。

**属性**：

 * `value`：用于获取数据的 key。 key 可以是 POJO 的属性名称，也可以是 map 结构的 key。


**应用场景**：当表单提交数据不是完整的实体类数据时，保证没有提交数据的字段使用数据库对象原来的数据。

* 例如：用户含有三个属性 uname(String)、age(Integer)、date(Date)，而表单提交只包含 uname 与 age 属性。

**使用示例**：

* **User 实体类**

```java
public class User implements Serializable{
    private String uname;
    private Integer age;
    private Date date;
    ...
}

* **jsp 中示例代码**

```html
<form action="anno/testModelAttribute" method="post">
    用户姓名：<input type="text" name="uname" /><br/>
    用户年龄：<input type="text" name="age" /><br/>
    <input type="submit" value="提交" />
</form>
```

* **控制器：有返回值（方式一）**

```java
@Controller
@RequestMapping("/anno")
public class AnnoController {

    /**
     * ModelAttribute注解
     */
    @RequestMapping(value="/testModelAttribute")
    public String testModelAttribute(User user){
        System.out.println("testModelAttribute执行了...");
        System.out.println(user);
        return "success";
    }

    @ModelAttribute
    public User showUser(String uname){
        System.out.println("showUser执行了...");
        // 通过用户查询数据库（模拟）
        User user = new User();
        user.setUname(uname);
        user.setAge(20);
        user.setDate(new Date());
        return user;
    }
}
```


* **控制器：没有返回值（方式二）**

```java
@Controller
@RequestMapping("/anno")
public class AnnoController {
    /**
     * ModelAttribute注解
     */
    @RequestMapping(value="/testModelAttribute")
    public String testModelAttribute(@ModelAttribute("abc") User user){
        System.out.println("testModelAttribute执行了...");
        System.out.println(user);
        return "success";
    }

    // 会先执行该方法
    @ModelAttribute
    public void showUser(String uname, Map<String,User> map){
        System.out.println("showUser执行了...");
        // 通过用户查询数据库（模拟）
        User user = new User();
        user.setUname(uname);
        user.setAge(20);
        user.setDate(new Date());
        // 将查询到的结果放到 map 中
        map.put("abc",user);
    }
}
```


#### 2.8 SessionAttributes 注解


**作用**：用于多次执行控制器方法间的参数共享。

**属性**：

* `value`：用于指定存入的属性名称。
* `type`：用于指定存入的数据类型。

**使用示例**：

* **jsp 中示例代码**

```html
<a href="anno/testSessionAttributes">testSessionAttributes</a>
<a href="anno/getSessionAttributes">getSessionAttributes</a>
<a href="anno/delSessionAttributes">delSessionAttributes</a>
```

* **控制器中示例代码**

```java
@Controller
@RequestMapping("/anno")
@SessionAttributes(value={"msg"})   // 把msg=美美存入到session域对中，该注解只能作用在类上
public class AnnoController {
    /**
     * SessionAttributes的注解
     */
    @RequestMapping(value="/testSessionAttributes")
    public String testSessionAttributes(Model model){
        System.out.println("testSessionAttributes...");
        // 底层会存储到request域对象中
        model.addAttribute("msg","美美");
        return "success";
    }

    /**
     * 获取值
     */
    @RequestMapping(value="/getSessionAttributes")
    public String getSessionAttributes(ModelMap modelMap){
        System.out.println("getSessionAttributes...");
        String msg = (String) modelMap.get("msg");
        System.out.println(msg);
        return "success";
    }

    /**
     * 清除
     */
    @RequestMapping(value="/delSessionAttributes")
    public String delSessionAttributes(SessionStatus status){
        System.out.println("getSessionAttributes...");
        status.setComplete();
        return "success";
    }
}
```


* **在 success.jsp 页面访问**

要设置 isELIgnored 页面属性

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h3>入门成功</h3>
    ${ msg }
    ${sessionScope}
</body>
</html>
```


