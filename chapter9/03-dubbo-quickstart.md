## 第三节 Dubbo 入门


### 3.1 环境搭建

#### 3.1.1 ZooKeeper 注册中心

##### 安装

1 在官网下载 Zookeeper 压缩包，解压。

2 在 zookeeper-x.x.xx 文件夹的 conf 文件夹下，复制 zoo_sample.cfg 文件并重命名为 zoo.cfg。

3 在 zookeeper-x.x.xx 文件夹下创建 data 文件夹，修改 zoo.cfg 配置：

```cfg
dataDir=../data
```

4 运行 bin 文件夹下的 zkServer.cmd 启动服务端，zkCli.cmd 启动客户端。


##### 使用

&emsp;&emsp;Zookeeper 是 Apache Hadoop 的子项目，是一个**树型的目录服务**，支持**变更推送**，适合作为 Dubbo 服务的注册中心，工业强度较高，可用于生产环境，并推荐使用。

<img src="./img9/09-zookeeper.jpg" width=400>


**流程说明**：

* **服务提供者启动时**: 向 `/dubbo/com.foo.BarService/providers` 目录下写入自己的 URL 地址

* **服务消费者启动时**: 订阅 `/dubbo/com.foo.BarService/providers` 目录下的提供者 URL 地址。并向 `/dubbo/com.foo.BarService/consumers` 目录下写入自己的 URL 地址

* **监控中心启动时**: 订阅 `/dubbo/com.foo.BarService` 目录下的所有提供者和消费者 URL 地址。

**客户端操作**：

* 获取根节点下的值 `get /`

```cmd
cZxid = 0x0
ctime = Thu Jan 01 08:00:00 CST 1970
mZxid = 0x0
mtime = Thu Jan 01 08:00:00 CST 1970
pZxid = 0x0
cversion = -1
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 0
numChildren = 1
```

* 获取节点 `ls /`

```
[zookeeper]
```

* 创建节点并设置值 `create -e /atguigu 123456`，然后查看节点 `ls /`

```
[zookeeper, atguigu]
```

* 获取创建节点的值 `get /atguigu`

```
123456
cZxid = 0x2
ctime = Fri Dec 04 15:36:31 CST 2020
mZxid = 0x2
mtime = Fri Dec 04 15:36:31 CST 2020
pZxid = 0x2
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x10010f255ab0000
dataLength = 6
numChildren = 0
```

#### 3.1.2 搭建管理控制台(监控中心 dubbo-admin)  

1 下载 dubbo-admin 对应的 zip 包。github: https://github.com/apache/dubbo-admin/tree/master。

2 解压后进入目录修改指定 zookeeper 地址

* 查看 dubbo-admin-master\dubbo-admin\src\main\resources\application.properties 文件。

* 将 zookeeper 的监控中心的地址配置为本地端口，`dubbo.registry.address=zookeeper://127.0.0.1:2181`。

3 在dubo-zookeeper\dubbo-admin-master\dubbo-admin文件夹下 cmd 打包，输入 `mvn clean package`。

4 在生成的 target 目录下，找到 jar 包。命令行 `java -jar dubbo-admin-0.0.1-SNAPSHOT.jar`。

5 浏览器 localhost:7001 访问，默认情况下，用户与密码均是 root。

**查看端口占用情况**

```shell
netstat -ano|findstr "7001"
//输出
//  TCP    0.0.0.0:7001           0.0.0.0:0              LISTENING       9556
//  TCP    [::]:7001              [::]:0                 LISTENING       9556

taskkill /T /F /PID 9556
// 成功: 已终止 PID 9556 (属于 PID 7736 子进程)的进程。
```

### 3.2 dubbo-helloword

#### 3.2.1 需求

某个电商系统，**订单服务**需要调用**用户服务**获取**某个用户的所有地址**。

现在需要创建两个服务模块进行测试：

| 模块                  | 功能           |
| --------------------- | -------------- |
| 订单服务 web 模块     | 创建订单等     |
| 用户服务 service 模块 | 查询用户地址等 |


#### 3.2.2 创建提供者消费者工程


1）创建工程 dubbohelloword 工程，删除 src 文件夹，groupId 设为 com.gx

2）创建 gmall-interface 模块，用于存放实体类与接口文件。

* 在 java 下创建包 com.gx.gmall.bean 存放 UserAddress 类。

    ```java
    package com.gx.gmall.bean;
    
    import java.io.Serializable;
    
    public class UserAddress implements Serializable {
        private Integer id;
        private String userAddress; //用户地址
        private String userId; //用户id
        private String consignee; //收货人
        private String phoneNum; //电话号码
        private String isDefault; //是否为默认地址    Y-是     N-否
        // getter and setter ...
    }
    ```

* 在 java 下创建包 com.gx.gmall.service 存放 OrderService 与 UserService 接口。

    ```java
    package com.gx.gmall.service;
    
    public interface OrderService {
        /**
         * 初始化订单
         */
        public void initOrder(String userID);
    }
    ```

    ```java
    package com.gx.gmall.service;
    
    import com.gx.gmall.bean.UserAddress;
    import java.util.List;
    
    public interface UserService {
        /**
         * 按照用户id返回所有的收货地址
         */
        public List<UserAddress> getUserAddressList(String userId);
    }
    ```

3）创建 user-service-provider 模块，服务提供者

