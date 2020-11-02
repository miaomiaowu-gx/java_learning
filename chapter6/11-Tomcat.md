## 第一十节 Tomcat

### 11.1 web 相关概念回顾

1 软件架构
* C/S：客户端/服务器端
* B/S：浏览器/服务器端
	
2 资源分类
* **静态资源**：所有用户访问后，得到的结果都是一样的，称为静态资源。静态资源可以直接被浏览器解析，如：html、css、JavaScript。
* **动态资源**：每个用户访问相同资源后，得到的结果可能不一样，称为动态资源。动态资源被访问后，需要先转换为静态资源，再返回给浏览器，如 servlet/jsp、php、asp。

3 网络通信三要素
* IP：电子设备(计算机)在网络中的唯一标识。
* 端口：应用程序在计算机中的唯一标识。 0~65536
* 传输协议：规定了数据传输的规则。如基础协议：
   * tcp：安全协议，三次握手。 速度稍慢
   * udp：不安全协议。 速度快

### 11.2 web 服务器软件概述

* 服务器：安装了服务器软件的计算机
* 服务器软件：接收用户的请求，处理请求，做出响应
* web 服务器软件：接收用户的请求，处理请求，做出响应。
	* 在 web 服务器软件中，可以部署 web 项目，让用户通过浏览器来访问这些项目
	* web 容器

* 常见的java相关的web服务器软件：
	* webLogic：oracle 公司，大型的 JavaEE 服务器，支持所有的 JavaEE 规范，收费的。
	* webSphere：IBM 公司，大型的 JavaEE 服务器，支持所有的 JavaEE 规范，收费的。
	* JBOSS：JBOSS 公司的，大型的 JavaEE 服务器，支持所有的 JavaEE 规范，收费的。
	* Tomcat：Apache 基金组织，中小型的 JavaEE 服务器，仅仅支持少量的 JavaEE 规范 servlet/jsp。开源的，免费的。

* JavaEE：Java 语言在企业级开发中使用的技术规范的总和，一共规定了 13 项大的规范
	
### 11.3 Tomcat-web 服务器软件

#### 11.3.1 准备工作

1. 下载：http://tomcat.apache.org/
2. 安装：直接解压压缩包即可，注意安装目录建议不要有中文和空格。
3. 卸载：直接删除目录。

#### 11.3.2 启动与关闭

**启动**：bin/startup.bat，双击运行该文件即可。

**访问**：浏览器输入：`http://localhost:8080` 回车访问自己，`http://别人的ip:8080` 访问别人。
			
**可能遇到的问题**：
1 黑窗口一闪而过：
	* 原因：没有正确配置 JAVA_HOME 环境变量
	* 解决方案：正确配置 JAVA_HOME 环境变量
		
2 启动报错：
	* 找到占用的端口号，并且找到对应的进程，杀死该进程。`netstat -ano`
	* 修改自身的端口号：`conf/server.xml`
	* 一般会将 tomcat 的默认端口号修改为 80。80 端口号是 http 协议的默认端口号。好处：在访问时，不用输入端口号。

**关闭**：
	* 正常关闭：
		* 点击：bin/shutdown.bat
		* 在启动窗口输入：ctrl+c
	* 强制关闭：点击启动窗口的 ×

#### 11.3.3 部署项目的三种方式

在 D 盘创建 hello 文件夹，并创建 `hello.html` 文件，文件内容如下：

```html
<font color='red'>Hello Tomcat</font>
```

1 直接将项目(D 盘的 hello 文件夹及内部所有文件)放到 tomcat 安装目录下的 webapps 目录下。访问 url：`http://localhost:8080/hello/hello.html`。
    * `/hello`：项目的访问路径-->虚拟目录
    * 简化部署：当项目很大时，直接传输文件很慢。将项目打成一个 war 包，再将 war 包放置到 webapps 目录下。war 包会自动解压缩。删除项目时，直接删除 war 包即可，解压缩文件会自动跟随删除。
	
2 配置 conf/server.xml 文件
    * 在 `<Host>` 标签体中配置：`<Context docBase="D:\hello" path="/hehe" />`
    * docBase：项目存放的路径
    * path：虚拟目录（访问时用到的目录），如上述配置，访问 url 为：`http://localhost:8080/hehe/hello.html`
    * 必须重启服务器才能生效

3 在 conf\Catalina\localhost 创建任意名称的 xml 文件。在文件中编写
    * <Context docBase="D:\hello" />
    * 虚拟目录：xml 文件的名称(热部署)


#### 11.3.4 java 动态项目的目录结构



#### 11.3.5 tomcat 与 IDEA 集成&创建 web 项目 



