## 第三节 Spring 基于注解的 IOC 以及 IoC 的案例

### 3.1 基于注解的 IOC配置

基于注解的 IoC 配置与 xml 配置要实现的功能是一样的，都是要降低程序间的耦合。只是配置的形式不一样。 

#### 3.1.1 准备

```xml
//为 pom.xml 添加坐标
<packaging>jar</packaging>
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.0.2.RELEASE</version>
    </dependency>
</dependencies>

//在 bean.xml 文件中，为 beans 标签添加 context 名称空间和约束中。
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!--告知spring在创建容器时要扫描的包，配置所需要的标签不是在beans的约束中，而是一个名称为
    context名称空间和约束中-->
    <context:component-scan base-package="com.itheima"></context:component-scan>
</beans>
```

#### 3.1.2 文件内容























