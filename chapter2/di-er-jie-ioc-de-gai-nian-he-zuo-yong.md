## 第二节 使用 spring 的 IOC 解决程序耦合

### 2.1 IoC 的概念和作用

<img src="./img2/02-ioc.png" width=400>

**控制反转**（Inversion of Control，缩写为IoC），是面向对象编程中的一种设计原则，可以用来减低计算机代码之间的耦合度。其中最常见的方式叫做**依赖注入**（Dependency Injection，简称DI），还有一种方式叫**依赖查找**（Dependency Lookup）。通过控制反转，对象在被创建的时候，由一个调控系统内所有对象的外界实体将其所依赖的对象的引用传递给它。也可以说，依赖被注入到对象中。

### 2.2 使用 Spring 的 IOC 解决程序耦合

案例：账户的业务层和持久层的依赖关系解决。

#### 2.2.1 前期准备

**创建业务层接口和实现类** 

```java
public interface IAccountService {
	//保存账户（此处只是模拟，并不是真的要保存）
	void saveAccount();
}
```

```java
//账户的业务层实现类
public class AccountServiceImpl implements IAccountService {
	private IAccountDao accountDao = new AccountDaoImpl();//此处的依赖关系有待解决
	@Override
	public void saveAccount() {
		accountDao.saveAccount();
	}
}
```

**创建持久层接口和实现类** 

```java
public interface IAccountDao {
	//保存账户
	void saveAccount();
}
```

```java
//账户的持久层实现类
public class AccountDaoImpl implements IAccountDao {
	@Override
	public void saveAccount() {
		System.out.println("保存了账户");
	}
}
```

#### 2.2.2 基于 XML 的配置（入门案例） 

1 创建 Maven 工程，添加 Spring 坐标:

```xml
<packaging>jar</packaging>

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.0.2.RELEASE</version>
    </dependency>
</dependencies>
```

2 在 src->main->resources 下，创建文件 `bean.xml`（名称可以任意取），添加约束。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

让 Spring 管理资源，在配置文件 `bean.xml` 中配置 service 和 dao 

```xml
<!--把对象的创建交给 Spring 来管理-->
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"></bean>
<bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl"></bean>
<!-- bean 标签：用于配置让 Spring 创建对象，并且存入 ioc 容器之中
	id 属性：对象的唯一标识。
	class 属性：指定要创建对象的全限定类名
-->
```

3 测试配置是否成功 

```java
public class Client {
    //获取Spring的Ioc核心容器，并根据id获取对象
    public static void main(String[] args) {
        //1.获取核心容器对象
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.根据id获取Bean对象（两种方式）
        IAccountService as  = (IAccountService)ac.getBean("accountService");
        IAccountDao adao = ac.getBean("accountDao",IAccountDao.class); //根据字节码转换类型
        System.out.println(as);
        System.out.println(adao);
        as.saveAccount();
    }
}
//输出：
//com.itheima.service.impl.AccountServiceImpl@6f45df59
//com.itheima.dao.impl.AccountDaoImpl@38e79ae3
//保存了账户
```

> spring5 版本是用 jdk8 编写的，所以要求 jdk 版本是 8 及以上，同时 tomcat 的版本要求 8.5 及以上。

 
【丢失部分待补充 17之后】  
   
     
#### 2.3 Spring 注入

