## 第三节 Dubbo 入门


### 3.1 环境搭建

#### 3.1.1 ZooKeeper 注册中心

##### 安装
 
1 在官网下载 Zookeeper 压缩包，解压。

2 在 zookeeper-x.x.xx 文件夹的 conf 文件夹下，复制 zoo_sample.cfg 文件并重命名为 zoo.cfg。

3 在 zookeeper-x.x.xx 文件夹下创建 data 文件夹，修改 zoo.cfg 配置：

```cfg
dataDir=../data
```

4 运行 bin 文件夹下的 zkServer.cmd 启动服务端，zkCli.cmd 启动客户端。


##### 使用

&emsp;&emsp;Zookeeper 是 Apache Hadoop 的子项目，是一个**树型的目录服务**，支持**变更推送**，适合作为 Dubbo 服务的注册中心，工业强度较高，可用于生产环境，并推荐使用。

<img src="./img9/09-zookeeper.jpg" width=400>


**流程说明**：

* **服务提供者启动时**: 向 `/dubbo/com.foo.BarService/providers` 目录下写入自己的 URL 地址

* **服务消费者启动时**: 订阅 `/dubbo/com.foo.BarService/providers` 目录下的提供者 URL 地址。并向 `/dubbo/com.foo.BarService/consumers` 目录下写入自己的 URL 地址

* **监控中心启动时**: 订阅 `/dubbo/com.foo.BarService` 目录下的所有提供者和消费者 URL 地址。



**客户端操作**：

* 获取根节点下的值 `get /`

```cmd
cZxid = 0x0
ctime = Thu Jan 01 08:00:00 CST 1970
mZxid = 0x0
mtime = Thu Jan 01 08:00:00 CST 1970
pZxid = 0x0
cversion = -1
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 0
numChildren = 1
```

* 获取节点 `ls /`

```
[zookeeper]
```

* 创建节点并设置值 `create -e /atguigu 123456`，然后查看节点 `ls /`

```
[zookeeper, atguigu]
```

* 获取创建节点的值 `get /atguigu`

```
123456
cZxid = 0x2
ctime = Fri Dec 04 15:36:31 CST 2020
mZxid = 0x2
mtime = Fri Dec 04 15:36:31 CST 2020
pZxid = 0x2
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x10010f255ab0000
dataLength = 6
numChildren = 0
```



#### 3.1.2 搭建管理控制台(监控中心 Monitor)  

【待解决】

1 下载 dubbo-admin 对应的 zip 包。github: https://github.com/apache/dubbo-admin/tree/master。

2 解压后进入目录修改指定 zookeeper 地址

* 查看 dubbo-admin-master\dubbo-admin\src\main\resources\application.properties 文件。

* 将 zookeeper 的监控中心的地址配置为本地端口，`dubbo.registry.address=zookeeper://127.0.0.1:2181`。

3 在dubo-zookeeper\dubbo-admin-master\dubbo-admin文件夹下 cmd 打包，输入 `mvn clean package`。

4 在生成的 target 目录下，找到 jar 包。命令行 `java -jar dubbo-admin-0.0.1-SNAPSHOT.jar`。

5 浏览器 localhost:7001 访问，默认情况下，用户与密码均是 root。



查看端口占用情况

```
netstat -ano|findstr "7001"
```



### 3.2 创建提供者消费者工程




### 3.3 服务提供者配置&测试





### 3.4 服务消费者配置&测试


监控中心 Simple Monitor安装配置

