## 第五节 Spring 中的 AOP

### 5.1 AOP 概念

**概念**：AOP 全称是 Aspect Oriented Programming 即面向切面编程。通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP 是 OOP 的延续，也是 Spring 框架的重要内容，是函数式编程的一种衍生范型。利用 AOP 可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高开发的效率。
* 简单的说，它就是把程序中重复的代码抽取出来，在需要执行的时候，使用动态代理的技术，在不修改源码的基础上，对已有方法进行增强。

**作用**：在程序运行期间，不修改源码对已有方法进行增强。

**优势**：
* 减少重复代码
* 提高开发效率
* 维护方便

**AOP 的实现方式**：使用动态代理技术

### 5.2 Spring 中的 AOP 术语和细节

#### 5.2.1 AOP 相关术语 

**Joinpoint(连接点):**

* 所谓连接点是指那些被拦截到的点。在 spring 中，这些点指的是方法，因为 spring 只支持方法类型的连接点。如账户的业务层接口 IAccountService 中的各个方法。

**Pointcut(切入点):**

* 所谓切入点是指要对哪些 Joinpoint 进行拦截的定义，即被增强的 Joinpoint。
* 所有的切入点都是连接点，但是并不是所有的连接点都一定是切入点。

**Advice(通知/增强):**

* 所谓通知是指拦截到 Joinpoint 之后所要做的事情就是通知。动态代理中 invoke 方法具有拦截功能。在拦截前（method.invoke）的语句是前置通知，拦截后的语句是后置通知。catch 语句块中的是异常通知，finally 语句块中的是最终通知。整个的 invoke 方法在执行就是环绕通知，在环绕通知中有明确的切入点方法调用。
* 通知的类型： 前置通知、后置通知、异常通知、最终通知、环绕通知。

Introduction(引介):
* 引介是一种特殊的通知在不修改类代码的前提下，Introduction 可以在运行期为类动态地添加一些方法或 Field。

Target(目标对象):
* 代理的目标对象，即被代理对象。

Weaving(织入):
* 是指把增强应用到目标对象来创建新的代理对象的过程。
* spring 采用动态代理织入，而 AspectJ 采用编译期织入和类装载期织入。

Proxy（代理） :
* 一个类被 AOP 织入增强后，就产生一个结果代理类。

Aspect(切面):
* 是切入点和通知（引介）的结合。 

#### 5.2.2 学习 Spring 中的 AOP 要明确的事 

**a、开发阶段（程序员）**

* 编写核心业务代码（开发主线）：大部分程序员来做，要求熟悉业务需求。
* 把公用代码抽取出来，制作成通知。（开发阶段最后再做）会 AOP 编程的人员来做。
* 在配置文件中，声明切入点与通知间的关系，即切面。会 AOP 编程的人员来做。

**b、运行阶段（Spring 框架完成）**

* Spring 框架监控切入点方法的执行。一旦监控到切入点方法被运行，使用代理机制，动态创建目标对象的代理对象，根据通知类别，在代理对象的对应位置，将通知对应的功能织入，完成完整的代码逻辑运行。


> 在 Spring 中，框架会根据目标类是否实现了接口来决定采用哪种动态代理的方式。

### 5.3  Spring基于 XML 的 AOP 

#### 5.3.1 编写必要的代码

1 添加坐标

```xml
<packaging>jar</packaging>

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.0.2.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.8.7</version>
    </dependency>
</dependencies>
```

2 在 src->main->java 下创建 `com.itheima.service.IAccountService` 接口

```java
package com.itheima.service;

/**
 * 账户的业务层接口
 */
public interface IAccountService {
    /**
     * 模拟保存账户
     */
    void saveAccount();

    /**
     * 模拟更新账户
     * @param i
     */
    void updateAccount(int i);

    /**
     * 删除账户
     * @return
     */
    int  deleteAccount();
}
```

3 在 src->main->java->com->itheima->service 下创建 `impl.AccountServiceImpl` 类

