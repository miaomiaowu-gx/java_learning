## 第五节 JDBC-Java 语言操作数据库

### 5.1 介绍

**概念**：Java DataBase Connectivity (JDBC，Java数据库连接，即使用Java语言操作数据库)

**本质**：官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口。各个数据库厂商实现这套接口，提供数据库驱动jar包。使用JDBC接口编程，真正执行的代码是驱动jar包中的实现类。

<img src="https://img2020.cnblogs.com/blog/2051825/202007/2051825-20200719202237796-1625288882.bmp" width=600>

**快速入门步骤**

~~~
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;

public class JdbcQuickStart {
    public static void main(String[] args) throws Exception {
        //1. 导入驱动jar包【mysql-connector-java-5.1.37-bin.jar】
            // * 将jar包复制到项目的libs目录（自己创建的）
            //* 在该目录上右键 --> Add As Library (将 jar 包加入到项目中)
        //2. 注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //3. 获取数据库连接对象 Connection
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db3", "root","mysql");
        //4. 定义sql（sql语句后面不要加分号）
        String sql = "update account set balance = 500 where id = 1";
        //5. 获取执行sql语句的对象 Statement
        Statement statement = conn.createStatement();
        //6. 执行sql，接受返回结果
        int count = statement.executeUpdate(sql);
        //7. 处理结果
        System.out.println(count);  // 1
        //8. 释放资源
        statement.close();
        conn.close();
    }
}
~~~

### 5.2 JDBC各个类详解

#### 5.2.1 DriverManager 驱动管理对象

1）**注册驱动**：告诉程序该使用哪一个数据库驱动 jar 包

`static void registerDriver(Driver driver) `：向 DriverManager 注册给定驱动程序。 
写代码时使用：`Class.forName("com.mysql.jdbc.Driver");`。 

通过查看源码发现，在 com.mysql.jdbc.Driver 类中存在静态代码块：
~~~
public class Driver implements java.sql.Driver {

      public Driver() throws SQLException {
      }

      static {
            try {
                  DriverManager.registerDriver(new Driver()); //注册数据库驱动
            } catch (SQLException var1) {
                  throw new RuntimeException("Can't register driver!");
            }
      }
}
~~~

注：在MySQL 5 版本后，可以省略注册驱动的步骤，即不使用代码 `Class.forName("com.mysql.jdbc.Driver");`，其可以自动注册驱动。


2）**获取数据库的连接**

`static Connection getConnection(String url, String user, String password) `：试图建立到给定数据库 URL 的连接。 

* String url：指定连接的路径，语法 `jdbc:mysql://ip地址(域名):端口号/数据库`。
      如果连接的是本机 mysql 服务器，并且 mysql 服务器的默认端口是 3306，则 url 可以简化为`jdbc:mysql:///数据库`。

* String user：用户名 

* String password：密码


#### 5.2.2 Connection 数据库连接对象

1）获取执行 sql 的对象

`Statement createStatement()` ：创建一个 Statement 对象来将 SQL 语句发送到数据库。 

`PreparedStatement prepareStatement(String sql)` ：创建一个 PreparedStatement 对象来将参数化的 SQL 语句发送到数据库。 

2）管理事务

**开启事务** 

`void setAutoCommit(boolean autoCommit)` 调用该方法设置参数为false，即开启事务。 

**提交事务**

`void commit()` 使所有上一次提交/回滚后进行的更改成为持久更改，并释放此 Connection 对象当前持有的所有数据库锁。

**回滚事务**

`void rollback()` 取消在当前事务中进行的所有更改，并释放此 Connection 对象当前持有的所有数据库锁。

#### 5.2.3 Statement 执行静态 SQL 语句

**用于执行【静态 SQL 语句】并返回它所生成结果的对象。**

**静态 SQL 语句**：参数都是给定值。

**执行 sql 的三种方法**：

boolean execute(String sql) 可以执行任意的 sql，该语句可能返回多个结果。（了解，用的不多）
* 如果第一个结果为 ResultSet 对象，则返回 true；如果其为更新计数或者不存在任何结果，则返回 false。 

int executeUpdate(String sql) 执行 DML(INSERT、UPDATE、DELETE) 语句、DDL(CREATE、ALTER、DROP)语句
* 返回值：影响的行数。可以通过影响的行数判断，增删改查的语句是否执行成功。返回值大于0，则执行成功。
* DDL 语句一般不用此方法，直接使用 mysql 语句执行，因为使用该语句时，一般已经连接了某一数据库，对数据库/表的增删改查不方便用此语句。
      

ResultSet executeQuery(String sql) 执行给定的 DQL(SELECT) 语句，该语句返回单个 ResultSet 对象。 

**练习**

 <details>
<summary>练习一：account表 添加记录【insert 语句】</summary>
<pre>
<code>
package cn.itcast.jdbc;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

// account表 添加一条记录 insert 语句
public class JDBCDemo2 {

