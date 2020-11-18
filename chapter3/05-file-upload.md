## 第五节 SpringMVC 实现文件上传


#### 5.1 上传原理分析

##### 5.1.1 文件上传的必要前提

1. form 表单的 enctype 取值必须是：`multipart/form-data`
(默认值是：`application/x-www-form-urlencoded`)
   * `enctype`：是表单请求正文的类型。

2. `method` 属性取值必须是 `Post`。

3. 提供一个文件选择域 `<input type=”file” />`


##### 5.1.2 文件上传的原理分析

1. 当 form 表单的 enctype 取值不是默认值后，`request.getParameter()` 将失效。

2. `enctype="application/x-www-form-urlencoded"` 时，form 表单的正文内容是：`key=value&key=value&key=value`。

3. 当 form 表单的 enctype 取值为 `Mutilpart/form-data` 时，请求正文内容就变成：

```markdown
(每一部分都是 MIME 类型描述的正文)
-----------------------------7de1a433602ac (分界符)
Content-Disposition: form-data; name="userName" (协议头)
aaa (协议的正文)
-----------------------------7de1a433602ac
Content-Disposition: form-data; name="file";
filename="C:\Users\zhy\Desktop\fileupload_demofile\b.txt"
Content-Type: text/plain              (协议的类型 - MIME 类型)
                                      (空行)
bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
-----------------------------7de1a433602ac--
```


##### 5.1.3 借助第三方组件实现文件上传

&emsp;&emsp;使用 Commons-fileupload 组件实现文件上传，需要导入该组件相应的支撑 jar 包：**Commons-fileupload 和 commons-io**。commons-io 不属于文件上传组件的开发 jar 文件，但 Commons-fileupload 组件从 1.1 版本开始，它工作时需要 commons-io 包的支持。


#### 5.2 搭建环境


参考第一节 SpringMVC 入门环境搭建

1. 在 pom.xml 导入相关坐标。

2. 在 web.xml 中配置前端控制器以及解决中文乱码的过滤器。

3. 在 src->main 下创建 java 文件夹、resources 文件夹，并将两者设置为对应的 Root 目录。

4. 在 resources 文件夹下创建 springmvc.xml 配置文件，配在配置文件中开启注解扫描、配置视图解析器对象并开启 SpringMVC 框架注解的支持。

5. 在 webapp 文件夹下删除原 index.jsp，然后创建新的 index.jsp 文件。

6. 在 webapp 文件夹下创建目录 pages，并在其下创建 success.jsp 页面。

需要多导入几个相关坐标

```xml
<dependency>
  <groupId>commons-fileupload</groupId>
  <artifactId>commons-fileupload</artifactId>
  <version>1.3.1</version>
</dependency>
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.4</version>
</dependency>

<dependency>
  <groupId>com.sun.jersey</groupId>
  <artifactId>jersey-core</artifactId>
  <version>1.18.1</version>
</dependency>
<dependency>
  <groupId>com.sun.jersey</groupId>
  <artifactId>jersey-client</artifactId>
  <version>1.18.1</version>
</dependency>
```


 

#### 5.3 


#### 5.4 

#### 5.5 

#### 5.6   


​      

#### 5.7                         
