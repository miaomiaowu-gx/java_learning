## 第三节 SpringBoot 配置文件

### 3.1 SpringBoot 配置文件类型

#### 3.1.1 SpringBoot 配置文件类型和作用

&emsp;&emsp;SpringBoot 是基于约定的，所以很多配置都有默认值，但如果想使用自己的配置替换默认配置的话，就可以使用 application.properties 或者 application.yml（application.yaml）进行配置。

&emsp;&emsp;**SpringBoot 默认会从 Resources 目录下加载 application.properties 或 application.yml（application.yaml）文件。**

&emsp;&emsp;其中，application.properties 文件是键值对类型的文件，之前一直在使用，所以此处不在对 properties 文件的格式进行阐述。除了 properties 文件外，SpringBoot 还可以使用 yml 文件进行配置，下面对 yml 文件进行讲解。

#### 3.1.2 application.yml 配置文件

##### 3.1.2.1 yml 配置文件简介

&emsp;&emsp;YML 文件格式是 YAML (YAML Aint Markup Language) 编写的文件格式，YAML 是一种直观的能够被电脑识别的**数据序列化格式**，并且容易被人类阅读，容易和脚本语言交互，可以被支持 YAML 库的不同的编程语言程序导入，比如： C/C++, Ruby, Python, Java, Perl, C#, PHP 等。YML 文件是以数据为核心的，比传统的 xml 方式更加简洁。


&emsp;&emsp;**YML 文件的扩展名可以使用 `.yml` 或者 `.yaml`。**

##### 3.1.2.2 yml 配置文件的语法

配置文件加载顺序

```xml
<resource>
    <filtering>true</filtering>
    <directory>${basedir}/src/main/resources</directory>
    <includes>
        <include>**/application*.yml</include>
        <include>**/application*.yaml</include>
        <include>**/application*.properties</include>
    </includes>
</resource>
```

后加载的会覆盖之前加载的，因此 properties 文件优先级最高，yaml 优先级次之，yml优先级最低。


###### 3.1.2.2.1 配置普通数据

- 语法： key: value

- 示例代码：

  ```yaml
  name: haohao
  ```

- 注意：**value 之前有一个空格**

###### 3.1.2.2.2 配置对象数据

- 语法：缩进一致，代表同一级 

   ```yaml
  ​ key: 
  ​	key1: value1 # [冒号与 value 间留有一个空格]
  ​	key2: value2

  ​ # 或者：

  ​ key: {key1: value1,key2: value2}
   ```
  
- 示例代码：

- ```yaml
  person:
    name: haohao
    age: 31
    addr: beijing

  #或者

  person: {name: haohao,age: 31,addr: beijing}
  # 冒号后仍然需要留有空格！
  ```

- 注意：key1 前面的空格个数不限定，在 yml 语法中，相同缩进代表同一个级别

###### 3.1.2.2.2 配置 Map 数据 

同上面的对象写法

###### 3.1.2.2.3 配置数组（List、Set）数据

- 语法： 
  
  ```yaml
  ​key: 
  ​	- value1
  ​	- value2
    
  # 或者：

  ​key: [value1,value2]
  ```

- 示例代码：

  ```yaml
  #集合中的元素是普通字符串
  city:
    - beijing
    - tianjin
    - shanghai
    - chongqing
    
  #或者

  city: [beijing,tianjin,shanghai,chongqing]

  #集合中的元素是对象形式
  student:
    - name: zhangsan
      age: 18
      score: 100
    - name: lisi
      age: 28
      score: 88
    - name: wangwu
      age: 38
      score: 90
      
  #或者
  student: [{name: zhangsan,age: 18,score: 100},{name: lisi,age: 28,score: 88}]
  ```

- 注意：value1 与之间的 - 之间存在一个空格

#### 3.1.3 SpringBoot 配置信息的查询

上面提及过，SpringBoot 的配置文件，主要的目的就是对配置信息进行修改的，但在配置时的 key 从哪里去查询呢？可以查阅 SpringBoot 的官方文档

文档 URL：https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#common-application-properties

常用的配置摘抄如下：

