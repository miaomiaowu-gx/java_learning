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

分析：超链接的 url 中传递的参数名 name 与 Controller 中 testRequestParam 方法的参数名 username 不一致，无法赋值。使用 `@RequestParam` 注解 “ 标明匹配关系 ”。


#### 2.2 RequestBody 注解



#### 2.3 PathVariable 注解




#### 2.4 HiddentHttpMethodFilter 过滤器



#### 2.5 RequestHeader 注解



#### 2.6 CookieValue 注解


#### 2.7 ModelAttribute 注解 



#### 2.8 SessionAttributes 注解


