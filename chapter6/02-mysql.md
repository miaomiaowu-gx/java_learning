# 第二节 MySQL 基础

## 一、前期准备

[详见](https://www.cnblogs.com/miaomiaowu/p/13181595.html#autoid-h2-3-0-0)

### MySQL配置及使用

安装MySQL实际上安装的是MySQL服务，安装MySQL服务后，其会在Windows的服务列表里注册一个MySQL服务。

什么是服务？一些没有界面的应用程序。

1. MySQL服务启动

      1. 手动 win10:右键任务管理器->服务->MySQL(点击启动、关闭)。

      2. cmd--> services.msc  打开服务的窗口->MySQL(点击启动、关闭)。

      3. 使用【管理员权限】打开cmd。
         - `net start mysql`: 启动mysql的服务
         - `net stop mysql`:关闭mysql服务

2. MySQL登录

      1. `mysql -uroot -p密码` 或 `mysql -uroot -p`回车后再输入密码。默认连接本地MySQL用户。

      2. `mysql -hip -uroot -p连接目标的密码`，如连接本机`mysql -h127.0.0.1 -uroot -p密码`

      3. `mysql --host=ip --user=root --password=连接目标的密码`

3. MySQL退出

      1. exit

      2. quit


4. MySQL目录结构

      1. MySQL安装目录
         - 配置文件 my.ini

      2. MySQL数据目录（默认在C盘）：datadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"
      <img src="https://img2020.cnblogs.com/blog/2051825/202007/2051825-20200703002216759-1973715416.bmp" width=600>

            * 数据库：文件夹

            * 表：文件

            * 数据：数据