* 在 pom.xml 中加入对 gmail-interface 的依赖

    ```xml
    <dependency>
        <groupId>com.gx</groupId>
        <artifactId>gmail-interface</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
    ```

* 在 java 下创建 com.gx.gmall.service.impl 包，并创建 UserServiceImpl 实现类

    ```java
    package com.gx.gmall.service.impl;
    
    import com.gx.gmall.bean.UserAddress;
    import com.gx.gmall.service.UserService;
    
    import java.util.Arrays;
    import java.util.List;
    
    public class UserServiceImpl implements UserService {
        public List<UserAddress> getUserAddressList(String userId) {
    
            UserAddress address1 = new UserAddress(1, "河南省郑州巩义市宋陵大厦2F", "1", "安然", "150360313x", "Y");
            UserAddress address2 = new UserAddress(2, "北京市昌平区沙河镇沙阳路", "1", "情话", "1766666395x", "N");
    
            return Arrays.asList(address1,address2);
        }
    }
    ```


4）创建 order-service-consumer 模块，服务消费者(订单服务)

* 在 pom.xml 中加入对 gmail-interface 的依赖

    ```xml
    <dependency>
        <groupId>com.gx</groupId>
        <artifactId>gmail-interface</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
    ```

* 在 java 下创建 com.gx.gmall.service.impl 包，并创建 OrderServiceImpl 实现类
  
    ```java
    package com.gx.gmall.service.impl;
    
    import com.gx.gmall.bean.UserAddress;
    import com.gx.gmall.service.OrderService;
    import com.gx.gmall.service.UserService;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Service;
    
    import java.util.List;
    
    @Service
    public class OrderServiceImpl implements OrderService {
        @Autowired
        public UserService userService;
        public void initOrder(String userID) {
            //查询用户的收货地址
            List<UserAddress> userAddressList = userService.getUserAddressList(userID);
            System.out.println(userAddressList);
        }
    }
    ```

#### 3.2.3 服务提供者配置&测试

1）服务提供者项目中添加依赖 pom.xml

```xml
<!--dubbo-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>dubbo</artifactId>
    <version>2.6.2</version>
</dependency>
<!--注册中心是 zookeeper，引入zookeeper客户端-->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>2.12.0</version>
</dependency>
```

2）在 resource 文件中创建 provider.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd
		http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
    <!--1、指定当前服务/应用的名字(同样的服务名字相同，不要和别的服务同名)-->
    <dubbo:application name="user-service-provider"></dubbo:application>
    <!--2、指定注册中心的位置-->
    <!--<dubbo:registry address="zookeeper://127.0.0.1:2181"></dubbo:registry>-->
    <dubbo:registry protocol="zookeeper" address="127.0.0.1:2181"></dubbo:registry>
    <!--3、指定通信规则（通信协议? 服务端口）此处name值是固定的-->
    <dubbo:protocol name="dubbo" port="20880"></dubbo:protocol>
    <!--4、暴露服务 让别人调用 ref指向服务的真正实现对象-->
    <dubbo:service interface="com.gx.gmall.service.UserService" ref="userServiceImpl"></dubbo:service>

    <!--服务的实现-->
    <bean id="userServiceImpl" class="com.gx.gmall.service.impl.UserServiceImpl"></bean>
</beans>
```


3）在 java 下的 com.gx.gmall 包下创建 MainApplication 类

```java
package com.gx.gmall;

import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.io.IOException;

public class MainApplication {
    public static void main(String[] args) throws IOException {
        ClassPathXmlApplicationContext applicationContext= new ClassPathXmlApplicationContext("provider.xml");
        applicationContext.start();
        //为了让程序不中止，阻塞读取字符
        System.in.read();
    }
}
```

4）启动 zookeeper 与 dubbo-admin，运行项目，在 localhost://7001 查看服务情况。

#### 3.2.4 服务消费者配置&测试


1）服务消费者项目中添加依赖 pom.xml

```xml
<!--dubbo-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>dubbo</artifactId>
    <version>2.6.2</version>
</dependency>
<!--注册中心是 zookeeper，引入zookeeper客户端-->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>2.12.0</version>
</dependency>
```

2）在 resource 文件中创建 consumer.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd
		http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
   
    <!--包扫描-->
    <context:component-scan base-package="com.gx.gmall.service.impl"/>

    <!--指定当前服务/应用的名字(同样的服务名字相同，不要和别的服务同名)-->
    <dubbo:application name="order-service-consumer"></dubbo:application>
    <!--指定注册中心的位置-->
    <dubbo:registry address="zookeeper://127.0.0.1:2181"></dubbo:registry>

    <!--调用远程暴露的服务，生成远程服务代理-->
    <dubbo:reference interface="com.gx.gmall.service.UserService" id="userService"></dubbo:reference>
</beans>
```

3）在 java 下的 com.gx.gmall 包下创建 ConsumerApplication 类

```java
package com.gx.gmall;

import com.gx.gmall.service.OrderService;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.io.IOException;

public class ConsumerApplication {
    public static void main(String[] args) throws IOException {
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("consumer.xml");
        OrderService orderService = applicationContext.getBean(OrderService.class);

        //调用方法查询出数据
        orderService.initOrder("1");
        System.out.println("调用完成...");
        System.in.read();
    }
}
```

4）启动 zookeeper 与 dubbo-admin，运行项目，在 localhost://7001 查看服务情况。


### 3.3 监控中心 Simple Monitor 安装配置

简单的监控中心：dubbo-monitor-simple