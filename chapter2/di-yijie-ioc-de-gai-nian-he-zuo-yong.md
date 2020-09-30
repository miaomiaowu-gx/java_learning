## 第一节 程序耦合

### 1.1 程序耦合

概念：在软件工程中，耦合指的就是就是对象之间的依赖性。对象之间的耦合越高，维护成本越高。因此对象的设计应使类和构件之间的耦合最小。软件设计中通常用**耦合度和内聚度**作为衡量模块独立程度的标准。**划分模块的一个准则就是高内聚低耦合。**


分类：
（1）**内容耦合**。当一个模块直接修改或操作另一个模块的数据时，或一个模块不通过正常入口而转入另一个模块时，这样的耦合被称为内容耦合。内容耦合是最高程度的耦合，应该避免使用之。
（2）**公共耦合**。两个或两个以上的模块共同引用一个全局数据项，这种耦合被称为公共耦合。在具有大量公共耦合的结构中，确定究竟是哪个模块给全局变量赋了一个特定的值是十分困难的。
（3）**外部耦合**。一组模块都访问同一全局简单变量而不是同一全局数据结构，而且不是通过参数表传递该全局变量的信息，则称之为外部耦合。
（4）**控制耦合**。一个模块通过接口向另一个模块传递一个控制信号，接受信号的模块根据信号值而进行适当的动作，这种耦合被称为控制耦合。
（5）**标记耦合**。若一个模块 A 通过接口向两个模块 B 和 C 传递一个公共参数，那么称模块 B 和 C 之间存在一个标记耦合。
（6）**数据耦合**。模块之间通过参数来传递数据，那么被称为数据耦合。数据耦合是最低的一种耦合形式，系统中一般都存在这种类型的耦合，因为为了完成一些有意义的功能，往往需要将某些模块的输出数据作为另一些模块的输入数据。
（7）**非直接耦合**。两个模块之间没有直接关系，它们之间的联系完全是通过主模块的控制和调用来实现的。


**总结**：耦合是影响软件复杂程度和设计质量的一个重要因素，在设计上应采用以下原则：**如果模块间必须存在耦合，就尽量使用数据耦合，少用控制耦合，限制公共耦合的范围，尽量避免使用内容耦合**。


**简单的说**：耦合，即程序间的依赖关系，包括**类之间的依赖**与**方法间的依赖**。解耦，即降低程序间的依赖关系。实际开发中，应该做到【编译期不依赖，运行时才依赖】。

**解耦的思路：**
* 第一步：使用反射来创建对象，而避免使用new关键字。
* 第二步：通过读取配置文件来获取要创建的对象全限定类名

#### JDBC 注册驱动解耦

【问题】 JDBC 操作，注册驱动时，为什么不使用 DriverManager 的 register 方法，而是采用 Class.forName 的方式。

```java
public class JdbcDemo1 {
    public static void main(String[] args) throws  Exception{
        //1.注册驱动
        //DriverManager.registerDriver(new com.mysql.jdbc.Driver());
        Class.forName("com.mysql.jdbc.Driver");

        //2.获取连接
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/eesy","root","mysql");
        //3.获取操作数据库的预处理对象
        PreparedStatement pstm = conn.prepareStatement("select * from account");
        //4.执行SQL，得到结果集
        ResultSet rs = pstm.executeQuery();
        //5.遍历结果集
        while(rs.next()){
            System.out.println(rs.getString("name"));
        }
        //6.释放资源
        rs.close();
        pstm.close();
        conn.close();
    }
}
```

1. 使用 `DriverManager.registerDriver` 注册驱动时，当对应的 mysql jar包不存在，会在编译器报错。应该尽量避免编译器报错。
2. 用 `Class.forName("com.mysql.jdbc.Driver")` 替换 `new com.mysql.jdbc.Driver()`，此时，`"com.mysql.jdbc.Driver"`只是一个字符串，当 jar 包不存在时，会抛出异常（编译可以通过）。
  * 好处：类中不再依赖具体的驱动类，此时就算删除 mysql 的驱动 jar 包，依然可以编译（运行就不要想了，没有驱动不可能运行成功的） 。
  * mysql 驱动的全限定类名字符串是在 java 类中写死的，一旦要改还是要修改源码。（解决：使用配置文件配置。）


### 1.2 工厂模式解耦


【问题如下】

<img src="./img2/01-wrong-code-analysis.png" width=1100>

当持久层具体实现类 `AccountDaoImpl.java` 不存在，由于 `new AccountDaoImpl()`，程序报错。表现层的 `new AccountServiceImpl()`也会由于 AccountServiceImpl 类不存在，而编译不通过。













