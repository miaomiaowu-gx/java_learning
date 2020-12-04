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






#### 3.1.2 管理控制台  


创建提供者消费者工程

服务提供者配置&测试

服务消费者配置&测试


监控中心 Simple Monitor安装配置

