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

基于上述代码，在 dao 文件夹下，添加实现类 `impl.UserDaoImpl`
    
    
      
        
            