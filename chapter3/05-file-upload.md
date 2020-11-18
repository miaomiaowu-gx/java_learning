## 第五节 SpringMVC 实现文件上传


#### 5.1 上传原理分析

##### 5.1.1 文件上传的必要前提

1. form 表单的 enctype 取值必须是：`multipart/form-data`
(默认值是：`application/x-www-form-urlencoded`)
  * `enctype`：是表单请求正文的类型。

2. `method` 属性取值必须是 `Post`。

3. 提供一个文件选择域 `<input type=”file” />`


##### 5.1.2 文件上传的原理分析

当 form 表单的 enctype 取值不是默认值后，`request.getParameter()` 将失效。

`enctype="application/x-www-form-urlencoded"` 时，form 表单的正文内容是：`key=value&key=value&key=value`。

当 form 表单的 enctype 取值为 Mutilpart/form-data 时，请求正文内容就变成：






##### 5.1.3 借助第三方组件实现文件上传

使用 Commons-fileupload 组件实现文件上传，需要导入该组件相应的支撑 jar 包：** Commons-fileupload 和 commons-io**。commons-io 不属于文件上传组件的开发 jar 文件，但 Commons-fileupload 组件从 1.1 版本开始，它工作时需要 commons-io 包的支持。


#### 5.2 搭建环境



 
  
#### 5.3 

    
#### 5.4 

#### 5.5 

#### 5.6   

  
      
#### 5.7                         
     