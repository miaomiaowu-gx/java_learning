# 第三节 Mybatis入门案例中的设计模式分析

## 3.1 Mybatis 主函数分析

<img src="./img1/05-code-quick-start.png" width=800>



## 3.2 Mybatis 执行查询所有(findAll)分析


<img src="./img1/06-query-all-analyses.png" width=1400>



## 3.3 Mybatis 创建代理对象的分析

<img src="./img1/07-custom-mybatis-analysis.png" width=700>


## 3.4 不使用 Mybatis，自己定义实现上述过程

自定义 Mybatis 的分析：

* mybatis在使用代理 dao 的方式实现增删改查时，只做了两件事：

   * 第一：创建代理对象

   * 第二：在代理对象中调用selectList





