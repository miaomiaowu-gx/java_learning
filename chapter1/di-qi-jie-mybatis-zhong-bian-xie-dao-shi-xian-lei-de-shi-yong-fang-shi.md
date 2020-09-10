# 第七节 Mybatis 中编写 dao 实现类的使用方式

### 7.1 查询列表

1）在 java->com->itheima->dao文件夹下，创建类 `impl.UserDaoImpl`

```java
package com.itheima.dao.impl;

import com.itheima.dao.IUserDao;
import com.itheima.domain.User;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;

import java.util.List;

public class UserDaoImpl implements IUserDao{

    private SqlSessionFactory factory;
    public UserDaoImpl(SqlSessionFactory factory){
        this.factory = factory;
    }

    public List<User> findAll() {
        //1. 根据factory获取SqlSession对象
        SqlSession session = factory.openSession();
        //2. 调用SqlSession方法，实现查询列表
        List<User> users = session.selectList("com.itheima.dao.IUserDao.findAll"); //参数就是能获取配置信息的key
        //3. 释放资源
        session.close();
        return users;
    }
```

2）修改测试文件，配置文件保持不变

① 取消私有变量 `SqlSession` 的定义。
② 无需对 SqlSession 对象进行 commit 与 close 操作。


```java
package com.itheima.test;

import com.itheima.dao.IUserDao;
import com.itheima.dao.impl.UserDaoImpl;
import com.itheima.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;

import java.io.InputStream;
import java.util.Date;
import java.util.List;

public class MybatisTest {

    private InputStream in;
    //private SqlSession session;
    private IUserDao userDao;

    @Before //该注解用于在测试方法执行之前执行
    public void init() throws Exception{
        //1. 读取配置文件，生成字节输入流
        in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2. 获取SqlSessionFactory对象
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
        //3. 使用工厂对象创建dao对象

        userDao = new UserDaoImpl(factory);
    }

    @After //该方法用于在测试方法执行之后执行
    public void destroy() throws Exception{
        in.close();
    }

    @Test
    public void testFindAll() {

        //5. 执行查询所有方法
        List<User> users = userDao.findAll();
        for(User user : users){
            System.out.println(user);
        }
    }
}
```














