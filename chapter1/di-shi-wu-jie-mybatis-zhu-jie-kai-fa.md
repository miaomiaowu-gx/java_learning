## 第十五节 Mybatis 注解开发

### 15.1 环境搭建

1. 向 pom.xml 导入坐标

```xml
<packaging>jar</packaging>

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

2. 在 main->resources 添加主配置文件 SqlMapConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <!-- 配置properties -->
    <properties resource="jdbcConfig.properties"></properties>

    <!--使用typeAliases配置别名，它只能配置domain中类的别名 -->
    <typeAliases>
        <package name="com.itheima.domain"></package>
    </typeAliases>

    <!-- 配置环境，default参数为默认选择的环境 -->
    <environments default="mysql">
        <environment id="mysql">
            <!-- 配置事务 -->
            <transactionManager type="JDBC"></transactionManager>
            <!-- 配置连接池 -->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>
    </environments>

    <!-- 配置映射文件位置 -->
    <mappers>
        <!-- package标签是用于指定dao接口所在的包,当指定了之后就不需要在写mapper以及resource或者class了 -->
        <package name="com.itheima.dao"></package>
    </mappers>
</configuration>
```


3. 在 main->java 文件夹下创建实体类 `com.itheima.domain.User`

```java
public class User implements Serializable{
    private Integer id;
    private String username;
    private String address;
    private String sex;
    private Date birthday;
    
    ... getter and setter 方法
}
```

4. 在 main->java 文件夹下创建接口 `com.itheima.dao.IUserDao`

在 mybatis 中针对，CRUD 一共有四个注解 `@Select @Insert @Update @Delete`

```java
public interface IUserDao {
    // @Select(value="select * from user") 只有一个value属性，则value可以省略
    @Select("select * from user")
    List<User> findAll();
}
```

5. 在 test->java 下创建 `com.itheima.test.MybatisAnnoTest` 用于测试

```java
public class MybatisAnnoTest  {
    public static void main(String[] args) throws  Exception {
        //1.获取字节输入流
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.根据字节输入流构建SqlSessionFactory
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
        //3.根据SqlSessionFactory生产一个SqlSession
        SqlSession session = factory.openSession();
        //4.使用SqlSession获取Dao的代理对象
        IUserDao userDao = session.getMapper(IUserDao.class);
        //5.执行Dao的方法
        List<User> users = userDao.findAll();
        for(User user : users){
            System.out.println(user);
        }
        //6.释放资源
        session.close();
        in.close();
    }
}
```

注意：在使用注解开发时，尽管没有使用对应的 xml 文件（在主配置文件中），只要该 xml 文件存在，就会报错。无论是否使用。解决：删掉对应配置文件，或者将配置文件移动到不相干的位置。


### 15.2 注解开发-当实体类名与数据库名字不对应时

实体类修改为

```java
public class User implements Serializable{

    private Integer userId;
    private String userName;
    private String userAddress;
    private String userSex;
    private Date userBirthday;
    
}
```




### 15.3 



### 15.4  

 