# 第四节 自定义 Mybatis 小结


## 4.1 自定义 Mybatis 流程分析

<img src="./img1/08-user-defined-mybatis-development-flow-chart.png" width=1000>



## 4.2 回顾 Mybatis 环境搭建-实现查询所有功能


### 4.2.1 创建的类 User 为什么要实现序列化（Serializable）接口

java 中类实现 Serializable 接口的原因：

* 一个 java 中的类只有实现了 Serializable 接口，它的对象才是**可序列化**的。如果要序列化某些类的对象，这些类就必须实现 Serializable 接口。Serializable是一个空接口，没有什么具体内容，它的目的只是简单的标识一个类的对象可以被序列化。

* 实现 Serializable 接口：为了保存在内存中的各种对象的状态（也就是实例变量，不是方法），并且可以把保存的对象状态再读出来，这是 java 中的提供的**保存对象状态的机制—序列化**。


在什么情况下需要使用到 Serializable 接口呢？

　　1、当想把的内存中的对象状态保存到一个文件中或者数据库中时候；

　　2、当想用套接字在网络上传送对象的时候；

　　3、当想通过RMI传输对象的时候；

