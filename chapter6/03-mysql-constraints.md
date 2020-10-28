## 第三节 MySQL 约束

概念：对表中的数据进行限定，保证数据的正确性、有效性和完整性。

分类：
1. 主键约束：primary key
2. 非空约束：not null
3. 唯一约束：unique
4. 外键约束：foreign key

### **非空约束**：not null，某一列的值不能为null。

1. 创建表时添加约束
      ~~~
      CREATE TABLE stu(
            id INT,
            NAME VARCHAR(20) NOT NULL -- name为非空
      );
      ~~~

2. 创建表完后，添加非空约束
`ALTER TABLE stu MODIFY NAME VARCHAR(20) NOT NULL;`

3. 删除name的非空约束
`ALTER TABLE stu MODIFY NAME VARCHAR(20);`

***


### **唯一约束**：unique，某一列的值不能重复。

1. 注意：唯一约束可以有NULL值，但是只能有一条记录为null。

2. 在创建表时，添加唯一约束
      ~~~
      CREATE TABLE stu(
            id INT,
            phone_number VARCHAR(20) UNIQUE -- 手机号
      );
      ~~~

3. 删除唯一约束(与非空不同，记住！)
ALTER TABLE stu DROP INDEX phone_number;

4. 在表创建完后，添加唯一约束
`ALTER TABLE stu MODIFY phone_number VARCHAR(20) UNIQUE;`


***

### **主键约束**：primary key。

1. 介绍
      * 含义：非空且唯一
      * 一张表<font color=#ff8918>**只能有一个字段**</font>为主键
      * 主键就是表中记录的<font color=#ff8918>**唯一标识**</font>

2. 在创建表时，添加主键约束
      ~~~
      create table stu(
            id int primary key,-- 给id添加主键约束
            name varchar(20)
      );
      ~~~

3. 删除主键
      ~~~
      -- 错误 alter table stu modify id int ;
      ALTER TABLE stu DROP PRIMARY KEY;
      ~~~

4. 创建完表后，添加主键
      `ALTER TABLE stu MODIFY id INT PRIMARY KEY;`

5. 自动增长：如果某一列是<font color=#ff8918>**数值类型**</font>的，使用 auto_increment 可以来完成值得自动增长。

      * 在创建表时，添加主键约束，并且完成主键自增长
      ~~~
      create table stu(
            id int primary key auto_increment,-- 给id添加主键约束
            name varchar(20)
      );
      ~~~
      注：使用时，可以输入NULL让其自动增长，也可以输入指定值（输入的值可以是任意的，无需按照递增顺序输入）。自动增长时，数据只跟上一条有关，即按照上一条数据的id增加1。

      * 删除自动增长
      `ALTER TABLE stu MODIFY id INT;`

      * 添加自动增长
      `ALTER TABLE stu MODIFY id INT AUTO_INCREMENT;`

      * 默认地 AUTO_INCREMENT 的开始值是 1，如果希望修改起始值,请使用下列 SQL 语法
      ~~~
      方式一：创建表时
      CREATE TABLE 表名(
            列名 int primary key AUTO
      ) AUTO_INCREMENT=起始值;

      方式二：创建表以后修改起始值
      ALTER TABLE 表名 AUTO_INCREMENT=起始值;
      ~~~

      * DELETE 和 TRUNCATE 对自增长的影响
      DELETE： 删除所有的记录之后，自增长没有影响。
      <img src="https://img2020.cnblogs.com/blog/2051825/202007/2051825-20200718114456915-601081908.png" width=200>
      TRUNCATE：删除以后，自增长又重新开始。
      <img src="https://img2020.cnblogs.com/blog/2051825/202007/2051825-20200718114608499-1294098109.png" width=200>

6. 哪个字段应该作为表的主键？

      通常<font color=#ff8918>**不用业务字段作为主键**</font>，单独给每张表设计一个 id 的字段，把 id 作为主键。 主键是给数据库和程序使用的，不是给最终的客户使用的。所以主键有没有含义没有关系，只要不重复，非空就行。<font color=#ff8918>**如：身份证，学号不建议做成主键**</font>。

7. 注意

      * 主键数在一个表中，只能有一个。不能出现多个主键。主键可以单列，也可以是多列。

      * 自增长只能用在主键上。

***

### **外键约束**：foreign key，让表于表产生关系，从而保证数据的正确性。