```java
package com.itheima.service.impl;

import com.itheima.service.IAccountService;

/**
 * 账户的业务层实现类
 */
public class AccountServiceImpl implements IAccountService{

    public void saveAccount() {
        System.out.println("执行了保存");
    }

    public void updateAccount(int i) {
        System.out.println("执行了更新"+i);

    }

    public int deleteAccount() {
        System.out.println("执行了删除");
        return 0;
    }
}
```

4 在 src->main->java->com->itheima 下创建 `utils.Logger` 类

```java
package com.itheima.utils;

/**
 * 用于记录日志的工具类，它里面提供了公共的代码
 */
public class Logger {

    /**
     * 用于打印日志：计划让其在切入点方法执行之前执行（切入点方法就是业务层方法）
     */
    public  void printLog(){
        System.out.println("Logger类中的pringLog方法开始记录日志了。。。");
    }
}
```

#### 5.3.2 Spring 中基于 XML 的 AOP 配置步骤

1、把通知 Bean 也交给 Spring 来管理，即实例中配置 Logger 类。

2、使用 aop:config 标签表明开始 AOP 的配置

3、使用 aop:aspect 标签表明配置切面
* id 属性：是给切面提供一个唯一标识。
* ref 属性：是指定通知类 bean 的 Id。

4、在 aop:aspect 标签的内部使用对应标签来配置通知的类型
* 示例是让 printLog 方法在切入点方法执行之前执行，所以是前置通知 aop:before。
* method 属性：用于指定 Logger 类中哪个方法是前置通知。
* pointcut 属性：用于指定切入点表达式，该表达式的含义指的是对业务层中哪些方法增强。

1 在 src->main->resources 下创建 bean.xml 文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 配置 srping 的 Ioc,把 service 对象配置进来-->
    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"></bean>

    <!-- 配置Logger类 -->
    <bean id="logger" class="com.itheima.utils.Logger"></bean>

    <!--配置AOP-->
    <aop:config>
        <!--配置切面 -->
        <aop:aspect id="logAdvice" ref="logger">
            <!-- 配置通知的类型，并且建立通知方法和切入点方法的关联-->
            <aop:before method="printLog" pointcut="execution(public void com.itheima.service.impl.AccountServiceImpl.saveAccount())"></aop:before>
        </aop:aspect>
    </aop:config>
</beans>
```

2 测试：在 src->test->java 下创建 `com.itheima.test.AOPTest` 测试类

```java
package com.itheima.test;

import com.itheima.service.IAccountService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 测试AOP的配置
 */
public class AOPTest {

    public static void main(String[] args) {
        //1.获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.获取对象
        IAccountService as = (IAccountService)ac.getBean("accountService");
        //3.执行方法
        as.saveAccount();
    }
}

```

### 5.4 切入点表达式的写法

* 关键字：`execution(表达式)`

* 表达式：`访问修饰符  返回值  包名.包名.包名...类名.方法名(参数列表)`

* 标准的表达式写法：`public void com.itheima.service.impl.AccountServiceImpl.saveAccount()`

* 访问修饰符可以省略 `void com.itheima.service.impl.AccountServiceImpl.saveAccount()`

* 返回值可以使用通配符，表示任意返回值 `* com.itheima.service.impl.AccountServiceImpl.saveAccount()`

* 包名可以使用通配符，表示任意包。但是有几级包，就需要写几个 `*.`：`* *.*.*.*.AccountServiceImpl.saveAccount())`

* 包名可以使用 `..` 表示当前包及其子包：`* *..AccountServiceImpl.saveAccount()`

* 类名和方法名都可以使用 `*` 来实现通配：`* *..*.*()`

* 参数列表：
  * 可以直接写数据类型：
    * 基本类型直接写名称         int
    * 引用类型写包名.类名的方式   java.lang.String
  * 可以使用通配符表示任意类型，但是必须有参数
  * 可以使用 `..` 表示有无参数均可，有参数可以是任意类型

* 全通配写法：`* *..*.*(..)`，实际开发中不建议使用。

* 实际开发中切入点表达式的通常写法：
	切到业务层实现类下的所有方法
		* com.itheima.service.impl.*.*(..)







### 5.5     