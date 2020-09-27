## 第十一节 Mybatis 多表查询

表之间的关系有几种：一对多、多对一、一对一、多对多。

### 10.1 示例：用户和账户

需求：
* 一个用户可以有多个账户
* 一个账户只能属于一个用户（多个账户也可以属于同一个用户）

步骤：
1、建立两张表：用户表，账户表
* 让用户表和账户表之间具备一对多的关系：需要使用外键在账户表中添加。

2、建立两个实体类：用户实体类和账户实体类
* 让用户和账户的实体类能体现出来一对多的关系。

3、建立两个配置文件
* 用户的配置文件
* 账户的配置文件

4、实现配置：
* 当查询用户时，可以同时得到用户下所包含的账户信息。
* 当查询账户时，可以同时得到账户的所属用户信息。

账户表准备：

```sql
DROP TABLE IF EXISTS `account`;

CREATE TABLE `account` (
  `ID` INT(11) NOT NULL COMMENT '编号',
  `UID` INT(11) DEFAULT NULL COMMENT '用户编号',
  `MONEY` DOUBLE DEFAULT NULL COMMENT '金额',
  PRIMARY KEY  (`ID`),
  KEY `FK_Reference_8` (`UID`),
  CONSTRAINT `FK_Reference_8` FOREIGN KEY (`UID`) REFERENCES `user` (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT  INTO `account`(`ID`,`UID`,`MONEY`) VALUES (1,46,1000),(2,45,1000),(3,46,2000);
```

建立 Account.java 实体类文件，IAccountDao.java 接口文件（生命方法），以及配置文件 IAccountDao.xml，以及测试文件 MybatisTest.java。


查询 Account 表，并显示对应的用户名以及地址信息（在User表中）。

1 建立 AccountUser.java 

```java
package com.itheima.domain;

public class AccountUser extends Account {
    private String username;
    private String address;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return super.toString() + "          AccountUser{" +
                "username='" + username + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}
```

2 在 IAccountDao.java 中添加方法：

```java
List<AccountUser> findAllAccount();
```

3 在 IAccountDao.xml 文件中添加配置：





### 10.2 示例：用户和角色

