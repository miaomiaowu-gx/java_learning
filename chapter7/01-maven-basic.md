## 第一节 Maven 基础

### 1.1 Maven 常用命令

* `mvn compile` 是 maven 工程的编译命令，作用是将 src/main/java 下的文件编译为 class 文件输出到 target 目录下。

* `mvn test` 是 maven 工程的测试命令，会执行 src/test/java 下的单元测试类。同时也会将 src/main/java 下的文件编译为 class 文件输出到 target 目录下。

* `mvn clean` 是 maven 工程的清理命令，执行 clean 会删除 target 目录及内容。

* `mvn package` 是 maven 工程的打包命令，对于 java 工程执行 package 打成 jar 包，对于 web 工程打成 war包。

* `mvn install` 是 maven 工程的安装命令，执行 install 将 maven 打成 jar 包或 war 包发布到本地仓库。


> 从运行结果中，可以看出：当后面的命令执行时，前面的操作过程也都会自动执行。
> 如执行 test 命令时，compile 命令也会执行；执行 package 命令时，compile 与 test 命令都会执行。

#### 1.1.1 声明周期

Maven 对项目构建过程分为三套相互独立的生命周期，分别是：
* Clean Lifecycle 在进行真正的构建之前进行一些清理工作。
* Default Lifecycle 构建的核心部分，编译，测试，打包，部署等等。
* Site Lifecycle 生成项目报告，站点，发布站点。

<img src="./img7/03-maven-life-cycle.png" width=400>

#### 1.1.2 Maven 概念模型 

<img src="./img7/04-conceptual-model.png" width=600>


### 1.2 IDEA 开发 maven 项目

#### 1.2.1 使用骨架创建 maven 的 java 工程

1 打开 IDEA，选择 Create New Project。

2 按下图选择 archetype。

<img src="./img7/05-maven-java-pro.png" width=400>

3 填写 GroupId（公司或组织名称） 与 ArtifactId（当前项目名称）。

4 在 main 文件夹下创建 resources 目录，右键选择 Mark Directory as，选择 Resources Root。

5 在 test 文件夹下创建 resources 目录，右键选择 Mark Directory as，选择 Test Resources Root。


#### 1.2.2 不使用骨架创建 maven 的 java 工程

&emsp;&emsp;选择 Maven 工程后，不勾选 Create from archetype，直接点击 Next。

推荐使用此方式创建 java 工程，创建的文件结构更清晰。


#### 1.2.3 使用骨架创建 maven 的 web 工程

1）创建 Maven 工程时选择如下 archetype：

<img src="./img7/06-maven-webapp.png" width=450>

2）补全目录：main 下创建 java，右键选择 Mark Directory as，选择 Sources Root。

#### 1.2.4 指定 web 资源包 

<img src="./img7/07-web-path.png" width=950>

   

#### 1.2.5 
    
### 1.3 




### 1.4 







