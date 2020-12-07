## 第四节 Dubbo 与 SpringBoot 整合


### 4.1 适配版本

|versions|java|Spring Boot|Dubbo|
|----|----|----|----|
|0.2.0|1.8+|2.0.x|2.6.2 +|
|0.1.1|1.7+|1.5.x|2.6.2 +|



### 4.2 boot-user-service-provider 服务提供者


#### 4.2.1 基础代码准备 


1）创建一个 boot 服务者模块，Group 为 com.gx，Artifact 为 boot-user-service-provider，Name 为 boot-user-service-provider，Package 为 com.gx.gmall。下一步，不用选中任何功能模块。

2）在 java 下的 com.gx.gmall 下创建 service.impl 包，并添加 UserServiceImpl 类，具体实现见第三节。

3）在 pom.xml 中添加对公共接口的依赖

```xml
<dependency>
	<groupId>com.gx</groupId>
	<artifactId>gmall-interface</artifactId>
	<version>1.0-SNAPSHOT</version>
</dependency>
```


#### 4.2.2 配置

1）在 pom.xml 中导入 dubbo 依赖

```xml
<dependency>
	<groupId>com.alibaba.boot</groupId>
	<artifactId>dubbo-spring-boot-starter</artifactId>
	<version>0.2.1.RELEASE</version>
</dependency>
```

2）配置 resources 文件夹下的 application.properties 文件

```properties 
dubbo.application.name=boot-user-service-provider
dubbo.registry.address=127.0.0.1:2181
dubbo.registry.protocol=zookeeper

dubbo.protocol.name=dubbo
dubbo.protocol.port=20880

#连接监控中心
dubbo.monitor.protocol=registry
```     
 
3）使用 dubbo 的 @Service 注解暴漏服务

```java
 package com.gx.gmall.service.impl;

import com.gx.gmall.bean.UserAddress;
import com.gx.gmall.service.UserService;
import com.alibaba.dubbo.config.annotation.Service;
import org.springframework.stereotype.Component;

import java.util.Arrays;
import java.util.List;

@Service //dubbo用于声明provider的注解
@Component //用于标注业务层组件（service）
public class UserServiceImpl implements UserService {

    @Override
    public List<UserAddress> getUserAddressList(String userId) {

        UserAddress address1 = new UserAddress(1, "河南省郑州巩义市宋陵大厦2F", "1", "安然", "150360313x", "Y");
        UserAddress address2 = new UserAddress(2, "北京市昌平区沙河镇沙阳路", "1", "情话", "1766666395x", "N");

        return Arrays.asList(address1,address2);
    }
}
```     
     
4）BootProviderApplication 启动类配置

```java
package com.gx.gmall;

import com.alibaba.dubbo.config.spring.context.annotation.EnableDubbo;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@EnableDubbo //开启基于注解的dubbo功能
@SpringBootApplication
public class BootUserServiceProviderApplication {

	public static void main(String[] args) {
		SpringApplication.run(BootUserServiceProviderApplication.class, args);
	}

}
```
                  
                                                      

### 4.3 boot-order-service-consumer 服务消费者


#### 4.3.1 基础代码准备 

1）创建一个 boot 消费者模块，Group 为 com.gx，Artifact 为 boot-order-service-consumer，Name 为 boot-order-service-consumer，Package 为 com.gx.gmall。下一步，选择 web 功能模块。

2）在 java 下的 com.gx.gmall 下创建 service.impl 包，并添加 OrderServiceImpl 类。在 gmall-interface 中将接口 OrderService 的返回值由 void 改为 `List<UserAddress>`。

```java
package com.gx.gmall.service.impl;

import com.gx.gmall.bean.UserAddress;
import com.gx.gmall.service.OrderService;
import com.gx.gmall.service.UserService;
import jdk.nashorn.internal.ir.annotations.Reference;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class OrderServiceImpl implements OrderService {
    //@Autowired
    @Reference //引用远程提供者服务
    public UserService userService;
    public List<UserAddress> initOrder(String userID) {
        //查询用户的收货地址
        List<UserAddress> userAddressList = userService.getUserAddressList(userID);
        return userAddressList;
    }
}
```


3）在 pom.xml 中添加对公共接口的依赖

```xml
<dependency>
	<groupId>com.gx</groupId>
	<artifactId>gmall-interface</artifactId>
	<version>1.0-SNAPSHOT</version>
</dependency>
```

4）在 java 下的 com.gx.gmall 下创建 controller 包，并添加 OrderController 类

```java
package com.gx.gmall.controller;

import com.gx.gmall.bean.UserAddress;
import com.gx.gmall.service.OrderService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.List;

@Controller
public class OrderController  {
    @Autowired
    OrderService orderService;

    @RequestMapping("/initOrder")
    @ResponseBody //可以以json方式写回
    public List<UserAddress> initOrder(@RequestParam("uid")String userId) {
        return orderService.initOrder(userId);
    }
}
```

#### 4.3.2 配置 



5）

