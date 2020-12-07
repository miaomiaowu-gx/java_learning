## 第五节 Dubbo 配置


### 5.1 dubbo.properties & 属性加载顺序

#### 5.1.1 属性配置

&emsp;&emsp;以属性配置的方式来配置 Dubbo 应用，如果应用足够简单，例如，不需要多注册中心或多协议，并且需要在 spring 容器中共享配置，那么，可以直接使用 dubbo.properties 作为默认配置。

&emsp;&emsp;Dubbo 可以**自动加载** classpath 根目录下的 dubbo.properties，但是同样可以使用 JVM 参数来指定路径：`-Ddubbo.properties.file=xxx.properties`。


#### 5.1.2 重写与优先级


<img src="./img9/10-dubbo-properties-override.jpg" width=400>

优先级从高到低：

* JVM -D 参数：当部署或者启动应用时，它可以轻易地重写配置，比如，改变 dubbo 协议端口；

* XML：XML 中的当前配置会重写 dubbo.properties 中的；

* Properties：默认配置，仅仅作用于以上两者没有配置时。

> 如果在 classpath 下有超过一个 dubbo.properties 文件，比如，两个 jar 包都各自包含了 dubbo.properties，dubbo 将随机选择一个加载，并且打印错误日志。
> 如果 id 没有在 protocol 中配置，将使用 name 作为默认属性。


### 5.2 启动检查 



### 5.3 配置超时 & 配置覆盖关系


### 5.4 配置重试次数


### 5.5 配置多版本


### 5.6 配置本地存根


### 5.7 配置与 SpringBoot 整合的三种方式
