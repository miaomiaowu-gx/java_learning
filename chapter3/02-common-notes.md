## 第二节 常用注解

#### 2.1 RequestParam 注解

**作用**：把请求中指定名称的参数给控制器中的形参赋值。

**属性**：
* value：请求参数中的名称。
* required：请求参数中是否必须提供此参数。默认值：true。表示必须提供，如果不提供将报错。

**使用示例**：

```html
<a href="anno/testRequestParam?name=哈哈">RequestParam</a>
```

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

```html
<form action="anno/testRequestBody" method="post">
    用户姓名：<input type="text" name="username" /><br/>
    用户年龄：<input type="text" name="age" /><br/>
    <input type="submit" value="提交" />
</form>
```

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

**作用**：

**属性**：

**使用示例**：

```html
<a href="anno/testPathVariable/10">testPathVariable</a>
```

```java
@Controller
@RequestMapping("/anno")
public class AnnoController {
    @RequestMapping(value="/testPathVariable/{sid}")
    public String testPathVariable(@PathVariable(name="sid") String id){
        System.out.println("执行了...");
        System.out.println(id);
        return "success";
    }
}
```

**分析**：





#### 2.4 HiddentHttpMethodFilter 过滤器

**作用**：

**属性**：

**使用示例**：

**分析**：







#### 2.5 RequestHeader 注解

**作用**：

**属性**：

**使用示例**：

**分析**：







#### 2.6 CookieValue 注解

**作用**：

**属性**：

**使用示例**：

**分析**：






#### 2.7 ModelAttribute 注解 

**作用**：

**属性**：

**使用示例**：

**分析**：







#### 2.8 SessionAttributes 注解

**作用**：

**属性**：

**使用示例**：

**分析**：







