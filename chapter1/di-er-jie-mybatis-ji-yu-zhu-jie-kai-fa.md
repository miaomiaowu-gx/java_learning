# 第二节 Mybatis 基于注解开发

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
   
   