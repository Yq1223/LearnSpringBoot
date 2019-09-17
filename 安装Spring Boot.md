###### 安装Spring Boot
Spring Boot可以与经典Java开发工具一起使用，也可以作为命令行工具安装。五路那种方式，您都需要Java SDK v1.8或更高版本。在开始之前，你需要使用以下命令检查当前的Java安装：
```
java -version
```
**注意：**如果你不熟悉Java开发，或者想要尝试Spring Boot，则可能需要先尝试Spring Boot CLI（命令行界面）。否则，请继续阅读经典安装说明。
###### Java DeveLoper的安装说明
你可以像使用任何标准的Java库一样使用Spring Boot。为此，请`spring-boot-*.jar`在类路径中包含相应的文件。Spring Boot不需要任何特殊工具集成，因此你可以使用任何IDE或文本编辑器。此外，Spring Boot应用程序没有什么特别之处，因此您可以像运行任何其他Java程序一样运行和调试Spring Boot应用程序。
虽然你可以复制Spring Boot jar，但我们通常建议你使用支持依赖关系管理的构建工具（例如Maven或Gradle）。
###### Maven的安装
Spring Boot与Apache Maven 3.3或高版本兼容。
> 在许多操作系统上，Maven可以与软件包管理器一起安装。如果你使用OSX Homebrew，请尝试`brew install maven`。Ubuntu用可以运行`sudo apt-get install maven`。使用Chocolatey的windows用户可以choco installmaven从提升（管理员）提示符运行。

Spring Boot依赖项使用`org.springframework.boot` `groupId`。通常,你的Maven POM文件继承自`spring-boot-starter-parent`项目并声明对一个或者多个<font color=0099ff>“Starters”</font>的依赖关系。Sproing Boot还提供了一个可选的<font color=0099ff>Maven插件</font>来创建可执行jar。
以下清单显示了一个典型的pom.xml文件：
```
<？xml version =“1.0”encoding =“UTF-8”？> 
<project  xmlns = “http://maven.apache.org/POM/4.0.0”  xmlns：xsi = “http：//www.w3 .org / 2001 / XMLSchema-instance“ 
	xsi：schemaLocation = ”http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd“ > 
	<modelVersion> 4.0.0 </ modelVersion>
	<!--模型版本-->
	
	<groupId> com.example </ groupId> 
	<!--公司或者组织的唯一你标识，并且在配置时生成的路径也是由此生成。-->
	<artifactId> myproject </ artifactId>
	<!--项目的唯一ID，一个groupId下面可能有多个项目，就是靠artifactId来区分的-->
	<version> 0.0.1-SNAPSHOT </ version>
	<!--版本号-->

	<！ - 继承默认值为Spring Boot  - > 
	<parent> 
	<!--父项目的坐标-->
		<groupId> org.springframework.boot </ groupId> 
		<!--被继承的父项目的构件标识符-->
		<artifactId> spring-boot-starter-parent </ artifactId> 
		<!--被继承的父项目的全球唯一标识符-->
		<version> 2.1.3.RELEASE < / version> 
		<!--被继承的父项目的版本-->
	</ parent>

	<！ - 添加Web应用程序的典型依赖项 - > 
	<dependencies> 
	<!--项目引入插件所需要的额外依赖-->
		<dependency> 
			<groupId> org.springframework.boot </ groupId> 
			<!--依赖的group Id-->
			<artifactId> spring-boot-starter-web </ artifactId> 
			<!--依赖的artifact Id-->
		</ dependency> 
	</依赖>

	<！ - 打包为可执行jar  - > 
	<build> 
		<plugins> 
		<!--使用的插件列表-->
			<plugin> 
			<!--plugin元素包含描述插件所需要的信息-->
				<groupId> org.springframework.boot </ groupId> 
				<!--插件在仓库中的groupId-->
				<artifactId> spring-boot-maven-plugin </ artifactId> 
				<!--插件在仓库中的artifactId-->
			</ plugin > 
		</ plugins> 
	</ build>

</项目>
```
> 这 `spring-boot-starter-parent`是使用Spring的好方法，但它可能并不适合所有时间。有时你可能需要从不同的父POM继承，或者你可能不喜欢我们的默认设置。这些情况请参见“使用没有父POM的Spring Boot”来获得使用`import`范围的替代解决方案。

###### Gradle安装
Spring Boot与Gradle 4.4及更高版本兼容。如果你尚未安装Gradle，则可以按照gradle.org上的说明进行操作。
可以使用`org.springframework.boot` `group`。声明Spring Boot依赖项。通常，你的项目会声明对一个或者多个“Starters”的依赖关系。Spring Boot提供了一个有用的Gradle插件，可用于简化依赖声明和创建可执行jar
```
Gradle Wrapper
当你需要构建项目时，Gradle wrapper提供了一种“获取”Gradle的好方法。他是一个小脚本和库，你可以与代码一起提交以引导构建过程。[详细信息请参阅](https://docs.gradle.org/4.2.1/userguide/gradle_wrapper.html)
```
###### 安装Spring Boot CLI
Spring Boot CLI（命令行界面）是一个命令行工具，你可以使用他来快速使用Spring进行原型设计。它允许你运行groovy脚本，这意味着你拥有熟悉的类似Java的语法，而没有太过的样板代码。
你不需要使用CLI来使用Spring Boot，但它绝对是实现Spring应用程序的最快的方法。
###### 手动安装
你可以从Spring软件库下载Spring CLI发行版：
- 下载地址1：https://repo.spring.io/release/org/springframework/boot/spring-boot-cli/2.1.3.RELEASE/spring-boot-cli-2.1.3.RELEASE-bin.tar.gz
- 下载地址2：https://repo.spring.io/release/org/springframework/boot/spring-boot-cli/2.1.3.RELEASE/spring-boot-cli-2.1.3.RELEASE-bin.zip

下载完成后，请按照解压缩的存档中的INSTALL.txt说明进行操作。总之，文件中的目录中有一个`spring`脚本（`spring.bat`用于Windows）。或者，你可以使用该文件（该脚本可帮助你正确的设置类路径）。`bin/` `.zip` `java -jar` `.jar`

详细信息请看：https://docs.spring.io/spring-boot/docs/2.1.3.RELEASE/reference/html/getting-started-installing-spring-boot.html