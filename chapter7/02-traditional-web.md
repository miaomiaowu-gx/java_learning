## 第二节 Maven 传统 web 工程做数据查询操作

### 2.1 Maven 知识点总结

maven 是一个项目管理工具。主要功能：依赖管理、一键构建。

* **依赖管理**：maven 对项目中 jar 包的管理过程。传统工程直接把 jar 包放置在项目中。maven 工程真正的 jar 包放置在仓库中，项目中只用放置 jar 包的坐标。

* **一键构建**：maven 自身集成了 tomcat 插件，可以对项目进行编译，测试，打包，安装，发布等操作。


仓库的种类：本地仓库，远程仓库【私服】，中央仓库。

仓库之间的关系：当启动一个 maven 工程时，maven 工程会通过 pom 文件中 jar 包的坐标去本地仓库找对应 jar 包。

* 默认情况下，如果本地仓库没有对应 jar 包，maven 工程会自动去中央仓库下载 jar 包到本地仓库。

* 在公司中，如果本地没有对应 jar 包，会先从私服下载 jar 包，

* 如果私服没有 jar 包，可以从中央仓库下载，也可以从本地上传。

**maven 常用命令**：clean，compile，test，package，install，deploy。

**maven 三套生命周期**：清理生命周期，默认生命周期，站点生命周期。


### 2.2 环境准备

#### 2.2.1 数据库

创建 maven 数据库，执行如下 sql 语句

```sql
SET FOREIGN_KEY_CHECKS=0;
-- ----------------------------
-- Table structure for `items`
-- ----------------------------
DROP TABLE IF EXISTS `items`;
CREATE TABLE `items` (
  `id` int(10) NOT NULL auto_increment,
  `name` varchar(20) default NULL,
  `price` float(10,0) default NULL,
  `pic` varchar(40) default NULL,
  `createtime` datetime default NULL,
  `detail` varchar(200) default NULL,
  PRIMARY KEY  (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of items
-- ----------------------------
INSERT INTO `items` VALUES ('1', '传智播客', '1000', null, '2018-03-13 09:29:30', '带我走上人生巅峰');
INSERT INTO `items` VALUES ('2', '黑马310', null, null, '2018-03-28 10:05:52', '插入测试');
INSERT INTO `items` VALUES ('3', '黑马307', '199', null, '2018-03-07 10:08:04', '插入测试');
INSERT INTO `items` VALUES ('7', '插入测试', null, null, null, null);
INSERT INTO `items` VALUES ('8', '插入测试', null, null, null, null);
```


#### 2.2.2 创建 maven web 工程

1）选择 apache 下的 maven-archetype-webapp 骨架，GroupId 为 com.gx，项目名称 ArtifactId 为 maven_1. 

2）为 web.xml 添加 【web 配置文件头部声明】

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
          http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">

</web-app>
```

3）删掉 pom.xml 中多余的内容

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.gx</groupId>
  <artifactId>maven_1</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>
  
</project>
```                

### 2.3 maven 导入 jar 包时冲突的解决

maven 工程要导入 jar 包的坐标，就必须要考虑解决 jar 包冲突。

maven 导入 jar 包中的一些概念：

* 直接依赖：项目中直接导入的 jar 包，就是该项目的直接依赖包。

* 传递依赖：项目中没有直接导入的 jar 包，即项目直接依赖依赖的 jar 包。


**解决 jar 包冲突的方式一**：

* 第一声明优先原则：哪个 jar 包的坐标在靠上的位置，这个 jar 包就是先声明的。先声明的 jar 包坐标下的依赖包，可以优先进入项目中。

```xml
<dependencies>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-bean</artifactId>
    <version>4.2.4.RELEASE</version>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.0.2.RELEASE</version>
  </dependency>
</dependencies>
```

<img src"./img7/10-maven-pom-spring.png" width=500>

**解决 jar 包冲突的方式二**：

* 路径近者优先原则。直接依赖路径比传递依赖路径近，那么最终项目进入的 jar 包会是路径近的直接依赖包。


**解决 jar 包冲突的方式三【推荐使用】**：

* 直接排除法。当要排除某个 jar 包下依赖包，在配置 exclusions 标签的时候，内部可以不写版本号。因为此时依赖包使用的版本和默认和本 jar 包一样。










### 2.4 pom 文件内标签的讲解



### 2.5 dao 层代码编写



### 2.6 service 层代码编写



### 2.7 web 层代码编写
