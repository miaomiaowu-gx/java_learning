## 第二节 MySQL 基础

### 一、前期准备

[详见](https://www.cnblogs.com/miaomiaowu/p/13181595.html#autoid-h2-3-0-0)

#### MySQL配置及使用

安装 MySQL 实际上安装的是 MySQL 服务，安装 MySQL 服务后，其会在 Windows 的服务列表里注册一个 MySQL 服务。

什么是服务？一些没有界面的应用程序。

1. MySQL 服务启动

      1. 手动 win10 :右键任务管理器->服务->MySQL (点击启动、关闭)。

      2. cmd--> services.msc  打开服务的窗口->MySQL (点击启动、关闭)。

      3. 使用【管理员权限】打开 cmd。
         - `net start mysql`: 启动 mysql 的服务
         - `net stop mysql`:关闭 mysql 服务

2. MySQL 登录（[] 内的内容根据自己的设置替换）

      1. `mysql -u[root] -p[密码]` 或 `mysql -u[root] -p` 回车后再输入密码。默认连接本地MySQL用户。

      2. `mysql -h[ip] -u[root] -p[连接目标的密码]`，如连接本机 `mysql -h[127.0.0.1] -u[root] -p[密码]`

      3. `mysql --host=[ip] --user=[root] --password=[连接目标的密码]`

3. MySQL 退出

      1. exit

      2. quit


4. MySQL 目录结构

      1. MySQL 安装目录
         - 配置文件 my.ini

      2. MySQL 数据目录（默认在C盘）：datadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"
     <img src="./img6/03-mysql-database-structure.png" width=600>
     
   * 数据库：文件夹
     
   * 表：文件
     
   * 数据：数据

### 二、SQL

#### 2.1 什么是SQL？

Structured Query Language：结构化查询语言

定义了操作所有<font color=#ff8918>**关系型数据库**</font>的规则。每一种数据库操作的方式存在不一样的地方，称为“方言”。
#### 2.2 SQL 通用语法

1) SQL 语句可以**单行或多行**书写，以<font color=#ff8918>**分号结尾**</font>。

2) 可使用**空格和缩进**来增强语句的可读性。

3) MySQL 数据库的 SQL 语句<font color=#ff8918>**不区分大小写**</font>（windows系统下），关键字**建议**使用大写。

4) 3 种注释
      单行注释: `--【空格】注释内容`（--后一定要有空格） 或 `# 注释内容` (mysql 特有，#号之后可以没有空格) 
      多行注释: `/* 注释 */`		

#### 2.3 SQL分类

1) DDL (Data Definition Language) 数据定义语言
    用来定义数据库对象：**数据库，表，列**等。关键字：create, drop, alter 等

2) DML (Data Manipulation Language) 数据操作语言
    用来对数据库中**表**的数据进行增删改。关键字：insert, delete, update 等

3) DQL (Data Query Language) 数据查询语言
    用来查询数据库中**表**的记录(数据)。关键字：select, where 等

4) DCL (Data Control Language) 数据控制语言(了解)
    用来定义数据库的访问权限和安全级别，及创建用户。关键字：GRANT， REVOKE 等


### 三、



### 四、




### 五、