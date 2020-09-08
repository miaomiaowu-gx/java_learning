# 第三节 Mybatis入门案例中的设计模式分析

## 3.1 Mybatis 主函数分析

<img src="./img1/05-code-quick-start.png" width=800>



## 3.2 Mybatis 执行查询所有(findAll)分析


<img src="./img1/06-query-all-analyses.png" width=1400>



## 3.3 Mybatis 创建代理对象的分析

<img src="./img1/07-custom-mybatis-analysis.png" width=700>


## 3.4 自定义 Mybatis 框架

自定义 Mybatis 的分析：

* Mybatis 在使用代理 dao 的方式实现增删改查时，只做了两件事：

   * 第一：创建代理对象

   * 第二：在代理对象中调用selectList

自定义 Mybatis 通过入门案例看到类：

* `class Resources`

* `class SqlSessionFactoryBuilder`

* `interface SqlSessionFactory`

* `interface SqlSession`


### 实现

基于入门案例修改，去掉 `pom.xml` 文件中有关 mybatis 的坐标（即 dependency 项）。

1）在 main->java->com->itheima 下创建类 `mybatis.io.Resources`。

```java
package com.itheima.mybatis.io;

import java.io.InputStream;

// 使用类加载器读取配置文件的类
public class Resources {
    public static InputStream getResourceAsStream(String filePath){
         return Resources.class.getClassLoader().getResourceAsStream(filePath);
    }
}
```

在主函数文件中导入包 `import com.itheima.mybatis.io.Resources;`

2）在 mybatis 文件夹下，创建类 `sqlsession.SqlSessionFactoryBuilder`。

```java


         