<img src="https://img2020.cnblogs.com/blog/2051825/202007/2051825-20200718113503849-1132802013.png" width=400>

上表数据存在冗余，且修改不方便，如研发部由广州迁移到深圳，需要修改多条数据。

 <details>
<summary>员工表</summary>
<pre>
<code>
CREATE TABLE emp (
id INT PRIMARY KEY AUTO_INCREMENT,
NAME VARCHAR(30),
age INT,
dep_name VARCHAR(30),
dep_location VARCHAR(30)
);
-- 添加数据
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('张三', 20, '研发部', '广州');
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('李四', 21, '研发部', '广州');
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('王五', 20, '研发部', '广州');
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('老王', 20, '销售部', '深圳');
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('大王', 22, '销售部', '深圳');
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('小王', 18, '销售部', '深圳');
</code>
</pre>
</details>

解决：拆分表，部门表与员工表

 <details>
<summary>拆分表</summary>
<pre>
<code>
-- 解决方案：分成 2 张表

-- 创建部门表(id,dep_name,dep_location)
-- 一方，【主表】
create table department(
id int primary key auto_increment,
dep_name varchar(20),
dep_location varchar(20)
);

-- 创建员工表(id,name,age,dep_id)
-- 多方，【从表】
create table employee(
id int primary key auto_increment,
name varchar(20),
age int,
dep_id int -- 外键对应主表的主键
)

-- 添加 2 个部门
insert into department values(null, '研发部','广州'),(null, '销售部', '深圳');
select * from department;

-- 添加员工,dep_id 表示员工所在的部门
INSERT INTO employee (NAME, age, dep_id) VALUES ('张三', 20, 1);
INSERT INTO employee (NAME, age, dep_id) VALUES ('李四', 21, 1);
INSERT INTO employee (NAME, age, dep_id) VALUES ('王五', 20, 1);
INSERT INTO employee (NAME, age, dep_id) VALUES ('老王', 20, 2);
INSERT INTO employee (NAME, age, dep_id) VALUES ('大王', 22, 2);
INSERT INTO employee (NAME, age, dep_id) VALUES ('小王', 18, 2);
select * from employee;
</code>
</pre>
</details>

<img src="https://img2020.cnblogs.com/blog/2051825/202007/2051825-20200718122109759-1270732405.png" width=300>

<img src="https://img2020.cnblogs.com/blog/2051825/202007/2051825-20200718122159103-893479506.png" width=300>

问题：当我们在 employee 的 dep_id 里面输入不存在的部门，数据依然可以添加，但是并没有对应的部门，实际应用中不能出现这种情况。employee 的 dep_id 中的数据只能是 department 表中存在的 id。

解决方式: 使用外键约束。

<img src="https://img2020.cnblogs.com/blog/2051825/202007/2051825-20200718132832285-1476986966.png" width=500>


1. 在创建表时，可以添加外键
      ~~~
      create table 表名(
            ....
            外键列, 
            constraint 外键名称(自己命名) foreign key (外键列名称) references 主表名称(主表列名称)
      );

      如：
      create table employee(
            id int primary key auto_increment,
            name varchar(20),
            age int,
            dep_id int, -- 外键对应主表的主键，由于添加了外键约束，此处要加逗号
            -- 创建外键约束，emp_depid_fk 是自己起的外键约束名称，dep_id为从表的列，id为主表的主键。
            constraint emp_depid_fk foreign key (dep_id) references department(id)
      )
      ~~~

2. 删除外键
      `ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;`

3. 创建表之后，添加外键
      `ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);`

4. 外键值可以为 NULL，但是不能是不存在的外键值。

5. 级联操作：在修改和删除主表的主键时，同时更新副表的外键值或删除副表的外键值所在行，称为级联操作。

      设置外键约束后，若想修改主表关联的列（如上例中的id值），会报错，因为从表引用了该值。

      解决方法：
            * 将从表引用的值，设置为 NULL，然后修改主表对应的值，最后重新将从表 NULL值设置为主表修改后的值。
            * 使用级联

      1）添加级联操作
      同时设置级联更新与级联删除：
      `ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称) ON UPDATE CASCADE ON DELETE CASCADE;`

      2）分类：
            * 级联更新：`ON UPDATE CASCADE`。更新主表关联值时，从表对应的引用会更新。
            * 级联删除：ON DELETE CASCADE。删除主表关联值时，从表对应的引用【行】会被删除。整条数据被删除，而不是只删除对应的值。

