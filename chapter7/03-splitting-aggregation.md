## 第三节 Maven 工程拆分与聚合


### 3.1 maven 工程拆分与聚合的思想


<img src="./img7/11-split-aggregation.png" width=700>


### 3.2 maven 父子工程的创建

1）创建父工程，选择 new Project 或者 new Module 都可以，父工程只要有 pom.xml 就可以。此处，创建 Maven 工程，无需选任何骨架，GroupId 为 com.gx，ArtifactId 为 `maven_2_parent`。删除 src 文件。

2）选中父工程，右键，New Module，选择 Maven 工程，创建 dao 模块，与页面没有交互，是 java 工程，不用选择骨架，ArtifactId 为 `maven_2_dao`。

3）同理，创建 `maven_2_service` 模块，不用选择任何骨架。

4）创建 `maven_2_web` 模块，选择 apache 下的 `maven-archetype-webapp` 骨架。删除该模块 pom.xml 多余的自动生成内容。

```java
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>maven_2_parent</artifactId>
        <groupId>com.gx</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>maven_2_web</artifactId>
    <packaging>war</packaging>
</project>
```  


### 3.3 工程和模块的关系以及继承和依赖的概念





### 3.4 传递依赖下来的包是否能用






### 3.5 在父子工程中填充代码







### 3.6 maven 父子工程三种启动方式






### 3.7 

### 3.8 


### 3.9 

### 3.10 




































































