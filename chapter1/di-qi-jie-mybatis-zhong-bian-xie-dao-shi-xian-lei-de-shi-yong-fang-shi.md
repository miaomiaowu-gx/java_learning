## 第七节 Mybatis 中编写 dao 实现类的使用方式

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


### 7.2 保存操作

1）为类添加 `impl.UserDaoImpl`实现方法 `saveUser`

```java
public void saveUser(User user) {
    //1. 根据factory获取SqlSession对象
    SqlSession session = factory.openSession();
    //2. 调用方法实现保存
    session.insert("com.itheima.dao.IUserDao.saveUser",user);
    //3. 提交
    session.commit();
    //4. 释放资源
    session.close();

}
```

2）测试

```java
//测试保存操作
@Test
public void testSave() {
    User user = new User();
    user.setUsername("Dao impl user3");
    user.setAddress("东北");
    user.setSex("女");
    user.setBirthday(new Date());

    System.out.println("保存操作之前："+user);
    //5. 执行保存方法
    userDao.saveUser(user);
    
    System.out.println("保存操作之后："+user);
}
```

### 7.3 更新操作


1）为类添加 `impl.UserDaoImpl`实现方法 `updateUser`

```java
public void updateUser(User user) {
    //1. 根据factory获取SqlSession对象
    SqlSession session = factory.openSession();
    //2. 调用方法实现更新
    session.update("com.itheima.dao.IUserDao.updateUser",user);
    //3. 提交
    session.commit();
    //4. 释放资源
    session.close();
}
```

2）测试

```java
//测试更新操作
@Test
public void testUpdate() {
    User user = new User();
    user.setId(55);
    user.setUsername("mybatis Update resultMap");
    user.setAddress("扬州");
    user.setSex("女");
    user.setBirthday(new Date());

    //5. 执行更新方法
    userDao.updateUser(user);
}
```

### 7.4 删除操作


1）为类添加 `impl.UserDaoImpl`实现方法 `deleteUser`

```java
public void deleteUser(Integer userId) {
    //1. 根据factory获取SqlSession对象
    SqlSession session = factory.openSession();
    //2. 调用方法实现删除，调用的是update方法
    session.update("com.itheima.dao.IUserDao.deleteUser",userId);
    //3. 提交
    session.commit();
    //4. 释放资源
    session.close();
}
```

2）测试

```java
//测试删除操作
@Test
public void testDelete() {

    //5. 执行删除方法
    userDao.deleteUser(55);
}
```

### 7.5 查询一个操作

1）为类添加 `impl.UserDaoImpl`实现方法 `findById`

```java
public User findById(Integer userId) {
    //1. 根据factory获取SqlSession对象
    SqlSession session = factory.openSession();
    //2. 调用SqlSession方法，实现查询列表
    User user = session.selectOne("com.itheima.dao.IUserDao.findById",userId);
    //3. 释放资源
    session.close();
    return user;
}
```

2）测试

```java
//测试查询一个操作
@Test
public void testFindOne() {
    //5. 执行查询一个方法
    User user = userDao.findById(49);
    System.out.println(user);
}
```

### 7.6 模糊操作


1）为类添加 `impl.UserDaoImpl`实现方法 `saveUser`

```java
public List<User> findByName(String username) {
    //1. 根据factory获取SqlSession对象
    SqlSession session = factory.openSession();
    //2. 调用SqlSession方法，实现查询列表
    List<User> users = session.selectList("com.itheima.dao.IUserDao.findByName",username); //参数就是能获取配置信息的key
    //3. 释放资源
    session.close();
    return users;
}
```

2）测试

```java

```




### 7.7 操作







1）为类添加 `impl.UserDaoImpl`实现方法 `saveUser`
2）测试

### 7.8 操作







1）为类添加 `impl.UserDaoImpl`实现方法 `saveUser`
2）测试













































































































































































































































































































































































































































































































































































