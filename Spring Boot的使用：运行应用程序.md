###### Spring Boot的使用：运行应用程序
将应用打包成jar，并使用内嵌HTTP服务器的一个最大的好处是，你可以像其他方式那样运行你的应用程序。调试Spring Boot应用也很简单，你不需要任何特殊IDE插件或扩展！
> 在这里只覆盖基于jar的打包，如果悬着将应用打包成war文件，你最好参考相关的服务器的IDE文档

###### 从IDE中运行
你可以从IDE中运行Spring Boot 应用，就像一个简单的java应用但首先需要导入项目。导入步骤取决于你的IDE和构建系统，大多数IDE是能够直接导入Maven项目，例如Eclipse用户可以从菜单中选择`File`菜单中的`Improt..`->`Existing Maven Project`。
如果不能直接将项目导入IDE，你可以使用构建系统生成IDE的源数据。Maven有针对Eclipse和IDEA的插件；Gradle为各种IDE提供插件。
> 如果意外的多次运行一个Web应用，你将看到一个“端口已被占用”的错误。STS用户可以使用`Relaunch`而不是`Run`按钮，以确保任何存在的实例是被关闭的。

###### 作为一个打包后的应用运行
如果使用Spring Boot Maven或Gradle插件创建一个可执行jar，你可以使用`java -jar`运行应用。例如：
```
D:\desktop\Spring Boot> java -jar .\target\myproject-0.0.1-SNAPSHOT.jar
<!--运行以后打开一个浏览器在地址栏上面输入localhost:8080即可-->
```
Spring Boot支持以远程调试模式运行一个打包的应用，下面的命令可以为应用关联一个调试器：
```
D:\desktop\Spring Boot> java -Xdebug -Xrunjdwp:server=y,transport=dt_socket,address=8000,suspend=n \
       -jar target/myapplication-0.0.1-SNAPSHOT.jar
```
###### 使用Maven插件运行
Spring Boot Maven插件包含一个`run`目标，可以用来快速编译和运行应用程序，并且跟IDE运行一样支持热加载。
```
D:\desktop\Spring Boot>mvn spring-boot:run
<!--运行以后打开一个浏览器在地址栏上面输入localhost:8080即可-->
```
你可能还想使用`MAVEN_OPTS`操作系统环境变量，如下所示：
```
export MAVEN_OPTS = -Xmx1024m
```
###### 使用Gradle插件运行
Spring Boot Gradle插件还包含一个`bootRun`可用于以爆炸形式运行应用程序的任务。`bootRun`每当您应用`org.springframework.boot`和`java`插件时都会添加该任务 ，如以下示例所示：
```
gradle bootRun
```
您可能还想使用`JAVA_OPTS`操作系统环境变量，如以下示例所示：
```
export JAVA_OPTS = -Xmx1024m
```