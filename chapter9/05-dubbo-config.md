## 第五节 Dubbo 配置


### 5.1 dubbo.properties & 属性加载顺序

#### 5.1.1 属性配置

&emsp;&emsp;以属性配置的方式来配置 Dubbo 应用，如果应用足够简单，例如，不需要多注册中心或多协议，并且需要在 spring 容器中共享配置，那么，可以直接使用 dubbo.properties 作为默认配置。

&emsp;&emsp;Dubbo 可以**自动加载** classpath 根目录下的 dubbo.properties，但是同样可以使用 JVM 参数来指定路径：`-Ddubbo.properties.file=xxx.properties`。



### 5.2 重写与优先级