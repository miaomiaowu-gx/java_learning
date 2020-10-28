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

### 二、


### 三、



### 四、




### 五、