    public static void main(String[] args) {
        Statement stmt = null;
        Connection conn = null;
        try {
            //1. 注册驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2. 定义sql
            String sql = "insert into account values(null,'王五',3000)";
            //3.获取Connection对象
            conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "root");
            //4.获取执行sql的对象 Statement
            stmt = conn.createStatement();
            //5.执行sql
            int count = stmt.executeUpdate(sql);//影响的行数
            //6.处理结果
            System.out.println(count);
            if(count > 0){
                System.out.println("添加成功！");
            }else{
                System.out.println("添加失败！");
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            //stmt.close();  有可能在conn阶段出现异常，stmt未被赋值，调用close()造成空指针异常
            //7. 释放资源
            //避免空指针异常
            if(stmt != null){
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
</code>
</pre>
</details>

 <details>
<summary>练习二：account表 修改记录【update 语句】</summary>
<pre>
<code>
package cn.itcast.jdbc;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

// account表 修改记录
public class JDBCDemo3 {

    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        try {
            //1. 注册驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2.获取连接对象
            conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "root");
            //3.定义sql
            String sql  = "update account set balance = 1500 where id = 3";
            //4.获取执行sql对象
            stmt = conn.createStatement();
            //5.执行sql
            int count = stmt.executeUpdate(sql);
            //6.处理结果
            System.out.println(count);
            if(count > 0){
                System.out.println("修改成功！");
            }else{
                System.out.println("修改失败");
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            //7.释放资源
            if(stmt != null){
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
</code>
</pre>
</details>

 <details>
<summary>练习三：account表 删除记录【delete 语句】</summary>
<pre>
<code>
package cn.itcast.jdbc;
import cn.itcast.util.JDBCUtils;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

// account表 删除一条记录
public class JDBCDemo4 {
	
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        try {
            //1. 注册驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2.获取连接对象
            conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "root");
           //conn = JDBCUtils.getConnection("jdbc:mysql:///db3", "root", "root");
            //3.定义sql
            String sql  = "delete from account where id = 3";
            //4.获取执行sql对象
            stmt = conn.createStatement();
            //5.执行sql
            int count = stmt.executeUpdate(sql);
            //6.处理结果
            System.out.println(count);
            if(count > 0){
                System.out.println("删除成功！");
            }else{
                System.out.println("删除失败");
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            //7.释放资源
            if(stmt != null){
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
</code>
</pre>
</details>

 <details>
<summary>执行DDL语句 update创建表</summary>
<pre>
<code>
package cn.itcast.jdbc;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

// 执行DDL语句
public class JDBCDemo5 {

    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        try {
            //1. 注册驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2.获取连接对象
            conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "root");
            //3.定义sql
            String sql  = "create table student (id int , name varchar(20))";
            //4.获取执行sql对象
            stmt = conn.createStatement();
            //5.执行sql
            int count = stmt.executeUpdate(sql);
            //6.处理结果
            System.out.println(count);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            //7.释放资源
            if(stmt != null){
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
</code>
</pre>
</details>



#### 5.2.4 ResultSet 结果集对象，封装查询的结果

<img src="https://img2020.cnblogs.com/blog/2051825/202007/2051825-20200720153858881-472372691.png" width=300>

光标初始指向第一行（无数据可获取），调用next()函数指向下一行，不断获取数据。每次只能读取一行的一列数据。

**基本使用**

1）boolean next() 将光标从当前位置向下移一行，并判断当前行是否是最后一行之后（是否有数据）。如果是，返回false，说明没有数据了。如果不是，返回true，说明有数据。

2）getXXX(参数) 获取数据，其中XXX为根据实际情况的数据类型。

      // 以 Java 编程语言中 int 的形式获取此 ResultSet 对象的当前行中指定列的值。 
    
      int getInt(int columnIndex) // 参数代表列的编号，从 1 开始，如 getInt(1);
    
      int getInt(String columnLabel) // 参数为列名称，如 getInt("balance"); 
    
      // 以 Java 编程语言中 String 的形式获取此 ResultSet 对象的当前行中指定列的值。 
    
      String getNString(int columnIndex) 
    
      String getNString(String columnLabel) 

3）ResultSet 结果集也是资源，使用后需要释放。      

 <details>
<summary>一个基本使用示例</summary>
<pre>
<code>
package cn.itcast.jdbc;
import java.sql.*;

public class JDBCDemo6 {

    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            //1. 注册驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2.获取连接对象
            conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "root");
            //3.定义sql
            String sql  = "select * from account";
            //4.获取执行sql对象
            stmt = conn.createStatement();
            //5.执行sql
            rs = stmt.executeQuery(sql);
            //6.处理结果
            //6.1 让游标向下移动一行
            rs.next();
            //6.2 获取数据
            int id = rs.getInt(1);
            String name = rs.getString("name");
            double balance = rs.getDouble(3);
            System.out.println(id + "---" + name + "---" + balance);
            //6.1 让游标向下移动一行
            rs.next();
            //6.2 获取数据
            int id2 = rs.getInt(1);
            String name2 = rs.getString("name");
            double balance2 = rs.getDouble(3);
            System.out.println(id2 + "---" + name2 + "---" + balance2);    
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            //7.释放资源
            if(rs != null){
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(stmt != null){
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
</code>
</pre>
</details>

存在问题：上述读取数据，是基于已知数据库该表中有两条 数据，但对于未知数据集，读取数据时要判断数据是否存在。

使用步骤：
1. 游标向下移动一行
2. 判断是否有数据
3. 获取数据

~~~
while(rs.next()){
      int id = rs.getInt(1);
      String name = rs.getString("name");
      double balance = rs.getDouble(3);

      System.out.println(id + "---" + name + "---" + balance);
}
~~~


**使用实例**

emp 表结构

<img src="https://img2020.cnblogs.com/blog/2051825/202007/2051825-20200720160437290-1084569422.png" width=700>

emp 表数据

<img src="https://img2020.cnblogs.com/blog/2051825/202007/2051825-20200720160518024-1277868196.png" width=550>

定义一个方法，查询 emp 表的数据，将其封装为对象，然后装载在集合中，返回，打印。

1. 定义 Emp 类
2. 定义方法 public List<Emp> findAll(){}
3. 实现方法

 <details>
<summary>定义 Emp 类</summary>
<pre>
<code>
package cn.itcast.domain;
import java.util.Date;

//封装Emp表数据的JavaBean
public class Emp {
	
    private int id;
    private String ename;
    private int job_id;
    private int mgr;
    private Date joindate;
    private double salary;
    private double bonus;
    private int dept_id;


    public int getId() {
        return id;
    }
    
    public void setId(int id) {
        this.id = id;
    }
    
    public String getEname() {
        return ename;
    }
    
    public void setEname(String ename) {
        this.ename = ename;
    }
    
    public int getJob_id() {
        return job_id;
    }
    
    public void setJob_id(int job_id) {
        this.job_id = job_id;
    }
    
    public int getMgr() {
        return mgr;
    }
    
    public void setMgr(int mgr) {
        this.mgr = mgr;
    }
    
    public Date getJoindate() {
        return joindate;
    }
    
    public void setJoindate(Date joindate) {
        this.joindate = joindate;
    }
    
    public double getSalary() {
        return salary;
    }
    
    public void setSalary(double salary) {
        this.salary = salary;
    }


    public int getDept_id() {
        return dept_id;
    }
    
    public void setDept_id(int dept_id) {
        this.dept_id = dept_id;
    }


    public double getBonus() {
        return bonus;
    }
    
    public void setBonus(double bonus) {
        this.bonus = bonus;
    }
    
    @Override
    public String toString() {
        return "Emp{" +
                "id=" + id +
                ", ename='" + ename + '\'' +
                ", job_id=" + job_id +
                ", mgr=" + mgr +
                ", joindate=" + joindate +
                ", salary=" + salary +
                ", bonus=" + bonus +
                ", dept_id=" + dept_id +
                '}';
    }
}

</code>
</pre>
</details>

 <details>
<summary>使用实例</summary>
<pre>
<code>
package cn.itcast.jdbc;
import cn.itcast.domain.Emp;
import cn.itcast.util.JDBCUtils;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

//定义一个方法，查询emp表的数据将其封装为对象，然后装载集合，返回。
public class JDBCDemo8 {

    public static void main(String[] args) {
        List<Emp> list = new JDBCDemo8().findAll();
        System.out.println(list);
        System.out.println(list.size());
    }
     
    public List<Emp> findAll(){
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        List<Emp> list = null;
        try {
            //1.注册驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2.获取连接
            conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "root");
            //3.定义sql
            String sql = "select * from emp";
            //4.获取执行sql的对象
            stmt = conn.createStatement();
            //5.执行sql
            rs = stmt.executeQuery(sql);
            //6.遍历结果集，封装对象，装载集合
            Emp emp = null;
            list = new ArrayList<Emp>();
            while(rs.next()){
                //获取数据
                int id = rs.getInt("id");
                String ename = rs.getString("ename");
                int job_id = rs.getInt("job_id");
                int mgr = rs.getInt("mgr");
                Date joindate = rs.getDate("joindate");
                double salary = rs.getDouble("salary");
                double bonus = rs.getDouble("bonus");
                int dept_id = rs.getInt("dept_id");
                // 创建emp对象,并赋值
                emp = new Emp();
                emp.setId(id);
                emp.setEname(ename);
                emp.setJob_id(job_id);
                emp.setMgr(mgr);
                emp.setJoindate(joindate);
                emp.setSalary(salary);
                emp.setBonus(bonus);
                emp.setDept_id(dept_id);
    
                //装载集合
                list.add(emp);
            }
    
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            if(rs != null){
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
    
            if(stmt != null){
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
    
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        return list;
    }
}

</code>
</pre>
</details>

#### 5.2.5 抽取 JDBC 工具类：JDBCUtils

**目的**：代码重复度太高（注册驱动、释放资源等），通过抽取工作类简化书写。

**分析**：
1. 抽取注册驱动方法

2. 抽取一个方法获得连接对象
      需求：不想传递参数（太麻烦），不能写死（保证工具类的通用性）
      解决：配置文件
      ~~~
      // jdbc.properties

      url=jdbc:mysql:///db3
      user=root
      password=root
      driver=com.mysql.jdbc.Driver
      ~~~

3. 抽取一个方法释放资源

**工具类特点**：所有方法是静态的，方便调用。

**静态代码块**：
      静态代码块随着类的加载而加载，只会执行一次。
      只有静态变量才能被静态方法访问，被静态代码块访问。 
      静态代码块只能处理异常，不能抛出异常。抛出异常需要方法。

 <details>
<summary>JDBC工具类 JDBCUtils</summary>
<pre>
<code>
package cn.itcast.util;

import java.io.FileReader;
import java.io.IOException;
import java.net.URL;
import java.sql.*;
import java.util.Properties;

// JDBC工具类
public class JDBCUtils {
    private static String url;
    private static String user;
    private static String password;
    private static String driver;

    // 文件的读取，只需要读取一次即可拿到这些值。使用静态代码块
    static{
        //读取资源文件，获取值。
        try {
            //1. 创建Properties集合类。
            Properties pro = new Properties();
    
            //获取src路径下的文件的方式--->ClassLoader 类加载器
            ClassLoader classLoader = JDBCUtils.class.getClassLoader();
            URL res  = classLoader.getResource("jdbc.properties");
            String path = res.getPath();
            // System.out.println(path);///D:/IdeaProjects/itcast/out/production/day04_jdbc/jdbc.properties
    	   
            //2. 加载文件
            pro.load(new FileReader(path));
    
            //3. 获取数据，赋值
            url = pro.getProperty("url");
            user = pro.getProperty("user");
            password = pro.getProperty("password");
            driver = pro.getProperty("driver");
            //4. 注册驱动
            Class.forName(driver);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
    
    // 获取连接
    public static Connection getConnection() throws SQLException {
    
        return DriverManager.getConnection(url, user, password);
    }
    
    // 释放资源
    public static void close(Statement stmt,Connection conn){
        if( stmt != null){
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    
        if( conn != null){
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }


    // 释放资源
    public static void close(ResultSet rs,Statement stmt, Connection conn){
        if( rs != null){
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    
        if( stmt != null){
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    
        if( conn != null){
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}

</code>
</pre>
</details>

 <details>
<summary>使用示例</summary>
<pre>
<code>
package cn.itcast.jdbc;
import cn.itcast.domain.Emp;
import cn.itcast.util.JDBCUtils;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

//定义一个方法，查询emp表的数据将其封装为对象，然后装载集合，返回。
public class JDBCDemo8 {

    public static void main(String[] args) {
        List<Emp> list = new JDBCDemo8().findAll2();
        System.out.println(list);
        System.out.println(list.size());
    }
    
    // 演示JDBC工具类
    public List<Emp> findAll2(){
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        List<Emp> list = null;
        try {
            //1.注册驱动
            //2.获取连接
            conn = JDBCUtils.getConnection();
            //3.定义sql
            String sql = "select * from emp";
            //4.获取执行sql的对象
            stmt = conn.createStatement();
            //5.执行sql
            rs = stmt.executeQuery(sql);
            //6.遍历结果集，封装对象，装载集合
            Emp emp = null;
            list = new ArrayList<Emp>();
            while(rs.next()){
                //获取数据
                int id = rs.getInt("id");
                String ename = rs.getString("ename");
                int job_id = rs.getInt("job_id");
                int mgr = rs.getInt("mgr");
                Date joindate = rs.getDate("joindate");
                double salary = rs.getDouble("salary");
                double bonus = rs.getDouble("bonus");
                int dept_id = rs.getInt("dept_id");
                // 创建emp对象,并赋值
                emp = new Emp();
                emp.setId(id);
                emp.setEname(ename);
                emp.setJob_id(job_id);
                emp.setMgr(mgr);
                emp.setJoindate(joindate);
                emp.setSalary(salary);
                emp.setBonus(bonus);
                emp.setDept_id(dept_id);
    
                //装载集合
                list.add(emp);
            }
    
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {          
            JDBCUtils.close(rs,stmt,conn);
        }
        return list;
    }
}

</code>
</pre>
</details>

**练习**：

1. 通过键盘录入用户名与密码

2. 判断用户是否登录成功(有一张表存放用户与密码)
      * `select * from User where username="输入的用户名" and password="输入的密码";` 如果这个 sql 查询有结果，则成功。

~~~
-- 创建数据库 USER 表
CREATE TABLE USER(
      id INT PRIMARY KEY AUTO_INCREMENT,
      username VARCHAR(32),
      PASSWORD VARCHAR(32)
);

INSERT INTO USER VALUES(NULL, 'zhangsan', '123');
INSERT INTO USER VALUES(NULL, 'lisi', '234');

select * from USER;
~~~

 <details>
<summary>判断用户是否登录成功</summary>
<pre>
<code>
package cn.itcast.jdbc;
import cn.itcast.util.JDBCUtils;
import java.sql.*;
import java.util.Scanner;

public class JDBCDemo9 {

    public static void main(String[] args) {
        //1.键盘录入，接受用户名和密码
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名：");
        String username = sc.nextLine();
        System.out.println("请输入密码：");
        String password = sc.nextLine();
        //2.调用方法
        boolean flag = new JDBCDemo9().login(username, password);
        //3.判断结果，输出不同语句
        if(flag){
            //登录成功
            System.out.println("登录成功！");
        }else{
            System.out.println("用户名或密码错误！");
        }
    }
    
    public boolean login(String username ,String password){
        if(username == null || password == null){
            return false;
        }
        //连接数据库判断是否登录成功
        Connection conn = null;
        Statement stmt =  null;
        ResultSet rs = null;
        //1.获取连接
        try {
            conn =  JDBCUtils.getConnection();
            //2.定义sql
            String sql = "select * from user where username = '"+username+"' and password = '"+password+"' ";
            System.out.println(sql);
            //3.获取执行sql的对象
            stmt = conn.createStatement();
            //4.执行查询
            rs = stmt.executeQuery(sql);
            //5.判断
           return rs.next();//如果有下一行，则返回true
    
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JDBCUtils.close(rs,stmt,conn);
        }
        return false;
    }
}
</code>
</pre>
</details>

上述【判断用户是否登录成功】代码存在问题，由 SQL 注入问题引起。

<img src="https://img2020.cnblogs.com/blog/2051825/202007/2051825-20200720214333807-1677627910.png" width=550>


#### 5.2.6 PreparedStatement 执行 sql 的对象（功能更强大）

**SQL注入问题**：在拼接 sql 时，有一些 sql 的特殊关键字参与字符串的拼接，会造成安全性问题。

在【判断用户是否登录成功】的练习中，随便输入用户，输入密码：`a' or 'a' = 'a`，会生成 sql 语句 `select * from user where username ='fhdsjkf' and password = 'a' or 'a' = 'a'`  false and false or true -->  true，因此可以查询到所有数据。

**解决SQL注入问题**：使用 PreparedStatement 对象。

`public interface PreparedStatement extends Statement` 表示预编译的 SQL 语句的对象。 

**预编译的 SQL**：参数使用<font color=#ff8918>**？**</font>作为占位符，执行 sql 时，给<font color=#ff8918>**？**</font>赋值。
如：`PreparedStatement pstmt = con.prepareStatement("UPDATE EMPLOYEES SET SALARY = ? WHERE ID = ?");` 语句不会受传入字符串的关键字影响。

**步骤**：

1. 导入驱动jar包【mysql-connector-java-5.1.37-bin.jar】
2. 注册驱动
3. 获取数据库连接对象 Connection
4. 定义sql（sql语句后面不要加分号）
      * sql的参数使用?作为占位符。如 `select * from user where username = ? and password = ?;`
5. 获取执行sql语句的对象 PreparedStatement 
      * `PreparedStatement prepareStatement(String sql) throws SQLException`
      * `conn.prepareStatement(sql)` 需要传递sql参数，而 `conn.createStatement()`无需传参。
6. 给?赋值 
      * `setXxx(参数1, 参数2)` 其中，参数1表示?的位置，从1开始；参数2表示?的值。
7. 执行sql，接受返回结果。
      * 此时无序再传递 sql 语句，在 PreparedStatement 使用时已经传递。
8. 处理结果
9. 释放资源

 <details>
<summary>PreparedStatement 判断用户是否登录成功</summary>
<pre>
<code>
package cn.itcast.jdbc;

import cn.itcast.util.JDBCUtils;

import java.sql.*;
import java.util.Scanner;

public class JDBCDemo9 {

    public static void main(String[] args) {
        //1.键盘录入，接受用户名和密码
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名：");
        String username = sc.nextLine();
        System.out.println("请输入密码：");
        String password = sc.nextLine();
        //2.调用方法
        boolean flag = new JDBCDemo9().login2(username, password);
        //3.判断结果，输出不同语句
        if(flag){
            //登录成功
            System.out.println("登录成功！");
        }else{
            System.out.println("用户名或密码错误！");
        }
    }
       
    // 登录方法,使用PreparedStatement实现
    public boolean login2(String username ,String password){
        if(username == null || password == null){
            return false;
        }
        //连接数据库判断是否登录成功
        Connection conn = null;
        PreparedStatement pstmt =  null;
        ResultSet rs = null;
        //1.获取连接
        try {
            conn =  JDBCUtils.getConnection();
            //2.定义sql
            String sql = "select * from user where username = ? and password = ?";
            //3.获取执行sql的对象
            pstmt = conn.prepareStatement(sql);
            //给?赋值
            pstmt.setString(1,username);
            pstmt.setString(2,password);
            //4.执行查询,不需要传递sql
            rs = pstmt.executeQuery();
            //5.判断
            return rs.next();//如果有下一行，则返回true
    
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JDBCUtils.close(rs,pstmt,conn);
        }
        return false;
    }
}
</code>
</pre>
</details>

**注意**：后期都会使用PreparedStatement来完成增删改查的所有操作。
* 可以放置 SQL 注入
* 效率更高

### 5.3 JDBC 管理事务

<img src="https://img2020.cnblogs.com/blog/2051825/202007/2051825-20200721205134525-979800277.png" width=700>

银行转账案例

 <details>
<summary>展示代码</summary>
<pre>
<code>
package cn.itcast.jdbc;
import cn.itcast.util.JDBCUtils;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

// 事务操作
public class JDBCDemo10 {

    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement pstmt1 = null;
        PreparedStatement pstmt2 = null;
    
        try {
            //1.获取连接
            conn = JDBCUtils.getConnection();
            //开启事务
            conn.setAutoCommit(false);
    
            //2.定义sql
            //2.1 张三 - 500
            String sql1 = "update account set balance = balance - ? where id = ?";
            //2.2 李四 + 500
            String sql2 = "update account set balance = balance + ? where id = ?";
            //3.获取执行sql对象
            pstmt1 = conn.prepareStatement(sql1);
            pstmt2 = conn.prepareStatement(sql2);
            //4. 设置参数
            pstmt1.setDouble(1,500);
            pstmt1.setInt(2,1);
    
            pstmt2.setDouble(1,500);
            pstmt2.setInt(2,2);
            //5.执行sql
            pstmt1.executeUpdate();
            // 手动制造异常
            int i = 3/0;
    
            pstmt2.executeUpdate();
            //提交事务
            conn.commit();
        } catch (Exception e) {
            //事务回滚
            try {
                if(conn != null) {
                    conn.rollback();
                }
            } catch (SQLException e1) {
                e1.printStackTrace();
            }
            e.printStackTrace();
        }finally {
            JDBCUtils.close(pstmt1,conn);
            JDBCUtils.close(pstmt2,null);
        }
    }
}
</code>
</pre>
</details>

### 5.4 数据库连接池

#### 5.4.1 基本介绍

**概念**：其实就是一个容器(集合)，存放数据库连接的容器。当系统初始化好后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取连接对象，用户访问完之后，会将连接对象归还给容器。
	
**好处**：

* 节约资源

* 用户访问高效
	

**标准接口**：`DataSource`   （javax.sql包下的）

**方法**：

* 获取连接：`Connection getConnection()`

* 归还连接：`Connection对象.close()` 如果连接对象Connection是从连接池中获取的，那么调用Connection对象.close()方法，则不会再关闭连接了，而是归还连接。

对于连接池，一般我们不去实现它，由数据库厂商来实现：

* **C3P0**：数据库连接池技术

* **Druid**：数据库连接池实现技术，由阿里巴巴提供的


#### 5.4.2 C3P0：数据库连接池技术

**步骤**：

**1. 导入jar包 (两个)** 【c3p0-0.9.5.2.jar】【mchange-commons-java-0.2.12.jar】 ，
* 不要忘记导入数据库驱动jar包【mysql-connector-java-5.1.37-bin.jar】

**2. 定义配置文件**：
* 名称： c3p0.properties 或者 c3p0-config.xml
* 路径：直接将文件放在src目录下即可。

**3. 创建核心对象** 数据库连接池对象 ComboPooledDataSource

**4. 获取连接**： getConnection

~~~
// 1.创建数据库连接池对象 🔶 当不传参数时，使用的是xml文件中的默认配置<default-config>；
DataSource ds  = new ComboPooledDataSource();

// 2. 获取一个连接对象
Connection conn = ds.getConnection();

// 1.1 获取DataSource，使用指定名称配置
DataSource ds  = new ComboPooledDataSource("otherc3p0");
~~~

~~~
//c3p0-config.xml
<c3p0-config>
  <!-- 使用默认的配置读取连接池对象 -->
  <default-config>
  	<!--  连接参数 -->
    <property name="driverClass">com.mysql.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/db4</property>
    <property name="user">root</property>
    <property name="password">mysql</property>
    
    <!-- 连接池参数 -->
    <property name="initialPoolSize">5</property>
    <property name="maxPoolSize">10</property>
    <property name="checkoutTimeout">3000</property>
  </default-config>

  <named-config name="otherc3p0"> 
    <!--  连接参数 -->
    <property name="driverClass">com.mysql.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/day25</property>
    <property name="user">root</property>
    <property name="password">root</property>
    
    <!-- 连接池参数 -->
    <property name="initialPoolSize">5</property>
    <property name="maxPoolSize">8</property>
    <property name="checkoutTimeout">1000</property>
  </named-config>
</c3p0-config>
~~~



#### 5.4.3 Druid：数据库连接池实现技术，由阿里巴巴提供的

**步骤**：

**1. 导入jar包**【druid-1.0.9.jar】

**2. 定义配置文件**：
* 是properties形式的
* 可以叫任意名称，可以放在任意目录下

**3. 加载配置文件**，`Properties`。

**4. 获取数据库连接池对象**：通过工厂类来获取  DruidDataSourceFactory

**5. 获取连接**：getConnection

~~~
//1.导入jar包
//2.定义配置文件
//3.加载配置文件
Properties pro = new Properties();
InputStream is = DruidDemo.class.getClassLoader().getResourceAsStream("druid.properties");
pro.load(is);
//4.获取连接池对象
DataSource ds = DruidDataSourceFactory.createDataSource(pro);
//5.获取连接
Connection conn = ds.getConnection();
~~~

~~~
// 配置文件 druid.properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/db3
username=root
password=root
initialSize=5
maxActive=10
maxWait=3000
~~~



**定义工具类**

1. 定义一个类 JDBCUtils

2. 提供静态代码块加载配置文件，初始化连接池对象

3. 提供方法
      1）获取连接方法：通过数据库连接池获取连接
      2）释放资源
      3）获取连接池的方法

 <details>
<summary>JDBCUtils 工具类</summary>
<pre>
<code>
package cn.itcast.utils;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.IOException;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

// Druid连接池的工具类
public class JDBCUtils {

    //1.定义成员变量 DataSource
    private static DataSource ds ;
    
    static{
        try {
            //1.加载配置文件
            Properties pro = new Properties();
            pro.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
            //2.获取DataSource
            ds = DruidDataSourceFactory.createDataSource(pro);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }


​    
​    // 获取连接
​    public static Connection getConnection() throws SQLException {
​        return ds.getConnection();
​    }
​    
​    // 释放资源
​    public static void close(Statement stmt,Connection conn){
​       close(null,stmt,conn);
​    }


    public static void close(ResultSet rs , Statement stmt, Connection conn){
    
        if(rs != null){
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    
        if(stmt != null){
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    
        if(conn != null){
            try {
                conn.close();//归还连接
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
    
    // 获取连接池方法
    public static DataSource getDataSource(){
        return  ds;
    }

}

</code>
</pre>
</details>

使用工具类，完成添加操作：给account表添加一条记录。		

 <details>
<summary>使用工具类</summary>
<pre>
<code>
import cn.itcast.utils.JDBCUtils;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

// 使用新的工具类
public class DruidDemo2 {

    public static void main(String[] args) {       
        // 完成添加操作：给account表添加一条记录
        Connection conn = null;
        PreparedStatement pstmt = null;
        try {
            //1.获取连接
            conn = JDBCUtils.getConnection();
            //2.定义sql
            String sql = "insert into account values(null,?,?)";
            //3.获取pstmt对象
            pstmt = conn.prepareStatement(sql);
            //4.给？赋值
            pstmt.setString(1,"王五");
            pstmt.setDouble(2,3000);
            //5.执行sql
            int count = pstmt.executeUpdate();
            System.out.println(count);
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            //6. 释放资源
            JDBCUtils.close(pstmt,conn);
        }
    }
}
</code>
</pre>
</details>



### 5.5 Spring JDBC: JDBC Template

Spring框架对JDBC的简单封装。提供了一个JDBCTemplate对象简化JDBC的开发。


**步骤**：

**1. 导入jar包**（5个）
* 【commons-logging-1.2.jar】【spring-beans-5.0.0.RELEASE.jar】【spring-core-5.0.0.RELEASE.jar】【spring-jdbc-5.0.0.RELEASE.jar】【spring-tx-5.0.0.RELEASE.jar】

**2. 创建JdbcTemplate对象**，依赖于数据源DataSource。
* JdbcTemplate template = new JdbcTemplate(ds);
	

**3. 调用JdbcTemplate的方法来完成CRUD的操作**
* update(): 执行DML语句。增、删、改语句
* queryForMap(): 查询结果将结果集封装为map集合，将列名作为key，将值作为value 将这条记录封装为一个map集合
      * 注意：这个方法<font color=#ff8918>**查询的结果集长度只能是1**</font>
* queryForList(): 查询结果将结果集封装为list集合
      * 注意：将每一条记录封装为一个Map集合，再将Map集合装载到List集合中
* query(): 查询结果，将结果封装为<font color=#ff8918>**JavaBean对象**</font>。
      * query有两个参数：1）sql语句 2）RowMapper对象
      * 一般我们使用BeanPropertyRowMapper实现类。可以完成数据到JavaBean的自动封装
      * `new BeanPropertyRowMapper<类型>(类型.class)`
* queryForObject：查询结果，将结果封装为对象
      * 一般用于聚合函数的查询


~~~
package cn.itcast.jdbctemplate;

import cn.itcast.utils.JDBCUtils;
import org.springframework.jdbc.core.JdbcTemplate;

/**
 * JdbcTemplate入门
 */
public class JdbcTemplateDemo1 {

    public static void main(String[] args) {
        //1.导入jar包
        //2.创建JDBCTemplate对象
        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
        //3.调用方法
        String sql = "update account set balance = 5000 where id = ?";
        int count = template.update(sql, 3);
        System.out.println(count);
    }
}
~~~

上述代码，不需要自己在释放资源、释放连接，JdbcTemplate会自己完成这些步骤。




**操作 emp 表练习**

<img src="https://img2020.cnblogs.com/blog/2051825/202007/2051825-20200721225549717-1013203040.png" width=550>

1. 修改1号数据的 salary 为 10000

2. 添加一条记录

3. 删除刚才添加的记录

4. 查询id为1的记录，将其封装为Map集合

5. 查询所有记录，将其封装为List

6. 查询所有记录，将其封装为Emp对象的List集合

7. 查询总记录数
	

 <details>
<summary>封装 Emp 对象</summary>
<pre>
<code>
package cn.itcast.domain;
import java.util.Date;

public class Emp {

    private Integer id;   // 用Integer而不是int定义
    private String ename;
    private Integer job_id;
    private Integer mgr;
    private Date joindate;
    private Double salary; 
    private Double bonus; // 用 Double 而不是 double 定义。🍓 因为奖金bonus在数据库中可能是 null 值，基本类型 double 不能接受 null 值。
    private Integer dept_id;
    
    public Integer getId() {
        return id;
    }
    
    public void setId(Integer id) {
        this.id = id;
    }
    
    public String getEname() {
        return ename;
    }
    
    public void setEname(String ename) {
        this.ename = ename;
    }
    
    public Integer getJob_id() {
        return job_id;
    }
    
    public void setJob_id(Integer job_id) {
        this.job_id = job_id;
    }
    
    public Integer getMgr() {
        return mgr;
    }
    
    public void setMgr(Integer mgr) {
        this.mgr = mgr;
    }
    
    public Date getJoindate() {
        return joindate;
    }
    
    public void setJoindate(Date joindate) {
        this.joindate = joindate;
    }
    
    public Double getSalary() {
        return salary;
    }
    
    public void setSalary(Double salary) {
        this.salary = salary;
    }
    
    public Double getBonus() {
        return bonus;
    }
    
    public void setBonus(Double bonus) {
        this.bonus = bonus;
    }
    
    public Integer getDept_id() {
        return dept_id;
    }
    
    public void setDept_id(Integer dept_id) {
        this.dept_id = dept_id;
    }
    
    @Override
    public String toString() {
        return "Emp{" +
                "id=" + id +
                ", ename='" + ename + '\'' +
                ", job_id=" + job_id +
                ", mgr=" + mgr +
                ", joindate=" + joindate +
                ", salary=" + salary +
                ", bonus=" + bonus +
                ", dept_id=" + dept_id +
                '}';
    }
}

</code>
</pre>
</details>


 <details>
<summary>展示代码</summary>
<pre>
<code>
package cn.itcast.jdbctemplate;
import cn.itcast.domain.Emp;
import cn.itcast.utils.JDBCUtils;
import org.junit.Test;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import java.sql.Date;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;
import java.util.Map;

public class JdbcTemplateDemo2 {

    //Junit单元测试，可以让方法独立执行
    
    //1. 获取JDBCTemplate对象
    private JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
    
    // 1. 修改1号数据的 salary 为 10000
    @Test
    public void test1(){
    
        //2. 定义sql
        String sql = "update emp set salary = 10000 where id = 1001";
        //3. 执行sql
        int count = template.update(sql);
        System.out.println(count);
    }
    
    // 2. 添加一条记录
    @Test
    public void test2(){
        String sql = "insert into emp(id,ename,dept_id) values(?,?,?)";
        int count = template.update(sql, 1015, "郭靖", 10);
        System.out.println(count);
    
    }
    
    // 3.删除刚才添加的记录
    @Test
    public void test3(){
        String sql = "delete from emp where id = ?";
        int count = template.update(sql, 1015);
        System.out.println(count);
    }
    
    // 4.查询id为1001的记录，将其封装为Map集合
    // 注意：这个方法查询的结果集长度只能是1
    @Test
    public void test4(){
        String sql = "select * from emp where id = ? or id = ?";
        Map<String, Object> map = template.queryForMap(sql, 1001,1002);
        System.out.println(map);
        //{id=1001, ename=孙悟空, job_id=4, mgr=1004, joindate=2000-12-17, salary=10000.00, bonus=null, dept_id=20}
    
    }
    
    // 5. 查询所有记录，将其封装为List
    @Test
    public void test5(){
        String sql = "select * from emp";
        List<Map<String, Object>> list = template.queryForList(sql);
    
        for (Map<String, Object> stringObjectMap : list) {
            System.out.println(stringObjectMap);
        }
    }
    
    // 6. 查询所有记录，将其封装为Emp对象的List集合
    @Test
    public void test6(){
        String sql = "select * from emp";
        List<Emp> list = template.query(sql, new RowMapper<Emp>() {  // 自己实现RowMapper，不推荐！🍓
    
            @Override
            public Emp mapRow(ResultSet rs, int i) throws SQLException {
                Emp emp = new Emp();
                int id = rs.getInt("id");
                String ename = rs.getString("ename");
                int job_id = rs.getInt("job_id");
                int mgr = rs.getInt("mgr");
                Date joindate = rs.getDate("joindate");
                double salary = rs.getDouble("salary");
                double bonus = rs.getDouble("bonus");
                int dept_id = rs.getInt("dept_id");
    
                emp.setId(id);
                emp.setEname(ename);
                emp.setJob_id(job_id);
                emp.setMgr(mgr);
                emp.setJoindate(joindate);
                emp.setSalary(salary);
                emp.setBonus(bonus);
                emp.setDept_id(dept_id);
    
                return emp;
            }
        });
    
        for (Emp emp : list) {
            System.out.println(emp);
        }
    }
    
    // 6. 查询所有记录，将其封装为Emp对象的List集合
    @Test
    public void test6_2(){
        String sql = "select * from emp";
        List<Emp> list = template.query(sql, new BeanPropertyRowMapper<Emp>(Emp.class));
        for (Emp emp : list) {
            System.out.println(emp);
        }
    }
    
    //7. 查询总记录数
    @Test
    public void test7(){
        String sql = "select count(id) from emp";
        Long total = template.queryForObject(sql, Long.class);
        System.out.println(total);
    }

}

</code>
</pre>
</details>