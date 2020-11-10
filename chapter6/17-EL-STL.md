## 第十七节 EL & JSTL

### 17.1 JSP

#### 17.1.1 JSP 指令

作用：用于配置 JSP 页面，导入资源文件

格式：`<%@ 指令名称 属性名1=属性值1 属性名2=属性值2 ... %>`

分类：

1）page：配置 JSP 页面的

```html
<%@ page import="java.util.List" %>
<%@ page contentType="text/html;charset=gbk" errorPage="500.jsp"   pageEncoding="GBK" language="java" buffer="16kb" %>
```

* contentType：等同于 `response.setContentType()`
   * 设置响应体的 mime 类型以及字符集。
   * 设置当前 jsp 页面的编码（只能是高级的 IDE 才能生效；如果使用低级工具，则需要设置 pageEncoding 属性指定当前页面的字符集）。

* import：导包。

* errorPage：当前页面发生异常后，会自动跳转到指定的错误页面。

* isErrorPage：标识当前也是是否是错误页面。
   * true：是，可以使用内置对象 exception
   * false：否。默认值。不可以使用内置对象 exception

```html
<%
    String message = exception.getMessage();
    out.print(message);
%>
```

* buffer：缓冲区，默认 8kb。

2）include：包含页面，导入页面的资源文件

* `<%@include file="top.jsp"%>`


3）taglib：导入资源

* `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`

* prefix：前缀，自定义名字

* 使用 `<c:cathch ... >`


#### 17.1.2 注释

1. html 注释：会发送到浏览器端
   * `<!-- -->`：只能注释 html 代码片段
   
2. jsp 注释：推荐使用，不会发送到浏览器端
   * `<%-- --%>`：可以注释所有

```jsp
<!--
    可以注释标签，但不能注释由 <% %>包围的代码
    <h1>hello</h1>
-->
     
<%--
    该注释既可以注释代码也可以注释标签
    <%
        System.out.println("hi~~~~");
    %>
    
    <input>
--%>
```

#### 17.1.3 内置对象

在 jsp 页面中不需要创建，直接使用的对象，一共有9个：

|变量名|真实类型|作用|
|:--:|:--:|:--:|
|pageContext|PageContext|当前页面共享数据，还可以获取其他八个内置对象|

|  表头   | 表头  |
|  ----  | ----  |
| 单元格  | 单元格 |
| 单元格  | 单元格 |


### 17.2 MVC开发模式





### 17.3 EL表达式






### 17.4 JSTL标签






### 17.5 三层架构






