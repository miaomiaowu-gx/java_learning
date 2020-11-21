## 第二十一节 Ajax & Json

### 21.1 Ajax

#### 21.1.1 Ajax 概念

**概念**：ASynchronous JavaScript And XML 异步的 JavaScript 和 XML

**异步和同步**：客户端和服务器端相互通信的基础上

* 客户端必须等待服务器端的响应。在等待的期间客户端不能做其他操作。
* 客户端不需要等待服务器端的响应。在服务器处理请求的过程中，客户端可以进行其他的操作。

**Ajax** 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。通过在后台与服务器进行少量数据交换，Ajax 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。传统的网页（不使用 Ajax）如果需要更新内容，必须重载整个网页页面。
	
**作用**：提升用户的体验


#### 21.1.2 AJAX 实现原生 JS 方式（了解）

【web 网页】

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>
        //定义方法
        function fun() {
            //发送异步请求
            //1.创建核心对象
            var xmlhttp;
            if (window.XMLHttpRequest)
            {// code for IE7+, Firefox, Chrome, Opera, Safari
                xmlhttp=new XMLHttpRequest();
            }
            else
            {// code for IE6, IE5
                xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
            }

            //2. 建立连接
            /*
                参数：
                    1. 请求方式：GET、POST
                        * get方式，请求参数在URL后边拼接。send方法为空参
                        * post方式，请求参数在send方法中定义
                    2. 请求的URL：
                    3. 同步或异步请求：true（异步）或 false（同步）

             */
            xmlhttp.open("GET","ajaxServlet?username=tom",true);

            //3.发送请求
            xmlhttp.send();

            //4.接受并处理来自服务器的响应结果
            //获取方式 ：xmlhttp.responseText
            //什么时候获取？当服务器响应成功后再获取

            //当xmlhttp对象的就绪状态改变时，触发事件onreadystatechange。
            xmlhttp.onreadystatechange=function()
            {
                //判断readyState就绪状态是否为4，判断status响应状态码是否为200
                if (xmlhttp.readyState==4 && xmlhttp.status==200)
                {
                   //获取服务器的响应结果
                    var responseText = xmlhttp.responseText;
                    alert(responseText);
                }
            }

        }
    </script>
</head>
<body>
    <input type="button" value="发送异步请求" onclick="fun();">
    <input>
</body>
</html>
```

【控制器】

```java
package cn.itcast.web.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/ajaxServlet")
public class AjaxServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.获取请求参数
        String username = request.getParameter("username");

       /* //处理业务逻辑。耗时
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }*/
        //2.打印username
        System.out.println(username);

        //3.响应
        response.getWriter().write("hello : " + username);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

#### 21.1.3 AJAX 实现 JQuery 实现方式

 共有三种：

##### 21.1.3.1 🍒 `$.ajax()` ：

**语法**：`$.ajax({键值对});`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/jquery-3.3.1.min.js"></script>
    <script>
        //定义方法
        function  fun() {
            //使用$.ajax()发送异步请求

            $.ajax({
                url:"ajaxServlet1111" , // 请求路径
                type:"POST" , //请求方式
                //data: "username=jack&age=23",//请求参数，方式一
                data:{"username":"jack","age":23}, //请求参数，方式二（推荐使用）
                success:function (data) { // data接收服务器响应的结果值
                    alert(data);
                },//响应成功后的回调函数
                error:function () {
                    alert("出错啦...")
                },//表示如果请求响应出现错误，会执行的回调函数
                dataType:"text"//设置接受到的响应数据的格式
                //最后一个键值对不要写逗号！
            });
        }
    </script>
</head>
<body>
    <input type="button" value="发送异步请求" onclick="fun();">
    <input>
</body>
</html>
```

##### 21.1.3.2 🍒 `$.get()` ：发送 get 请求

* 语法：`$.get(url, [data], [callback], [type])`
* 参数：
   * url：请求路径
   * data：请求参数
   * callback：回调函数
   * type：响应结果的类型

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/jquery-3.3.1.min.js"></script>
    <script>
        //定义方法
        function  fun() {
            $.get("ajaxServlet",{username:"rose"},function (data) {
                alert(data);
            },"text");
        }
    </script>
</head>
<body>
    <input type="button" value="发送异步请求" onclick="fun();">
    <input>
</body>
</html>
```

##### 21.1.3.3 🍒 `$.post()` ：发送 post 请求

* 语法：`$.post(url, [data], [callback], [type])`
* 参数：
   * url：请求路径
   * data：请求参数
   * callback：回调函数
   * type：响应结果的类型

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/jquery-3.3.1.min.js"></script>
    <script>
        //定义方法
        function  fun() {
            $.post("ajaxServlet",{username:"rose"},function (data) {
                alert(data);
            },"text");
        }
    </script>
