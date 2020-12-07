## 第四节 Dubbo 与 SpringBoot 整合


### 4.1 适配版本

|versions|java|Spring Boot|Dubbo|
|----|----|----|----|
|0.2.0|1.8+|2.0.x|2.6.2 +|
|0.1.1|1.7+|1.5.x|2.6.2 +|



### 4.2 boot-user-service-provider 服务提供者

1）创建一个 boot 服务者模块，Group 为 com.gx，Artifact 为 boot-user-service-provider，Name 为 boot-user-service-provider，Package 为 com.gx.gmall。下一步，不用选中任何功能模块。

2）在 java 下的 com.gx.gmall 下创建 service.impl 包，并添加 UserServiceImpl 类，具体实现见第三节。

3）在 pom.xml 中添加对公共接口的依赖

```xml
<dependency>
	<groupId>com.gx</groupId>
	<artifactId>gmail-interface</artifactId>
	<version>1.0-SNAPSHOT</version>
</dependency>
```

### 4.3 boot-order-service-consumer 服务消费者

1）创建一个 boot 消费者模块，Group 为 com.gx，Artifact 为 boot-order-service-consumer，Name 为 boot-order-service-consumer，Package 为 com.gx.gmall。下一步，选择 web 功能模块。

2）在 java 下的 com.gx.gmall 下创建 service.impl 包，并添加 OrderServiceImpl 类，具体实现见第三节。

3）在 pom.xml 中添加对公共接口的依赖

```xml
<dependency>
	<groupId>com.gx</groupId>
	<artifactId>gmail-interface</artifactId>
	<version>1.0-SNAPSHOT</version>
</dependency>
```




2）