```properties
# QUARTZ SCHEDULER (QuartzProperties)
spring.quartz.jdbc.initialize-schema=embedded # Database schema initialization mode.
spring.quartz.jdbc.schema=classpath:org/quartz/impl/jdbcjobstore/tables_@@platform@@.sql # Path to the SQL file to use to initialize the database schema.
spring.quartz.job-store-type=memory # Quartz job store type.
spring.quartz.properties.*= # Additional Quartz Scheduler properties.

# ----------------------------------------
# WEB PROPERTIES
# ----------------------------------------

# EMBEDDED SERVER CONFIGURATION (ServerProperties)
server.port=8080 # Server HTTP port.
server.servlet.context-path= # Context path of the application.
server.servlet.path=/ # Path of the main dispatcher servlet.

# HTTP encoding (HttpEncodingProperties)
spring.http.encoding.charset=UTF-8 # Charset of HTTP requests and responses. Added to the "Content-Type" header if not set explicitly.

# JACKSON (JacksonProperties)
spring.jackson.date-format= # Date format string or a fully-qualified date format class name. For instance, `yyyy-MM-dd HH:mm:ss`.

# SPRING MVC (WebMvcProperties)
spring.mvc.servlet.load-on-startup=-1 # Load on startup priority of the dispatcher servlet.
spring.mvc.static-path-pattern=/** # Path pattern used for static resources.
spring.mvc.view.prefix= # Spring MVC view prefix.
spring.mvc.view.suffix= # Spring MVC view suffix.

# DATASOURCE (DataSourceAutoConfiguration & DataSourceProperties)
spring.datasource.driver-class-name= # Fully qualified name of the JDBC driver. Auto-detected based on the URL by default.
spring.datasource.password= # Login password of the database.
spring.datasource.url= # JDBC URL of the database.
spring.datasource.username= # Login username of the database.

# JEST (Elasticsearch HTTP client) (JestProperties)
spring.elasticsearch.jest.password= # Login password.
spring.elasticsearch.jest.proxy.host= # Proxy host the HTTP client should use.
spring.elasticsearch.jest.proxy.port= # Proxy port the HTTP client should use.
spring.elasticsearch.jest.read-timeout=3s # Read timeout.
spring.elasticsearch.jest.username= # Login username.

```

可以通过配置 application.poperties 或者 application.yml 来修改 SpringBoot 的默认配置。

例如：

application.properties 文件

```properties
server.port=8888
server.servlet.context-path=/demo
```

application.yml 文件

```yaml
server:
  port: 8888
  servlet:
    context-path: /demo
```



### 3.2 配置文件与配置类的属性映射方式

#### 3.2.1 使用注解 @Value 映射

可以通过 `@Value` 注解将配置文件中的值映射到一个 Spring 管理的 Bean 的字段上。

例如：

application.properties 配置如下：

```properties
person.name=zhangsan
person.age=18
```

或者，application.yml 配置如下：

```yaml
person:
  name: zhangsan
  age: 18
```

实体 Bean 代码如下：

```java
@Controller
public class QuickStartController {

    @Value("${person.name}")
    private String name;
    @Value("${person.age}")
    private Integer age;

    @RequestMapping("/quick")
    @ResponseBody
    public String quick(){
        return "springboot 访问成功! name="+name+",age="+age;
    }

}
```

浏览器访问地址：http://localhost:8080/quick。



#### 3.2.2 使用注解 @ConfigurationProperties 映射

通过注解 `@ConfigurationProperties(prefix="配置文件中的key的前缀")` 可以将配置文件中的配置自动与实体进行映射。

application.properties 配置如下：

```properties
person.name=zhangsan
person.age=18
```

或者，application.yml 配置如下：

```yaml
person:
  name: zhangsan
  age: 18
```

实体 Bean 代码如下：

```java
@Controller
@ConfigurationProperties(prefix = "person")
public class QuickStartController {

    //必须要有 set 方法
    private String name;
    private Integer age;

    @RequestMapping("/quick")
    @ResponseBody
    public String quick(){
        return "springboot 访问成功! name="+name+",age="+age;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setAge(Integer age) {
        this.age = age;
    }
}
```

浏览器访问地址：http://localhost:8080/quick。

**注意：使用 `@ConfigurationProperties` 方式可以进行配置文件与实体字段的自动映射，但需要字段必须提供 set 方法才可以，而使用 `@Value` 注解修饰的字段不需要提供 set 方法。**

#### 3.2.3 configuration-processor 作用

可以为 `@ConfigurationProperties` 注解在 pom.xml 中配置

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

作用：在配置 yaml 文件时，会根据实体类有提示。
