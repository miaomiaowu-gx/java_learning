# 第七节 Mybatis 中编写 dao 实现类的使用方式

### 7.1 查询列表

1）在 java->com->itheima->dao文件夹下，创建类 `impl.UserDaoImpl`

```java

```

2）修改测试文件

① 取消私有变量 `SqlSession` 的定义。
② 无需对 SqlSession 对象进行 commit 与 close 操作。
