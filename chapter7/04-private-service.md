## 第四节 私服 & 第三方 jar 包安装



### 4.1 私服的安装和启动

1）下载 `nexus-2.12.0-01-bundle.zip` 并解压到没有中文的目录下。进入 nexus-2.12.0-01/bin 文件夹，以管理员启动命令行，进入该目录：

```
安装：
nexus.bat install
启动：
nexus.bat start
卸载：
nexus.bat uninstall
```

2）JDK 9 会报错：`The nexus service was launched, but failed to start.`。修改 nexus-2.12.0-01\bin\jsw\conf 下 wrapper.conf 的 【wrapper.java.command=自己的JDK8安装目录\bin\java】

3）查看 nexus-2.12.0-01/conf 下 nexus.properties 配置文件。默认端口 8081。访问网址 `http://localhost:8081/nexus`。默认登录用户名 admin，密码 admin123。


<img src="./img7/14-nexus.png" width=800>



### 4.2 私服的应用




### 4.3 安装第三方 jar 包到本地仓库




### 4.4 安装第三方 jar 包到私服