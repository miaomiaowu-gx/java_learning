# 第一节 Mybatis 框架快速入门

## 1. Mybatis 环境搭建


### 1.1 安装前准备


创建数据库要操作的表

```sql
CREATE DATABASE eesy_mybatis;
USE eesy_mybatis;

DROP TABLE IF EXISTS `user`;

CREATE TABLE `user` (
  `id` INT(11) NOT NULL AUTO_INCREMENT,
  `username` VARCHAR(32) NOT NULL COMMENT '用户名称',
  `birthday` DATETIME DEFAULT NULL COMMENT '生日',
  `sex` CHAR(1) DEFAULT NULL COMMENT '性别',
  `address` VARCHAR(256) DEFAULT NULL COMMENT '地址',
  PRIMARY KEY  (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT  INTO `user`(`id`,`username`,`birthday`,`sex`,`address`) VALUES (41,'老王','2018-02-27 17:47:08','男','北京'),
(42,'小二王','2018-03-02 15:09:37','女','北京金燕龙'),
(43,'小二王','2018-03-04 11:34:34','女','北京金燕龙'),
(45,'传智播客','2018-03-04 12:04:06','男','北京金燕龙'),
(46,'老王','2018-03-07 17:37:26','男','北京'),
(48,'小马宝莉','2018-03-08 11:44:00','女','北京修正');

SELECT * FROM USER;
```

<img src="./img1/02-dataset-mysql.png" width=600>

### 1.2 Mybatis 环境搭建

① 创建工程，选择 Maven，点击 Next，填写 GroupId 为 `com.itheima`，ArtifactId 为 `day01_eesy_01mybatis`，点击 Next、Finish。


② 配置 Maven 项目中的 pom.xml 文件

1）配置打包方式
`<packaging>jar</packaging>`

2）如果使用 Maven 来构建项目，则需将下面的依赖代码置于 pom.xml 文件中

```html
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>x.x.x</version>
</dependency>
```

3）项目还需要使用数据库、日志、单元测试。

```html
<dependencies>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.4.5</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.6</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.12</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.10</version>
    </dependency>
</dependencies>
```
<img src="./img1/03-external-libraries.png" width = 400>

③ 创建 User 类，其**实体类中的属性和数据库表的字段名称保持一致**。

在 src->main->java 文件夹下，创建类，包名为 `com.itheima.domain`，类名为 `User`，即在 Name 处填写 `com.itheima.domain.User`。

```java
package com.itheima.domain;

import java.io.Serializable;
import java.util.Date;

public class User implements Serializable{
    private Integer id;
    private String username;
    private Date birthday;
    private String sex;
    private String address;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday=" + birthday +
                ", sex='" + sex + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}

```


④ 创建 Dao 接口

在 java 文件夹下创建接口 `com.itheima.dao.IUserDao`(命名习惯：在接口文件前加I)。

```java
package com.itheima.dao;

import com.itheima.domain.User;
import java.util.List;

// 用户的持久层接口
public interface IUserDao {
    // 查询所有操作
    List<User> findAll();
}
```


⑤ 在 src->main->resources 下创建 `xml` 文件，命名为 `SqlMapConfig.xml` （命名没有要求，习惯用此命名）。

添加 Config 的约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
```

配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!-- mybatis 的主配置环境 -->
<configuration>
    <!-- 配置环境 -->
    <environments default="mysql">
        <!-- 配置 mysql 环境 -->
        <environment id="mysql">
            <!-- 配置事务类型 -->
            <transactionManager type="JDBC"></transactionManager>
            <!-- 配置数据源（连接池） -->
            <dataSource type="POOLED">
                <!-- 配置连接数据库的4个基本信息 -->
                <property name="driver" value="com.mysql.jdbc.Driver" />
                <property name="url" value="jdbc:mysql://localhost:3306/eesy_mybatis" />
                <property name="username" value="root" />
                <property name="password" value="root" />
            </dataSource>
        </environment>
    </environments>

    <!-- 指定映射配置文件的位置，映射配置文件指的是每个dao独立的配置文件 -->
    <mappers>
        <mapper resource="com/itheima/dao/IUserDao.xml" />
    </mappers>
</configuration>
```


⑥ 在 src->main->resources->com->itheima->dao 下创建 `IUserDao.xml` 文件。该文件是针对 IUserDao 类的配置。

添加 Mapper 的约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper  
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
```

配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- namespace 处填写限定类名 -->
<mapper namespace="com.itheima.dao.IUserDao">
    <!-- 配置查询所有 id放入IUserDao类中 查询所有的函数名-->
    <select id="findAll">
        SELECT * FROM USER 
    </select>
</mapper>
```


### 1.3 Mybatis 环境搭建注意

第一个：创建 IUserDao.xml 和 IUserDao.java 时，名称是为了和之前的知识保持一致。在Mybatis中，它把持久层的操作接口名称和映射文件也叫做：Mapper。所以，**IUserDao 和 IUserMapper 是一样的**。

第二个：在 idea 中创建目录的时候，它和包是不一样的。包在创建时：输入 `com.itheima.dao` 创建三级结构。目录在创建时：`com.itheima.dao` 是一级目录，即目录名字为 `com.itheima.dao`。

第三个：**mybatis 的映射配置文件位置必须和 dao 接口的包结构相同**。

第四个：**映射配置文件的 mapper 标签 namespace 属性的取值必须是 dao 接口的全限定类名**。

第五个：**映射配置文件的操作配置（select），id 属性的取值必须是 dao 接口的方法名**。


🍓 当开发者遵从了第三，四，五点之后，在开发中就无须再写 dao 的实现类。

## 2. Mybatis 入门
