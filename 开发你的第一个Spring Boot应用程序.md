###### 开发你的第一个Spring Boot应用程序
在这里将为你介绍如何开发一个简单的"Hello World!"Web应用程序，该应用程序突出了Spring Boot的一些主要功能。我们使用Maven来构建这个项目，因为大多数IDE都支持他。
> 该spring.io网站包含了许多“入门” 指南使用Spring的引导。如果您需要解决特定问题，请先检查一下。
> 您可以通过转到start.spring.io并从依赖关系搜索器中选择“Web”启动器来快捷执行以下步骤。这样做会生成一个新的项目结构，以便您可以立即开始编码。有关更多详细信息，请查看Spring Initializr文档。

在开始之前，打开终端运行下面命令以确保安装了有效的Java和Maven版本：
```
C:\Windows\System32>java -version
java version "1.8.0_191"
Java(TM) SE Runtime Environment (build 1.8.0_191-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.191-b12, mixed mode)
```
```
C:\Windows\System32>mvn -v
Apache Maven 3.6.2 (40f52333136460af0dc0d7232c0dc0bcf0d9e117; 2019-08-27T23:06:16+08:00)
Maven home: D:\Maven\apache-maven-3.6.2\bin\..
Java version: 1.8.0_191, vendor: Oracle Corporation, runtime: C:\Program Files\Java\jdk1.8.0_191\jre
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
```
> 注意：此实例需要在其自己的文件夹中创建。后续说明假定你已经创建了一个合适的文件夹，并且它是你当前的目录。

###### 创建POM
我们需要从创建Maven `pom.xml`文件开始。
本`pom.xml`是用来构建项目的配方。打开你喜欢的文本编辑器添加以下内容
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>myproject</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.3.RELEASE</version>
    </parent>

    <!-- Additional lines to be added here... -->
    <!--这里添加其他的行-->