</head>
<body>
    <input type="button" value="发送异步请求" onclick="fun();">
    <input>
</body>
</html>
```


### 21.2 Json

#### 21.2.1 JSON 概念

概念：JavaScript Object Notation，JavaScript 对象表示法。

```js
Person p = new Person();
p.setName("张三");
p.setAge(23);
p.setGender("男");

var p = {"name":"张三","age":23,"gender":"男"};
```

json 现在多作为**存储和交换文本信息**的语法，进行数据的传输时 JSON 比 XML 更小、更快，更易解析。

#### 21.2.2 语法

**【基本规则】**

1. 数据在名称/值对中：json 数据是由键值对构成的
   * 键用引号 (单双都行) 引起来，也可以不使用引号
   * 值得取值类型：
      * 数字（整数或浮点数）
      * 字符串（在双引号中）
      * 逻辑值（true 或 false）
      * 数组（在方括号中）`{"persons":[{},{}]}`
      * 对象（在花括号中） `{"address":{"province"："陕西"....}}`
      * null
2. 数据由逗号分隔：多个键值对由逗号分隔
3. 花括号保存对象：使用 `{}` 定义 json 格式
4. 方括号保存数组：`[]`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>
        //1. 定义基本格式
        var person = {"name": "张三", age: 23, 'gender': true};
		alert(person);
        
        //2. 嵌套格式   {}———> []
        var persons = {
            "persons": [
                {"name": "张三", "age": 23, "gender": true},
                {"name": "李四", "age": 24, "gender": true},
                {"name": "王五", "age": 25, "gender": false}
                ]
        };
        alert(persons);
        
        //3. 嵌套格式   []———> {}
        var ps = [{"name": "张三", "age": 23, "gender": true},
            {"name": "李四", "age": 24, "gender": true},
            {"name": "王五", "age": 25, "gender": false}];
        alert(ps);
    </script>
</head>
<body>

</body>
</html>
```

**【获取数据】**

1. `json对象.键名`
2. `json对象["键名"]`
3. `数组对象[索引]`

```html
<script>
    //接上面代码
    //1. 获取name的值
    var name1 = person.name;       //张三
    var name2 = person["name"];    //张三
    
    //2. 嵌套格式   {}———> []
    var name3 = persons.persons[2].name;   //王五
    
    //3. 嵌套格式   []———> {}
    var name4 = ps[1].name;  //李四
</script>  
```

【遍历】

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>
        //1.定义基本格式
        var person = {"name": "张三", age: 23, 'gender': true};

        var ps = [{"name": "张三", "age": 23, "gender": true},
            {"name": "李四", "age": 24, "gender": true},
            {"name": "王五", "age": 25, "gender": false}];
        
        //获取person对象中所有的键和值
        //for in 循环
        for(var key in person){
            //person.key：获取不到值，因为相当于 person."name"
            //alert(key + ":" + person.key);
            alert(key+":"+person[key]);
        }

       //获取ps中的所有值
        for (var i = 0; i < ps.length; i++) {
            var p = ps[i];
            for(var key in p){
                alert(key+":"+p[key]);
            }
        }
    </script>
</head>
<body>

</body>
</html>
```


#### 21.2.3 JSON 数据和 Java 对象的相互转换

**JSON 常见解析器**：Jsonlib，Gson，fastjson，jackson。

##### 21.2.3.1 JSON 转为 Java 对象

1. 导入 jackson 的相关 jar 包
2. 创建 Jackson 核心对象 ObjectMapper
3. 调用 ObjectMapper 的相关方法进行转换：`readValue(json字符串数据,Class)`

```java

```

##### 21.2.3.2 Java 对象转换 JSON

1. 导入 jackson 的相关 jar 包
2. 创建 Jackson 核心对象 ObjectMapper
3. 调用 ObjectMapper 的相关方法进行转换

```java

```

1) 转换方法：`writeValue(参数1，obj)`
参数1：
File：将obj对象转换为JSON字符串，并保存到指定的文件中
Writer：将obj对象转换为JSON字符串，并将json数据填充到字符输出流中
OutputStream：将obj对象转换为JSON字符串，并将json数据填充到字节输出流中
* writeValueAsString(obj):将对象转为json字符串

2) 注解：
* `@JsonIgnore`：排除属性。
* `@JsonFormat`：属性值得格式化
* `@JsonFormat(pattern = "yyyy-MM-dd")`

3) 复杂 java 对象转换
* List：数组
* Map：对象格式一致



### 21.3 案例：校验用户名是否存在 









