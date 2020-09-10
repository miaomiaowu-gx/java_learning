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

2）修改测试文件

① 取消私有变量 `SqlSession` 的定义。
② 无需对 SqlSession 对象进行 commit 与 close 操作。