</project>
```
上面的清单应该为你提供有效的构建。你可以通过运行`mvn package`来测试它（现在，你可以忽略jar的警告）
> 此时，你可以将项目导入IDE（大多数现代Java IDE包括对Maven的内置支持）。为了简单起见，我们继续为此实例使用纯文本编辑器。

###### 添加类路径依赖关系
Spring Boot提供了许多Startes，可以添加jar添加到类路径中。我们的实例应用程序已经`spring-boot-starter-parent`在`parent`POM部分中使用过。这`spring-boot-starter-parent`是一个特殊的启动器，提供有用的Maven默认值。它还提供了一个`dependency-management`部分，以便你可以省略子模块依赖关系的版本标签。
其他Starter提供了在开发特定类型的应用程序时可能需要的依赖关系。由于我们正在开发一个Web应用程序，我们添加一个`spring-boot-starter-web`依赖项。在此前，我们可以通过运行以下命令来查看我们目前的功能：
```
D:\desktop\Spring Boot>mvn dependency:tree
[INFO] Scanning for projects...
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-clean-plu
gin/3.1.0/maven-clean-plugin-3.1.0.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-clean-plug
in/3.1.0/maven-clean-plugin-3.1.0.pom (5.2 kB at 4.2 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-clean-plu
gin/3.1.0/maven-clean-plugin-3.1.0.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-clean-plug
in/3.1.0/maven-clean-plugin-3.1.0.jar (30 kB at 51 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-install-p
lugin/2.5.2/maven-install-plugin-2.5.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-install-pl
ugin/2.5.2/maven-install-plugin-2.5.2.pom (6.4 kB at 16 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-plugins/2
5/maven-plugins-25.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-plugins/25
/maven-plugins-25.pom (9.6 kB at 22 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-parent/24/maven-p
arent-24.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-parent/24/maven-pa
rent-24.pom (37 kB at 83 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/apache/14/apache-14.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/apache/14/apache-14.pom (15 kB
 at 35 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-install-p
lugin/2.5.2/maven-install-plugin-2.5.2.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-install-pl
ugin/2.5.2/maven-install-plugin-2.5.2.jar (33 kB at 74 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-deploy-pl
ugin/2.8.2/maven-deploy-plugin-2.8.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-deploy-plu
gin/2.8.2/maven-deploy-plugin-2.8.2.pom (7.1 kB at 18 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-deploy-pl
ugin/2.8.2/maven-deploy-plugin-2.8.2.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-deploy-plu
gin/2.8.2/maven-deploy-plugin-2.8.2.jar (34 kB at 75 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-site-plug
in/3.7.1/maven-site-plugin-3.7.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-site-plugi
n/3.7.1/maven-site-plugin-3.7.1.pom (19 kB at 46 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-site-plug
in/3.7.1/maven-site-plugin-3.7.1.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-site-plugi
n/3.7.1/maven-site-plugin-3.7.1.jar (135 kB at 213 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/johnzon/johnzon-maven-plugin/
1.1.11/johnzon-maven-plugin-1.1.11.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/johnzon/johnzon-maven-plugin/1
.1.11/johnzon-maven-plugin-1.1.11.pom (3.6 kB at 8.5 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/johnzon/johnzon/1.1.11/johnzo
n-1.1.11.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/johnzon/johnzon/1.1.11/johnzon
-1.1.11.pom (25 kB at 57 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/johnzon/johnzon-maven-plugin/
1.1.11/johnzon-maven-plugin-1.1.11.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/johnzon/johnzon-maven-plugin/1
.1.11/johnzon-maven-plugin-1.1.11.jar (26 kB at 58 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/jetbrains/kotlin/kotlin-maven-plugin
/1.2.71/kotlin-maven-plugin-1.2.71.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/jetbrains/kotlin/kotlin-maven-plugin/
1.2.71/kotlin-maven-plugin-1.2.71.pom (6.1 kB at 13 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/jetbrains/kotlin/kotlin-project/1.2.
71/kotlin-project-1.2.71.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/jetbrains/kotlin/kotlin-project/1.2.7
1/kotlin-project-1.2.71.pom (10 kB at 25 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/jetbrains/kotlin/kotlin-maven-plugin
/1.2.71/kotlin-maven-plugin-1.2.71.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/jetbrains/kotlin/kotlin-maven-plugin/
1.2.71/kotlin-maven-plugin-1.2.71.jar (79 kB at 172 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/jooq/jooq-codegen-maven/3.11.9/jooq-
codegen-maven-3.11.9.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/jooq/jooq-codegen-maven/3.11.9/jooq-c
odegen-maven-3.11.9.pom (3.5 kB at 8.5 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/jooq/jooq-parent/3.11.9/jooq-parent-
3.11.9.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/jooq/jooq-parent/3.11.9/jooq-parent-3
.11.9.pom (11 kB at 26 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/jooq/jooq-codegen-maven/3.11.9/jooq-
codegen-maven-3.11.9.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/jooq/jooq-codegen-maven/3.11.9/jooq-c
odegen-maven-3.11.9.jar (16 kB at 38 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/springframework/boot/spring-boot-mav
en-plugin/2.1.3.RELEASE/spring-boot-maven-plugin-2.1.3.RELEASE.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/springframework/boot/spring-boot-mave
n-plugin/2.1.3.RELEASE/spring-boot-maven-plugin-2.1.3.RELEASE.pom (4.8 kB at 8.6 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/springframework/boot/spring-boot-too
ls/2.1.3.RELEASE/spring-boot-tools-2.1.3.RELEASE.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/springframework/boot/spring-boot-tool
s/2.1.3.RELEASE/spring-boot-tools-2.1.3.RELEASE.pom (1.8 kB at 4.4 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/springframework/boot/spring-boot-par
ent/2.1.3.RELEASE/spring-boot-parent-2.1.3.RELEASE.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/springframework/boot/spring-boot-pare
nt/2.1.3.RELEASE/spring-boot-parent-2.1.3.RELEASE.pom (1.8 kB at 4.3 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/springframework/boot/spring-boot-mav
en-plugin/2.1.3.RELEASE/spring-boot-maven-plugin-2.1.3.RELEASE.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/springframework/boot/spring-boot-mave
n-plugin/2.1.3.RELEASE/spring-boot-maven-plugin-2.1.3.RELEASE.jar (68 kB at 153 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-antrun-pl
ugin/1.8/maven-antrun-plugin-1.8.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-antrun-plu
gin/1.8/maven-antrun-plugin-1.8.pom (3.3 kB at 7.6 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-plugins/2
7/maven-plugins-27.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-plugins/27
/maven-plugins-27.pom (11 kB at 8.4 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-parent/26/maven-p
arent-26.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-parent/26/maven-pa
rent-26.pom (40 kB at 52 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-antrun-pl
ugin/1.8/maven-antrun-plugin-1.8.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-antrun-plu
gin/1.8/maven-antrun-plugin-1.8.jar (36 kB at 83 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-assembly-
plugin/3.1.1/maven-assembly-plugin-3.1.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-assembly-p
lugin/3.1.1/maven-assembly-plugin-3.1.1.pom (15 kB at 35 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-assembly-
plugin/3.1.1/maven-assembly-plugin-3.1.1.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-assembly-p
lugin/3.1.1/maven-assembly-plugin-3.1.1.jar (236 kB at 294 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-dependenc
y-plugin/3.1.1/maven-dependency-plugin-3.1.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-dependency
-plugin/3.1.1/maven-dependency-plugin-3.1.1.pom (15 kB at 36 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-dependenc
y-plugin/3.1.1/maven-dependency-plugin-3.1.1.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-dependency
-plugin/3.1.1/maven-dependency-plugin-3.1.1.jar (167 kB at 268 kB/s)
[INFO]
[INFO] -----------------------< com.example:myproject >------------------------
[INFO] Building myproject 0.0.1-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:3.1.1:tree (default-cli) @ myproject ---
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/reporting/maven-reporti
ng-impl/2.3/maven-reporting-impl-2.3.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/reporting/maven-reportin
g-impl/2.3/maven-reporting-impl-2.3.pom (5.0 kB at 12 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-shared-com
ponents/20/maven-shared-components-20.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-shared-comp
onents/20/maven-shared-components-20.pom (5.1 kB at 13 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-shared-uti
ls/0.6/maven-shared-utils-0.6.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-shared-util
s/0.6/maven-shared-utils-0.6.pom (4.9 kB at 12 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-sink-api/1.
2/doxia-sink-api-1.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-sink-api/1.2
/doxia-sink-api-1.2.pom (1.6 kB at 4.0 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia/1.2/doxia-1
.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia/1.2/doxia-1.
2.pom (19 kB at 45 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-parent/19/maven-p
arent-19.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-parent/19/maven-pa
rent-19.pom (25 kB at 61 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-logging-api
/1.2/doxia-logging-api-1.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-logging-api/
1.2/doxia-logging-api-1.2.pom (1.6 kB at 4.1 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-container-def
ault/1.0-alpha-30/plexus-container-default-1.0-alpha-30.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-container-defa
ult/1.0-alpha-30/plexus-container-default-1.0-alpha-30.pom (3.5 kB at 8.3 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-containers/1.
0-alpha-30/plexus-containers-1.0-alpha-30.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-containers/1.0
-alpha-30/plexus-containers-1.0-alpha-30.pom (1.9 kB at 4.7 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-classworlds/1
.2-alpha-9/plexus-classworlds-1.2-alpha-9.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-classworlds/1.
2-alpha-9/plexus-classworlds-1.2-alpha-9.pom (3.2 kB at 8.0 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-core/1.2/do
xia-core-1.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-core/1.2/dox
ia-core-1.2.pom (4.0 kB at 9.5 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/xerces/xercesImpl/2.9.1/xercesImpl-2.9.1
.pom
Downloaded from central: https://repo.maven.apache.org/maven2/xerces/xercesImpl/2.9.1/xercesImpl-2.9.1.
pom (1.4 kB at 3.5 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/xml-apis/xml-apis/1.3.04/xml-apis-1.3.04
.pom
Downloaded from central: https://repo.maven.apache.org/maven2/xml-apis/xml-apis/1.3.04/xml-apis-1.3.04.
pom (1.8 kB at 4.6 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/httpclient/4.0
.2/httpclient-4.0.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/httpclient/4.0.
2/httpclient-4.0.2.pom (7.5 kB at 18 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/httpcomponents
-client/4.0.2/httpcomponents-client-4.0.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/httpcomponents-
client/4.0.2/httpcomponents-client-4.0.2.pom (9.0 kB at 22 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/project/4.1/pr
oject-4.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/project/4.1/pro
ject-4.1.pom (16 kB at 40 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/httpcore/4.0.1
/httpcore-4.0.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/httpcore/4.0.1/
httpcore-4.0.1.pom (4.9 kB at 12 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/httpcomponents
-core/4.0.1/httpcomponents-core-4.0.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/httpcomponents-
core/4.0.1/httpcomponents-core-4.0.1.pom (9.4 kB at 22 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/project/4.0/pr
oject-4.0.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/project/4.0/pro
ject-4.0.pom (13 kB at 31 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-logging/commons-logging/1.1.1/co
mmons-logging-1.1.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-logging/commons-logging/1.1.1/com
mons-logging-1.1.1.pom (18 kB at 45 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/commons/commons-parent/5/comm
ons-parent-5.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/commons/commons-parent/5/commo
ns-parent-5.pom (16 kB at 39 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-codec/commons-codec/1.3/commons-
codec-1.3.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-codec/commons-codec/1.3/commons-c
odec-1.3.pom (6.1 kB at 14 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-site-render
er/1.2/doxia-site-renderer-1.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-site-rendere
r/1.2/doxia-site-renderer-1.2.pom (6.2 kB at 15 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-sitetools/1
.2/doxia-sitetools-1.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-sitetools/1.
2/doxia-sitetools-1.2.pom (16 kB at 39 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-decoration-
model/1.2/doxia-decoration-model-1.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-decoration-m
odel/1.2/doxia-decoration-model-1.2.pom (3.1 kB at 7.6 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-module-xhtm
l/1.2/doxia-module-xhtml-1.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-module-xhtml
/1.2/doxia-module-xhtml-1.2.pom (1.8 kB at 4.3 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-modules/1.2
/doxia-modules-1.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-modules/1.2/
doxia-modules-1.2.pom (2.5 kB at 6.2 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-module-fml/
1.2/doxia-module-fml-1.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-module-fml/1
.2/doxia-module-fml-1.2.pom (5.6 kB at 14 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-i18n/1.0-beta
-7/plexus-i18n-1.0-beta-7.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-i18n/1.0-beta-
7/plexus-i18n-1.0-beta-7.pom (1.1 kB at 2.6 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-components/1.
1.12/plexus-components-1.1.12.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-components/1.1
.12/plexus-components-1.1.12.pom (3.0 kB at 7.3 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-velocity/1.1.
7/plexus-velocity-1.1.7.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-velocity/1.1.7
/plexus-velocity-1.1.7.pom (2.0 kB at 4.8 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-container-def
ault/1.0-alpha-20/plexus-container-default-1.0-alpha-20.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-container-defa
ult/1.0-alpha-20/plexus-container-default-1.0-alpha-20.pom (3.0 kB at 7.2 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-containers/1.
0-alpha-20/plexus-containers-1.0-alpha-20.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-containers/1.0
-alpha-20/plexus-containers-1.0-alpha-20.pom (1.9 kB at 4.6 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-utils/1.3/ple
xus-utils-1.3.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-utils/1.3/plex
us-utils-1.3.pom (1.0 kB at 2.6 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus/1.0.8/plexus-
1.0.8.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus/1.0.8/plexus-1
.0.8.pom (7.2 kB at 18 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-classworlds/1
.2-alpha-7/plexus-classworlds-1.2-alpha-7.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-classworlds/1.
2-alpha-7/plexus-classworlds-1.2-alpha-7.pom (2.4 kB at 5.9 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/velocity/velocity/1.5/velocit
y-1.5.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/velocity/velocity/1.5/velocity
-1.5.pom (7.8 kB at 19 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/oro/oro/2.0.8/oro-2.0.8.pom
Downloaded from central: https://repo.maven.apache.org/maven2/oro/oro/2.0.8/oro-2.0.8.pom (140 B at 348
 B/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-validator/commons-validator/1.3.
1/commons-validator-1.3.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-validator/commons-validator/1.3.1
/commons-validator-1.3.1.pom (9.0 kB at 22 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-beanutils/commons-beanutils/1.7.
0/commons-beanutils-1.7.0.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-beanutils/commons-beanutils/1.7.0
/commons-beanutils-1.7.0.pom (357 B at 901 B/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-logging/commons-logging/1.0.3/co
mmons-logging-1.0.3.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-logging/commons-logging/1.0.3/com
mons-logging-1.0.3.pom (866 B at 2.2 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-digester/commons-digester/1.6/co
mmons-digester-1.6.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-digester/commons-digester/1.6/com
mons-digester-1.6.pom (974 B at 2.4 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-beanutils/commons-beanutils/1.6/
commons-beanutils-1.6.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-beanutils/commons-beanutils/1.6/c
ommons-beanutils-1.6.pom (2.3 kB at 5.7 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-logging/commons-logging/1.0/comm
ons-logging-1.0.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-logging/commons-logging/1.0/commo
ns-logging-1.0.pom (163 B at 407 B/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-collections/commons-collections/
2.0/commons-collections-2.0.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-collections/commons-collections/2
.0/commons-collections-2.0.pom (171 B at 430 B/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-collections/commons-collections/
2.1/commons-collections-2.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-collections/commons-collections/2
.1/commons-collections-2.1.pom (3.3 kB at 8.3 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/xml-apis/xml-apis/1.0.b2/xml-apis-1.0.b2
.pom
Downloaded from central: https://repo.maven.apache.org/maven2/xml-apis/xml-apis/1.0.b2/xml-apis-1.0.b2.
pom (2.2 kB at 5.6 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-logging/commons-logging/1.0.4/co
mmons-logging-1.0.4.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-logging/commons-logging/1.0.4/com
mons-logging-1.0.4.pom (5.3 kB at 13 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-sink-api/1.
4/doxia-sink-api-1.4.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-sink-api/1.4
/doxia-sink-api-1.4.pom (1.5 kB at 3.8 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia/1.4/doxia-1
.4.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia/1.4/doxia-1.
4.pom (18 kB at 44 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-logging-api
/1.4/doxia-logging-api-1.4.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-logging-api/
1.4/doxia-logging-api-1.4.pom (1.5 kB at 3.7 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-site-render
er/1.4/doxia-site-renderer-1.4.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-site-rendere
r/1.4/doxia-site-renderer-1.4.pom (6.1 kB at 15 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-sitetools/1
.4/doxia-sitetools-1.4.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-sitetools/1.
4/doxia-sitetools-1.4.pom (17 kB at 43 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-core/1.4/do
xia-core-1.4.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-core/1.4/dox
ia-core-1.4.pom (4.1 kB at 10 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-utils/3.0.10/
plexus-utils-3.0.10.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-utils/3.0.10/p
lexus-utils-3.0.10.pom (3.1 kB at 7.7 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus/3.3/plexus-3.
3.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus/3.3/plexus-3.3
.pom (20 kB at 48 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-decoration-
model/1.4/doxia-decoration-model-1.4.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-decoration-m
odel/1.4/doxia-decoration-model-1.4.pom (2.7 kB at 6.7 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-module-xhtm
l/1.4/doxia-module-xhtml-1.4.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-module-xhtml
/1.4/doxia-module-xhtml-1.4.pom (1.6 kB at 4.1 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-modules/1.4
/doxia-modules-1.4.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-modules/1.4/
doxia-modules-1.4.pom (2.6 kB at 6.6 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-module-fml/
1.4/doxia-module-fml-1.4.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-module-fml/1
.4/doxia-module-fml-1.4.pom (4.8 kB at 12 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/velocity/velocity-tools/2.0/v
elocity-tools-2.0.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/velocity/velocity-tools/2.0/ve
locity-tools-2.0.pom (18 kB at 43 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-digester/commons-digester/1.8/co
mmons-digester-1.8.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-digester/commons-digester/1.8/com
mons-digester-1.8.pom (7.0 kB at 18 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-logging/commons-logging/1.1/comm
ons-logging-1.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-logging/commons-logging/1.1/commo
ns-logging-1.1.pom (6.2 kB at 15 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/logkit/logkit/1.0.1/logkit-1.0.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/logkit/logkit/1.0.1/logkit-1.0.1.pom (147
 B at 365 B/s)
Downloading from central: https://repo.maven.apache.org/maven2/avalon-framework/avalon-framework/4.1.3/
avalon-framework-4.1.3.pom
Downloaded from central: https://repo.maven.apache.org/maven2/avalon-framework/avalon-framework/4.1.3/a
valon-framework-4.1.3.pom (167 B at 413 B/s)
Downloading from central: https://repo.maven.apache.org/maven2/javax/servlet/servlet-api/2.3/servlet-ap
i-2.3.pom
Downloaded from central: https://repo.maven.apache.org/maven2/javax/servlet/servlet-api/2.3/servlet-api
-2.3.pom (156 B at 384 B/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-chain/commons-chain/1.1/commons-
chain-1.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-chain/commons-chain/1.1/commons-c
hain-1.1.pom (6.0 kB at 15 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-collections/commons-collections/
3.2/commons-collections-3.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-collections/commons-collections/3
.2/commons-collections-3.2.pom (11 kB at 26 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/dom4j/dom4j/1.1/dom4j-1.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/dom4j/dom4j/1.1/dom4j-1.1.pom (142 B at 3
48 B/s)
Downloading from central: https://repo.maven.apache.org/maven2/sslext/sslext/1.2-0/sslext-1.2-0.pom
Downloaded from central: https://repo.maven.apache.org/maven2/sslext/sslext/1.2-0/sslext-1.2-0.pom (653
 B at 1.6 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-core/1.3.8/stru
ts-core-1.3.8.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-core/1.3.8/strut
s-core-1.3.8.pom (4.3 kB at 11 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-parent/1.3.8/st
ruts-parent-1.3.8.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-parent/1.3.8/str
uts-parent-1.3.8.pom (9.8 kB at 24 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-master/4/struts
-master-4.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-master/4/struts-
master-4.pom (12 kB at 29 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/apache/2/apache-2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/apache/2/apache-2.pom (3.4 kB
at 8.4 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/antlr/antlr/2.7.2/antlr-2.7.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/antlr/antlr/2.7.2/antlr-2.7.2.pom (145 B
at 370 B/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-taglib/1.3.8/st
ruts-taglib-1.3.8.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-taglib/1.3.8/str
uts-taglib-1.3.8.pom (3.1 kB at 7.5 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-tiles/1.3.8/str
uts-tiles-1.3.8.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-tiles/1.3.8/stru
ts-tiles-1.3.8.pom (2.9 kB at 7.2 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/velocity/velocity/1.6.2/veloc
ity-1.6.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/velocity/velocity/1.6.2/veloci
ty-1.6.2.pom (11 kB at 26 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/file-management/
3.0.0/file-management-3.0.0.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/file-management/3
.0.0/file-management-3.0.0.pom (4.7 kB at 12 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-shared-com
ponents/22/maven-shared-components-22.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-shared-comp
onents/22/maven-shared-components-22.pom (5.1 kB at 13 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-shared-io/
3.0.0/maven-shared-io-3.0.0.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-shared-io/3
.0.0/maven-shared-io-3.0.0.pom (4.2 kB at 10 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-compat/3.0/maven-
compat-3.0.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-compat/3.0/maven-c
ompat-3.0.pom (4.0 kB at 9.9 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/wagon/wagon-provider-ap
i/1.0-beta-6/wagon-provider-api-1.0-beta-6.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/wagon/wagon-provider-api
/1.0-beta-6/wagon-provider-api-1.0-beta-6.pom (1.8 kB at 4.4 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/wagon/wagon/1.0-beta-6/
wagon-1.0-beta-6.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/wagon/wagon/1.0-beta-6/w
agon-1.0-beta-6.pom (12 kB at 30 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-utils/1.4.2/p
lexus-utils-1.4.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-utils/1.4.2/pl
exus-utils-1.4.2.pom (2.0 kB at 4.8 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/wagon/wagon-provider-ap
i/2.10/wagon-provider-api-2.10.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/wagon/wagon-provider-api
/2.10/wagon-provider-api-2.10.pom (1.7 kB at 4.1 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/wagon/wagon/2.10/wagon-
2.10.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/wagon/wagon/2.10/wagon-2
.10.pom (21 kB at 49 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-utils/3.0.15/
plexus-utils-3.0.15.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-utils/3.0.15/p
lexus-utils-3.0.15.pom (3.1 kB at 7.7 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-io/3.0.0/plex
us-io-3.0.0.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-io/3.0.0/plexu
s-io-3.0.0.pom (4.5 kB at 11 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-dependency
-analyzer/1.10/maven-dependency-analyzer-1.10.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-dependency-
analyzer/1.10/maven-dependency-analyzer-1.10.pom (6.7 kB at 17 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/ow2/asm/asm/6.1.1/asm-6.1.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/ow2/asm/asm/6.1.1/asm-6.1.1.pom (2.9
kB at 7.4 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-model/2.0.5/maven
-model-2.0.5.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-model/2.0.5/maven-
model-2.0.5.pom (2.7 kB at 6.7 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven/2.0.5/maven-2.0.5
.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven/2.0.5/maven-2.0.5.
pom (5.7 kB at 14 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-utils/1.1/ple
xus-utils-1.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-utils/1.1/plex
us-utils-1.1.pom (767 B at 1.9 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-artifact/2.0.5/ma
ven-artifact-2.0.5.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-artifact/2.0.5/mav
en-artifact-2.0.5.pom (727 B at 1.8 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-dependency
-tree/3.0.1/maven-dependency-tree-3.0.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-dependency-
tree/3.0.1/maven-dependency-tree-3.0.1.pom (7.5 kB at 18 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-artifact-t
ransfer/0.9.1/maven-artifact-transfer-0.9.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-artifact-tr
ansfer/0.9.1/maven-artifact-transfer-0.9.1.pom (7.6 kB at 19 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-codec/commons-codec/1.6/commons-
codec-1.6.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-codec/commons-codec/1.6/commons-c
odec-1.6.pom (11 kB at 27 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-shared-uti
ls/3.2.0/maven-shared-utils-3.2.0.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-shared-util
s/3.2.0/maven-shared-utils-3.2.0.pom (4.9 kB at 12 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-lang/commons-lang/2.6/commons-la
ng-2.6.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-lang/commons-lang/2.6/commons-lan
g-2.6.pom (17 kB at 43 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/commons/commons-parent/17/com
mons-parent-17.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/commons/commons-parent/17/comm
ons-parent-17.pom (31 kB at 75 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-collections/commons-collections/
3.2.2/commons-collections-3.2.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/commons-collections/commons-collections/3
.2.2/commons-collections-3.2.2.pom (12 kB at 30 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-core/1.2/do
xia-core-1.2.jar
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/httpclient/4.0
.2/httpclient-4.0.2.jar
Downloading from central: https://repo.maven.apache.org/maven2/xml-apis/xml-apis/1.3.04/xml-apis-1.3.04
.jar
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/reporting/maven-reporti
ng-impl/2.3/maven-reporting-impl-2.3.jar
Downloading from central: https://repo.maven.apache.org/maven2/xerces/xercesImpl/2.9.1/xercesImpl-2.9.1
.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-core/1.2/dox
ia-core-1.2.jar (154 kB at 258 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/httpcore/4.0.1
/httpcore-4.0.1.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/httpcore/4.0.1/
httpcore-4.0.1.jar (173 kB at 143 kB/s)
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/reporting/maven-reportin
g-impl/2.3/maven-reporting-impl-2.3.jar (18 kB at 15 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-validator/commons-validator/1.3.
1/commons-validator-1.3.1.jar
Downloading from central: https://repo.maven.apache.org/maven2/commons-beanutils/commons-beanutils/1.7.
0/commons-beanutils-1.7.0.jar
Downloaded from central: https://repo.maven.apache.org/maven2/xml-apis/xml-apis/1.3.04/xml-apis-1.3.04.
jar (194 kB at 104 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-digester/commons-digester/1.6/co
mmons-digester-1.6.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/httpcomponents/httpclient/4.0.
2/httpclient-4.0.2.jar (293 kB at 152 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-logging/commons-logging/1.0.4/co
mmons-logging-1.0.4.jar
Downloaded from central: https://repo.maven.apache.org/maven2/commons-validator/commons-validator/1.3.1
/commons-validator-1.3.1.jar (139 kB at 72 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-sink-api/1.
4/doxia-sink-api-1.4.jar
Downloaded from central: https://repo.maven.apache.org/maven2/commons-beanutils/commons-beanutils/1.7.0
/commons-beanutils-1.7.0.jar (189 kB at 89 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-logging-api
/1.4/doxia-logging-api-1.4.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-sink-api/1.4
/doxia-sink-api-1.4.jar (11 kB at 4.7 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-site-render
er/1.4/doxia-site-renderer-1.4.jar
Downloaded from central: https://repo.maven.apache.org/maven2/commons-logging/commons-logging/1.0.4/com
mons-logging-1.0.4.jar (38 kB at 16 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-decoration-
model/1.4/doxia-decoration-model-1.4.jar
Downloaded from central: https://repo.maven.apache.org/maven2/xerces/xercesImpl/2.9.1/xercesImpl-2.9.1.
jar (1.2 MB at 500 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-module-xhtm
l/1.4/doxia-module-xhtml-1.4.jar
Downloaded from central: https://repo.maven.apache.org/maven2/commons-digester/commons-digester/1.6/com
mons-digester-1.6.jar (168 kB at 67 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-module-fml/
1.4/doxia-module-fml-1.4.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-logging-api/
1.4/doxia-logging-api-1.4.jar (11 kB at 4.4 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-i18n/1.0-beta
-7/plexus-i18n-1.0-beta-7.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-site-rendere
r/1.4/doxia-site-renderer-1.4.jar (53 kB at 19 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-container-def
ault/1.0-alpha-30/plexus-container-default-1.0-alpha-30.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-decoration-m
odel/1.4/doxia-decoration-model-1.4.jar (61 kB at 21 kB/s)
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-module-xhtml
/1.4/doxia-module-xhtml-1.4.jar (15 kB at 5.3 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-velocity/1.1.
7/plexus-velocity-1.1.7.jar
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/velocity/velocity/1.5/velocit
y-1.5.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/doxia/doxia-module-fml/1
.4/doxia-module-fml-1.4.jar (38 kB at 13 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/oro/oro/2.0.8/oro-2.0.8.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-i18n/1.0-beta-
7/plexus-i18n-1.0-beta-7.jar (11 kB at 3.5 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/velocity/velocity-tools/2.0/v
elocity-tools-2.0.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-container-defa
ult/1.0-alpha-30/plexus-container-default-1.0-alpha-30.jar (237 kB at 72 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-chain/commons-chain/1.1/commons-
chain-1.1.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-velocity/1.1.7
/plexus-velocity-1.1.7.jar (7.7 kB at 2.3 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/dom4j/dom4j/1.1/dom4j-1.1.jar
Downloaded from central: https://repo.maven.apache.org/maven2/oro/oro/2.0.8/oro-2.0.8.jar (65 kB at 19
kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/sslext/sslext/1.2-0/sslext-1.2-0.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/velocity/velocity-tools/2.0/ve
locity-tools-2.0.jar (347 kB at 94 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-core/1.3.8/stru
ts-core-1.3.8.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/velocity/velocity/1.5/velocity
-1.5.jar (392 kB at 104 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/antlr/antlr/2.7.2/antlr-2.7.2.jar
Downloaded from central: https://repo.maven.apache.org/maven2/commons-chain/commons-chain/1.1/commons-c
hain-1.1.jar (90 kB at 24 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-taglib/1.3.8/st
ruts-taglib-1.3.8.jar
Downloaded from central: https://repo.maven.apache.org/maven2/sslext/sslext/1.2-0/sslext-1.2-0.jar (26
kB at 6.7 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-tiles/1.3.8/str
uts-tiles-1.3.8.jar
Downloaded from central: https://repo.maven.apache.org/maven2/dom4j/dom4j/1.1/dom4j-1.1.jar (457 kB at
114 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-archiver/3.6.
0/plexus-archiver-3.6.0.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-core/1.3.8/strut
s-core-1.3.8.jar (329 kB at 72 kB/s)
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-tiles/1.3.8/stru
ts-tiles-1.3.8.jar (120 kB at 26 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/commons/commons-compress/1.16
.1/commons-compress-1.16.1.jar
Downloading from central: https://repo.maven.apache.org/maven2/org/objenesis/objenesis/2.6/objenesis-2.
6.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/struts/struts-taglib/1.3.8/str
uts-taglib-1.3.8.jar (252 kB at 55 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/file-management/
3.0.0/file-management-3.0.0.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-archiver/3.6.0
/plexus-archiver-3.6.0.jar (191 kB at 41 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-shared-io/
3.0.0/maven-shared-io-3.0.0.jar
Downloaded from central: https://repo.maven.apache.org/maven2/antlr/antlr/2.7.2/antlr-2.7.2.jar (358 kB
 at 76 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-compat/3.0/maven-
compat-3.0.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/objenesis/objenesis/2.6/objenesis-2.6
.jar (56 kB at 11 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/wagon/wagon-provider-ap
i/2.10/wagon-provider-api-2.10.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/file-management/3
.0.0/file-management-3.0.0.jar (35 kB at 7.0 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-io/3.0.0/plex
us-io-3.0.0.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-shared-io/3
.0.0/maven-shared-io-3.0.0.jar (41 kB at 8.0 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-dependency
-analyzer/1.10/maven-dependency-analyzer-1.10.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-compat/3.0/maven-c
ompat-3.0.jar (285 kB at 52 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/ow2/asm/asm/6.1.1/asm-6.1.1.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/commons/commons-compress/1.16.
1/commons-compress-1.16.1.jar (560 kB at 103 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-dependency
-tree/3.0.1/maven-dependency-tree-3.0.1.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/wagon/wagon-provider-api
/2.10/wagon-provider-api-2.10.jar (54 kB at 9.7 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-artifact-t
ransfer/0.9.1/maven-artifact-transfer-0.9.1.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-io/3.0.0/plexu
s-io-3.0.0.jar (74 kB at 13 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-codec/commons-codec/1.6/commons-
codec-1.6.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-dependency-
analyzer/1.10/maven-dependency-analyzer-1.10.jar (35 kB at 6.3 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-shared-uti
ls/3.2.0/maven-shared-utils-3.2.0.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/ow2/asm/asm/6.1.1/asm-6.1.1.jar (108
kB at 18 kB/s)
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-dependency-
tree/3.0.1/maven-dependency-tree-3.0.1.jar (37 kB at 6.2 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/commons-lang/commons-lang/2.6/commons-la
ng-2.6.jar
Downloading from central: https://repo.maven.apache.org/maven2/commons-collections/commons-collections/
3.2.2/commons-collections-3.2.2.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-artifact-tr
ansfer/0.9.1/maven-artifact-transfer-0.9.1.jar (123 kB at 21 kB/s)
Downloaded from central: https://repo.maven.apache.org/maven2/commons-codec/commons-codec/1.6/commons-c
odec-1.6.jar (233 kB at 38 kB/s)
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/shared/maven-shared-util
s/3.2.0/maven-shared-utils-3.2.0.jar (165 kB at 27 kB/s)
Downloaded from central: https://repo.maven.apache.org/maven2/commons-lang/commons-lang/2.6/commons-lan
g-2.6.jar (284 kB at 43 kB/s)
Downloaded from central: https://repo.maven.apache.org/maven2/commons-collections/commons-collections/3
.2.2/commons-collections-3.2.2.jar (588 kB at 85 kB/s)
[INFO] com.example:myproject:jar:0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  01:13 min
[INFO] Finished at: 2019-09-17T10:54:18+08:00
[INFO] ------------------------------------------------------------------------
```
该`mvn dependency:tree`命令时打印你的项目依赖关系的树形表示。你可以看到它`spring-boot-starter-parent`本身不提供依赖关系。要添加必要的依赖关系，请编辑`pom.xml`并`spring-boot-starter-web`在该`parent`部分下方添加依赖项：
```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```
如果让`mvn dependency:tree`再次运行，你会发现现在许多其他依赖项，包括Tomcat Web服务器和Spring Boot本身。
###### 编写代码
要完成我们的应用程序，我们需要创建一个java文件。默认情况下，Maven编译源代码`src/main/java`,因此你需要创建该文件夹结构，然后添加一个名为`src/main/java/Example.java`包含以下代码的文件：
```
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.web.bind.annotation.*;

@RestController
@EnableAutoConfiguration
public class Example {

	@RequestMapping("/")
	String home() {
		return "Hello World!";
	}

	public static void main(String[] args) {
		SpringApplication.run(Example.class, args);
	}

}
```
尽管这里没有太多的代码，但还是有很多重要的部分。
###### @RestController和@RequestMapping注解
我们`Example类`的第一个注释是`@RestController`。这被称为stereotype annotation。它为阅读代码的人提供了线索，对于Spring来说，这个类扮演者特定的角色。在这种情况下，我们的类是一个Web`@Controller`，所以Spring在处理传入的Web请求时会考虑这个类。
该`@RequestMapping`注释提供路由的信息。它告诉Spring，任何带有`/`路径的HTTP请求都映射到该`home`方法。该`@RestController`注释告诉Spring将结果字符串直接呈现给调用者。
> 在`@RestController`与`@RequestMapping`注解是SpringMVC的注解。（他们并不特定Spring Boot。）

###### @EnableAutoConfiguration注解
第二个界别注释是`@EnableAutoConfiguration`。这个注解告诉Spring Boot根据你体检的jar依赖来猜测你想要如何配置Spring。自从`spring-boot-starter-web`添加了Tomcat和Spring MVC之后，自动配置假定你正在开发一个Web应用程序并据此设置Spring。
> ###### 启动器和自动配置
> 自动配置是指在与starter配合使用，但这两个概念并不直接相关。你可以自由选择启动器之外的jar依赖项。Spring Boot仍然尽力自动配置你的应用程序。

###### main方法
我们应用程序的最后一部分是该`main`方法。这只是一个遵循Java约定的应用程序的入口点的标准方法。我们的主要方法`SpringApplication`通过调用委托给Spring Boot的类`run`。`SpringApplication`引导我们的应用程序，从Spring开始，然后启动自动配置的Tomcat Web服务器。我们需要`Example.class`将该`run`方法的参数作为参数传递，已确定`SpringApplication`那些事主要的Spring组件。还传递了args数组以传递命令行参数。
###### 运行示例
在这一点上，你的应用程序应该工作。由于你使用了`spring-boot-starter-parent`POM，`run`因此你可以使用一个有用的目标开启动应用程序。`mvn spring-boot:run`从根据项目目录中键入以启动应用程序。你应该看到类似于以下内容的输出：
```
D:\desktop\Spring Boot>mvn spring-boot:run
[INFO] Scanning for projects...
[INFO]
[INFO] -----------------------< com.example:myproject >------------------------
[INFO] Building myproject 0.0.1-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] >>> spring-boot-maven-plugin:2.1.3.RELEASE:run (default-cli) > test-compile @ myproject >>>
[INFO]
[INFO] --- maven-resources-plugin:3.1.0:resources (default-resources) @ myproject ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory D:\desktop\Spring Boot\src\main\resources
[INFO] skip non existing resourceDirectory D:\desktop\Spring Boot\src\main\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ myproject ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to D:\desktop\Spring Boot\target\classes
[INFO]
[INFO] --- maven-resources-plugin:3.1.0:testResources (default-testResources) @ myproject ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory D:\desktop\Spring Boot\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ myproject ---
[INFO] No sources to compile
[INFO]
[INFO] <<< spring-boot-maven-plugin:2.1.3.RELEASE:run (default-cli) < test-compile @ myproject <<<
[INFO]
[INFO]
[INFO] --- spring-boot-maven-plugin:2.1.3.RELEASE:run (default-cli) @ myproject ---

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.1.3.RELEASE)

2019-09-17 13:52:51.728  INFO 6972 --- [           main] Example                                  : Starting Example on E01N180317 with PID 6972 (started by yan.yan in D:\des
ktop\Spring Boot)
2019-09-17 13:52:51.734  INFO 6972 --- [           main] Example                                  : No active profile set, falling back to default profiles: default
2019-09-17 13:52:53.531  INFO 6972 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2019-09-17 13:52:53.601  INFO 6972 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2019-09-17 13:52:53.605  INFO 6972 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.16]
2019-09-17 13:52:53.626  INFO 6972 --- [           main] o.a.catalina.core.AprLifecycleListener   : The APR based Apache Tomcat Native library which allows optimal performanc
e in production environments was not found on the java.library.path: [C:\Program Files\Java\jdk1.8.0_191\bin;C:\WINDOWS\Sun\Java\bin;C:\WINDOWS\system32;C:\WINDOWS;D:\Program
 Files\JetBrains\IntelliJ IDEA 2019.2\jbr\\bin;D:\Program Files\JetBrains\IntelliJ IDEA 2019.2\jbr\\bin\server;C:\WINDOWS\system32;C:\Program Files\Java\jdk1.8.0_191\bin;C:\P
rogram Files\Java\jdk1.8.0_191\jre\bin;D:\Maven\apache-maven-3.6.2\bin;C:\Users\yan.yan\AppData\Local\Microsoft\WindowsApps;C:\Users\yan.yan\AppData\Local\Programs\Git\cmd;D:
\软件安装位置\Microsoft VS Code\bin;.]
2019-09-17 13:52:53.837  INFO 6972 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2019-09-17 13:52:53.945  INFO 6972 --- [           main] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 2160 ms
2019-09-17 13:52:54.290  INFO 6972 --- [           main] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
2019-09-17 13:52:54.504  INFO 6972 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2019-09-17 13:52:54.508  INFO 6972 --- [           main] Example                                  : Started Example in 3.27 seconds (JVM running for 10.175)
2019-09-17 13:53:08.604  INFO 6972 --- [nio-8080-exec-2] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2019-09-17 13:53:08.605  INFO 6972 --- [nio-8080-exec-2] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2019-09-17 13:53:08.619  INFO 6972 --- [nio-8080-exec-2] o.s.web.servlet.DispatcherServlet        : Completed initialization in 14 ms
2019-09-17 13:54:15.746  INFO 6972 --- [       Thread-4] o.s.s.concurrent.ThreadPoolTaskExecutor  : Shutting down ExecutorService 'applicationTaskExecutor'
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
```
如果你打开一个Web浏览器在地址栏上面输入`localhost:8080`，你应该可以看到以下的输出：
```
Hello world！
```
如果想在IDE中正常退出应用程序，请按`ctrl + C`。
###### 创建可执行的jar
我们通过创建一个完全自我包含的可执行jar文件来完成我们的实例，我们可以在生产环境中运行它。可执行的jar（有时成为fat jars）是包含编译的类以及代码运行所需要的所有的jar包依赖项的归档（archives）
```
######可执行jar和java
java不提供任何标准的方法来加载嵌套的jar文件（即本身包含在jar中的jar文件）。如果你正在寻找可以发布自包含的应用程序，这可能是有问题的。
为了解决这个问题，许多开发人员使用uber jars(超级jar)。一个uber jar简单的将所有类、jar包进行档案。这些方法的问题是，很难看到你在应用程序中实际使用了那些苦。如果在多个jar中是用相同的文件名（但具有不同的内容），也可能会出现问题。
Spring Boot采用一个不同的方法这样可以直接对jar进行嵌套。
```
要创建可执行的jar，我们需要将speing-boot-maven-plugin添加到我们的pom.xml中。在dependencies标签下方插入以下行：
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
> spring-boot-starter-parent POM包括重新打包的目标的executions标签配置。如果你不使用该父POM，你讲需要自己生命此配置。

保存你的`pom.xml`并从命令行运行`mvn package`
```
D:\desktop\Spring Boot>mvn package
[INFO] Scanning for projects...
[INFO]
[INFO] -----------------------< com.example:myproject >------------------------
[INFO] Building myproject 0.0.1-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:3.1.0:resources (default-resources) @ myproject ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory D:\desktop\Spring Boot\src\main\resources
[INFO] skip non existing resourceDirectory D:\desktop\Spring Boot\src\main\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ myproject ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:3.1.0:testResources (default-testResources) @ myproject ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory D:\desktop\Spring Boot\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ myproject ---
[INFO] No sources to compile
[INFO]
[INFO] --- maven-surefire-plugin:2.22.1:test (default-test) @ myproject ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:3.1.1:jar (default-jar) @ myproject ---
[INFO] Building jar: D:\desktop\Spring Boot\target\myproject-0.0.1-SNAPSHOT.jar
[INFO]
[INFO] --- spring-boot-maven-plugin:2.1.3.RELEASE:repackage (repackage) @ myproject ---
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  5.933 s
[INFO] Finished at: 2019-09-17T14:08:26+08:00
[INFO] ------------------------------------------------------------------------
```
这时候你在看`target`目录，你应该能看到`myproject-0.0.1-SNAPSHOT.jar`。该文件的大小约为10MB。如果你想查看里面的内容可以使用`jar tvf`
```
D:\desktop\Spring Boot>jar tvf target/myproject-0.0.1-SNAPSHOT.jar
     0 Tue Sep 17 14:08:24 CST 2019 META-INF/
   499 Tue Sep 17 14:08:24 CST 2019 META-INF/MANIFEST.MF
     0 Tue Sep 17 14:08:24 CST 2019 org/
     0 Tue Sep 17 14:08:24 CST 2019 org/springframework/
     0 Tue Sep 17 14:08:24 CST 2019 org/springframework/boot/
     0 Tue Sep 17 14:08:24 CST 2019 org/springframework/boot/loader/
     0 Tue Sep 17 14:08:24 CST 2019 org/springframework/boot/loader/data/
  2688 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/data/RandomAccessDataFile$DataInputStream.class
     0 Tue Sep 17 14:08:24 CST 2019 org/springframework/boot/loader/jar/
  5267 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/CentralDirectoryFileHeader.class
  3263 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/data/RandomAccessDataFile$FileAccess.class
     0 Tue Sep 17 14:08:24 CST 2019 org/springframework/boot/loader/archive/
  1487 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/ExplodedArchive$FileEntryIterator$EntryComparator.class
  3837 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/ExplodedArchive$FileEntryIterator.class
   282 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/data/RandomAccessDataFile$1.class
  3116 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/CentralDirectoryEndRecord.class
 11548 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/Handler.class
  5243 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/ExplodedArchive.class
  4015 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/data/RandomAccessDataFile.class
  4624 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/CentralDirectoryParser.class
  1813 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/ZipInflaterInputStream.class
   302 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/Archive$Entry.class
   273 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/ExplodedArchive$1.class
   485 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/data/RandomAccessData.class
   437 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/Archive$EntryFilter.class
  7336 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/JarFileArchive.class
  1953 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/PropertiesLauncher$PrefixMatchingArchiveFilter.class
  1484 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/PropertiesLauncher$ArchiveEntryFilter.class
   266 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/PropertiesLauncher$1.class
 19737 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/PropertiesLauncher.class
  4684 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/Launcher.class
  1502 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/MainMethodRunner.class
  3608 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/ExecutableArchiveLauncher.class
  1721 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/WarLauncher.class
  1585 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/JarLauncher.class
     0 Tue Sep 17 14:08:24 CST 2019 org/springframework/boot/loader/util/
  5203 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/util/SystemPropertyUtils.class
  1535 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/LaunchedURLClassLoader$UseFastConnectionExceptionsEnumeration.class
  5699 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/LaunchedURLClassLoader.class
   616 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/Bytes.class
   702 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarURLConnection$1.class
  1779 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/JarFileArchive$EntryIterator.class
  4306 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarURLConnection$JarEntryName.class
  1081 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/JarFileArchive$JarFileEntry.class
  9854 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarURLConnection.class
   945 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/Archive.class
  1233 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarFile$2.class
  2062 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarFile$1.class
  1374 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarFile$JarFileType.class
 15076 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarFile.class
  4976 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/AsciiBytes.class
  1593 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarFileEntries$1.class
  2046 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarFileEntries$EntryIterator.class
   540 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/CentralDirectoryVisitor.class
   299 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarEntryFilter.class
  3619 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarEntry.class
   345 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/FileHeader.class
  3650 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/StringSequence.class
 14087 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarFileEntries.class
  1102 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/ExplodedArchive$FileEntry.class
     0 Tue Sep 17 14:08:26 CST 2019 META-INF/maven/
     0 Tue Sep 17 14:08:26 CST 2019 META-INF/maven/com.example/
     0 Tue Sep 17 14:08:26 CST 2019 META-INF/maven/com.example/myproject/
    99 Tue Sep 17 13:42:34 CST 2019 META-INF/maven/com.example/myproject/pom.properties
     0 Tue Sep 17 14:08:24 CST 2019 BOOT-INF/
     0 Tue Sep 17 14:08:24 CST 2019 BOOT-INF/classes/
   968 Tue Sep 17 13:52:50 CST 2019 BOOT-INF/classes/Example.class
  1111 Tue Sep 17 14:06:32 CST 2019 META-INF/maven/com.example/myproject/pom.xml
     0 Tue Sep 17 14:08:24 CST 2019 BOOT-INF/lib/
   406 Fri Feb 15 10:41:44 CST 2019 BOOT-INF/lib/spring-boot-starter-web-2.1.3.RELEASE.jar
   398 Fri Feb 15 10:41:22 CST 2019 BOOT-INF/lib/spring-boot-starter-2.1.3.RELEASE.jar
948706 Fri Feb 15 10:08:36 CST 2019 BOOT-INF/lib/spring-boot-2.1.3.RELEASE.jar
1257845 Fri Feb 15 10:18:16 CST 2019 BOOT-INF/lib/spring-boot-autoconfigure-2.1.3.RELEASE.jar
   407 Fri Feb 15 10:41:22 CST 2019 BOOT-INF/lib/spring-boot-starter-logging-2.1.3.RELEASE.jar
290339 Fri Mar 31 21:27:54 CST 2017 BOOT-INF/lib/logback-classic-1.2.3.jar
471901 Fri Mar 31 21:27:16 CST 2017 BOOT-INF/lib/logback-core-1.2.3.jar
 41203 Thu Mar 16 17:36:32 CST 2017 BOOT-INF/lib/slf4j-api-1.7.25.jar
 17522 Tue Feb 05 18:14:24 CST 2019 BOOT-INF/lib/log4j-to-slf4j-2.11.2.jar
266283 Tue Feb 05 18:11:30 CST 2019 BOOT-INF/lib/log4j-api-2.11.2.jar
  4596 Thu Mar 16 17:37:48 CST 2017 BOOT-INF/lib/jul-to-slf4j-1.7.25.jar
 26586 Wed Feb 21 15:54:16 CST 2018 BOOT-INF/lib/javax.annotation-api-1.3.2.jar
1293377 Wed Feb 13 05:32:02 CST 2019 BOOT-INF/lib/spring-core-5.1.5.RELEASE.jar
 23725 Wed Feb 13 05:31:54 CST 2019 BOOT-INF/lib/spring-jcl-5.1.5.RELEASE.jar
301298 Mon Aug 27 16:23:36 CST 2018 BOOT-INF/lib/snakeyaml-1.23.jar
   405 Fri Feb 15 10:41:44 CST 2019 BOOT-INF/lib/spring-boot-starter-json-2.1.3.RELEASE.jar
1347236 Sat Dec 15 21:59:06 CST 2018 BOOT-INF/lib/jackson-databind-2.9.8.jar
 66519 Sat Jul 29 20:53:26 CST 2017 BOOT-INF/lib/jackson-annotations-2.9.0.jar
325619 Sat Dec 15 13:19:12 CST 2018 BOOT-INF/lib/jackson-core-2.9.8.jar
 33391 Sat Dec 15 23:06:14 CST 2018 BOOT-INF/lib/jackson-datatype-jdk8-2.9.8.jar
100674 Sat Dec 15 23:06:24 CST 2018 BOOT-INF/lib/jackson-datatype-jsr310-2.9.8.jar
  8642 Sat Dec 15 23:06:04 CST 2018 BOOT-INF/lib/jackson-module-parameter-names-2.9.8.jar
   406 Fri Feb 15 10:41:44 CST 2019 BOOT-INF/lib/spring-boot-starter-tomcat-2.1.3.RELEASE.jar
3287907 Mon Feb 04 16:31:00 CST 2019 BOOT-INF/lib/tomcat-embed-core-9.0.16.jar
250080 Mon Feb 04 16:31:02 CST 2019 BOOT-INF/lib/tomcat-embed-el-9.0.16.jar
265364 Mon Feb 04 16:31:02 CST 2019 BOOT-INF/lib/tomcat-embed-websocket-9.0.16.jar
1155701 Fri Jan 04 14:52:48 CST 2019 BOOT-INF/lib/hibernate-validator-6.0.14.Final.jar
 93107 Tue Dec 19 16:23:28 CST 2017 BOOT-INF/lib/validation-api-2.0.1.Final.jar
 66469 Wed Feb 14 13:23:28 CST 2018 BOOT-INF/lib/jboss-logging-3.3.2.Final.jar
 66540 Tue Mar 27 18:35:34 CST 2018 BOOT-INF/lib/classmate-1.4.0.jar
1381453 Wed Feb 13 05:32:56 CST 2019 BOOT-INF/lib/spring-web-5.1.5.RELEASE.jar
672558 Wed Feb 13 05:32:08 CST 2019 BOOT-INF/lib/spring-beans-5.1.5.RELEASE.jar
800464 Wed Feb 13 05:33:32 CST 2019 BOOT-INF/lib/spring-webmvc-5.1.5.RELEASE.jar
368947 Wed Feb 13 05:32:22 CST 2019 BOOT-INF/lib/spring-aop-5.1.5.RELEASE.jar
1099682 Wed Feb 13 05:32:30 CST 2019 BOOT-INF/lib/spring-context-5.1.5.RELEASE.jar
280409 Wed Feb 13 05:32:24 CST 2019 BOOT-INF/lib/spring-expression-5.1.5.RELEASE.jar
```
在target文件目录中你应该还可以看到一个名为`myproject-0.0.1-SNAPSHOT.jar.original`的较小的文件。这是Maven在Spring Boot重新打包之前创建的原始jar文件：
```
>使用java -jar命令运行该应用程序：
```
```
D:\desktop\Spring Boot> java -jar target/myproject-0.0.1-SNAPSHOT.jar

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.1.3.RELEASE)

2019-09-17 14:17:00.636  INFO 13744 --- [           main] Example                                  : Starting Example on E01N180317 with PID 13744 (D:\desktop\Spring Boot\tar
get\myproject-0.0.1-SNAPSHOT.jar started by yan.yan in D:\desktop\Spring Boot)
2019-09-17 14:17:00.643  INFO 13744 --- [           main] Example                                  : No active profile set, falling back to default profiles: default
2019-09-17 14:17:03.229  INFO 13744 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2019-09-17 14:17:03.294  INFO 13744 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2019-09-17 14:17:03.295  INFO 13744 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.16]
2019-09-17 14:17:03.315  INFO 13744 --- [           main] o.a.catalina.core.AprLifecycleListener   : The APR based Apache Tomcat Native library which allows optimal performan
ce in production environments was not found on the java.library.path: [D:\Program Files\JetBrains\IntelliJ IDEA 2019.2\jbr\bin;C:\WINDOWS\Sun\Java\bin;C:\WINDOWS\system32;C:\
WINDOWS;D:\Program Files\JetBrains\IntelliJ IDEA 2019.2\jbr\\bin;D:\Program Files\JetBrains\IntelliJ IDEA 2019.2\jbr\\bin\server;C:\WINDOWS\system32;C:\Program Files\Java\jdk
1.8.0_191\bin;C:\Program Files\Java\jdk1.8.0_191\jre\bin;D:\Maven\apache-maven-3.6.2\bin;C:\Users\yan.yan\AppData\Local\Microsoft\WindowsApps;C:\Users\yan.yan\AppData\Local\P
rograms\Git\cmd;D:\软件安装位置\Microsoft VS Code\bin;.]
2019-09-17 14:17:03.515  INFO 13744 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2019-09-17 14:17:03.517  INFO 13744 --- [           main] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 2790 ms
2019-09-17 14:17:04.011  INFO 13744 --- [           main] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
2019-09-17 14:17:04.409  INFO 13744 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2019-09-17 14:17:04.421  INFO 13744 --- [           main] Example                                  : Started Example in 4.772 seconds (JVM running for 5.401)
```
这时候你在浏览器地址栏上输入：`localhost:8080`会达到以下显示：
```
Hello world!
```
此时如果想要停止运行程序使用：`ctrl + C`键即可退出。