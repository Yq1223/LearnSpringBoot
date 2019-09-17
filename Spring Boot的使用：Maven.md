###### Spring Boot的会使用：Maven
Maven用户可以从`spring-boot-starter-parent`项目中集成已获得合理的默认值。父项目提供一下功能：
- Java 1.8作为默认的编译器级别。
- UTF-8源码编码。
- 继承自spring-boot-dependencies pom的依赖关系管理部分，用于管理公共依赖关系的版本。此依赖关系管理允许你在自己的pom中使用时省略这些依赖项<version>标记。
- 明智的资源过滤。
- 明智的插件配置（exec plugin，Git commit ID，and shade）。
- 明智的资源过滤`application.properties`和`application.yml`包括配置文件特定的文件（例如`application-dev.properties`和`application-dev.yml`）

请注意，由于`application.properties`和`application.yml`文件接收Spring样式占位符（${...}），Maven过滤被更改为使用@..@占位符。（你可以通过设置一个叫做Maven的属性来覆盖它`resource.delimiter`）
###### 继承初始父项
要配置你的项目从中继承`spring-boot-starter-parent`，请设置`parent`如下：
```
<!-- Inherit defaults from Spring Boot -->
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-parent</artifactId>
	<version>2.1.3.RELEASE</version>
</parent>
```
> 注：你应该只需要在该依赖上指定Spring Boot版本，如果导入其他的starters，放心的省略版本号就好了。

按照以上设置，你可以在自己的项目中通过覆盖属性来覆盖个别依赖。例如，要升级到另一个Spring Data发行版，你需要讲一下内容添加到你的`pom.xml`中去：
```
<properties>
    <spring-data-releasetrain.version>Fowler-SR2</spring-data-releasetrain.version>
</properties>
```
> 检查`spring-boot-dependencies`pom以获取支持的属性列表。

###### 在没有父POM的情况下使用Spring Boot
不是每一个人都喜欢从`spring-boot-starter-parent`POM中去继承。你可能拥有自己的公司标准parent，或者你可能更愿意明确声明所有的Maven配置。
如果你不想使用它`spring-boot-starter-parent`,你仍然可以通过使用`scope=import`依赖项来保持依赖项管理（但不是插件管理）的好处，如下所示：
```
<dependencyManagement>
	<dependencies>
		<dependency>
			<!-- Import dependency management from Spring Boot -->
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-dependencies</artifactId>
			<version>2.1.3.RELEASE</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
	</dependencies>
</dependencyManagement>
```
如上所述，前面的示例设置不允许你使用属性覆盖单个依赖项，要获得相同的结果，你需要在输入之前在`dependencyManagement`项目中添加一个条目。例如，要升级到另一个Spring Data版本系列，你可以将一下元素添加到：`spring-boot-dependencies` `pom.xml`
```
<dependencyManagement>
	<dependencies>
		<!-- Override Spring Data release train provided by Spring Boot -->
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-releasetrain</artifactId>
			<version>Fowler-SR2</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-dependencies</artifactId>
			<version>2.1.3.RELEASE</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
	</dependencies>
</dependencyManagement>
```
> 在前面的实例中，我们制定了BOM，但是可以以相同的方式覆盖任何依赖关系类型。

###### 使用Spring Boot Maven插件
Spring Boot包含一个Maven插件，可以将项目打包为可执行的jar文件。如果想使用它，你可以将插件添加到`plugins`节点处：
```
<build>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
		</plugin>
	</plugins>
</build>
```
> 如果使用Spring Boot启动程序父POM，则只需添加插件。除非你要更改父级中定义的设置，否则无需对其进行配置。