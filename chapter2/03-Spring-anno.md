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

```md
文件结构
java.com.itheima
|___dao
| |___impl
| | |___AccountDaoIpml.java   
| |___IAccountDao.java 
|
|___service
| |___impl
| | |___AccountServiceIpml.java   
| |___IAccountService.java 
|
|___ui
  |___Client
```
其中 AccountServiceImpl 实现如下：
```java
public class AccountServiceImpl implements IAccountService {
    private IAccountDao accountDao;
    public AccountServiceImpl(){
        System.out.println("对象创建了！");
    }
    public void  saveAccount(){
        accountDao.saveAccount();
    }
}
```


#### 3.1.3 注解配置

```
曾经 XML 的配置：
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl" scope=""  init-method="" destroy-method="">
    <property name=""  value="" / ref=""></property>
</bean>
```

##### 用于创建对象的 Component 注解

作用：与 XML 配置文件中编写一个 `<bean>` 标签实现的功能是一样的。

* Component:
  * 作用：用于把当前类对象存入 Spring 容器中
  * 属性：value，用于指定 bean 的 id。当不写时，它的默认值是当前类名，且首字母改小写。
* Controller：一般用在表现层
* Service：一般用在业务层
* Repository：一般用在持久层
* 以上三个注解他们的作用和属性与 Component 是一模一样的（可以互相通用）。他们三个是 Spring 框架为使用者提供明确的三层使用的注解。


