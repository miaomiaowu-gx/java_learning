# 第二节 Mybatis 基于注解开发

## 1. 基于注解开发

1. 创建 Maven 项目，`GroupId = com.itheima`，`ArtifactId = day01_eesy_02mybatis_annotation`。

2. 大部分代码同【第一节】代码一致，微修改。

   * 删除 resources 文件夹下的 `IUserDao.xml` 文件及其所在包。
   
   * 在 IUserDao 接口上使用@Select注解，并且指定SQL语句。
   
   ```java
   public interface IUserDao {
      @Select("select * from user")
      List<User> findAll();
   }
   ```
   
   * 修改 `SqlMapConfig.xml` 文件中的 mapper 配置，使用 class 属性指定 dao 接口的全限定类名。
   
   ```xml
   <mappers>
        <mapper class = "com.itheima.dao.IUserDao"/>
    </mappers>
   ```
   
      
         
## 2. Mybatis 开发注意

需要明确

* 在实际开发中，都是越简便越好。不管使用** XML 还是注解**配置，都是采用**不写 dao 实现类**的方式。

* 但是 Mybatis 支持写 dao 实现类的。 

基于实现类的开发示例

1）基于上述代码，在 dao 文件夹下，添加实现类 `impl.UserDaoImpl`。
    
```java
package com.itheima.dao.impl;

import com.itheima.dao.IUserDao;
import com.itheima.domain.User;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;

import java.util.List;

public class UserDaoImpl implements IUserDao {
    private SqlSessionFactory factory;

    // 构造函数
    public UserDaoImpl(SqlSessionFactory factory){
        this.factory = factory;
    }
    public List<User> findAll() {
        //1. 使用工厂创建SqlSession对象
        SqlSession session = factory.openSession();
        //2. 使用session执行查询所有方法
        List<User> users = session.selectList("com.itheima.dao.IUserDao.findAll");
        session.close();
        return users;
    }
}
```
      
2）修改测试文件 `MybatisTest.java`

```java
package com.itheima.test;

import com.itheima.dao.IUserDao;
import com.itheima.dao.impl.UserDaoImpl;
import com.itheima.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;
import java.util.List;

public class MybatisTest {

    public static void main(String[] args)throws Exception {
        //1.读取配置文件
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.创建SqlSessionFactory工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);
        //3.使用工厂创建Dao对象
        IUserDao userDao = new UserDaoImpl(factory);
        //4.使用代理对象执行方法
        List<User> users = userDao.findAll();
        for(User user : users){
            System.out.println(user);
        }
        //5.释放资源
        in.close();
    }
}
```                                
            