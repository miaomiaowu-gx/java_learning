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

工程和模块的区别：

* 工程不等于完整的项目，模块也不等于完整的项目，一个完整的项目看的是代码，代码完整，就可以说这是一个完整的项目。和此项目是工程和模块没有关系。

* 工程天生只能使用自己内部资源，天生是独立的。后天可以和其他工程或模块建立关联关系。

* 模块天生不是独立的，模块天生是属于父工程的，模块一旦创建，所有父工程的资源都可以使用。

* 父子工程之间，子模块天生集成父工程，可以使用父工程所有资源。

* 子模块之间天生是没有任何关系的。

* 父子工程之间不用建立关系，**继承关系**是先天的，不需要手动建立。

* 平级之间的引用叫**依赖**，依赖不是先天的，依赖是需要后天建立的。



### 3.4 传递依赖下来的包是否能用


<img src="./img7/12-transitive-dependency.png" width=700>



### 3.5 在父子工程中填充代码

1）在父工程的 pom.xml 中配置坐标。见第二节 pom.xml。


2）在 `maven_2_dao` 模块

* src->main->java 文件夹下创建 com.gx.domain 包，放入 Items 类。创建 com.gx.dao 包，放入 ItemsDao 接口。

* 


### 3.6 maven 父子工程三种启动方式






### 3.7 

### 3.8 


### 3.9 

### 3.10 




































































