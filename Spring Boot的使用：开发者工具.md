###### Spring Boot的使用：开发者工具
Spring Boot包含了一些额外的工具集，用于提升Spring Boot应用的开发体验。`spring-boot-devtools`模块可以included到任何模块中，以提供development-time特性，你只需要简单的将该模块的依赖添加到构建中：
**Maven**：
```
<dependencies>
    <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
</dependencies>
```
**Gradle**:
```
configurations {
    developmentOnly
    runtimeClasspath {
        extendsFro, developmentOnly
    }
}
dependencies {
    developmentOnly("org.springframework.boot:spring-boot-devtools")
}
```
> 在运行一个完整的，打包过的应用时，开发者工具（devtools）会被自动禁用。如果应用使用`java -jar`或特殊的类加载器启动，都会被认为是一个产品级的应用（production application），从而禁用开发者工具。为了防止devtools传递到项目中的其他模块，设置该依赖级别为optional是个不错的实践。不过Gradle不支持`optional`依赖，所以你可能要了解下propdeps-plugin。如果想确保devtools绝对不会包含在一个产品构建中，你可以使用`excludeDevtools`构建属性彻底移除该JAR,Maven和Gradle都支持该属性。

###### 默认属性值
Spring Boot支持的几个库使用缓存来提高性能。例如，模板引擎缓存已编译的模板以避免重复解析模板文件。此外，Spring MVC可以在提供静态资源时为响应添加HTTP缓存表头。
虽然缓存在生产中非常有用，但是在开发过程中可能会适得其反，使你无法看到刚刚在应用程序中进行的更改。因此，spring-boot-devtools默认禁用缓存选项。
缓存选项通常由`application.properties`文件中的设置配置。例如，Thymeleaf提供该`spring.thymeleaf.cache`物业。该`spring-boot-devtools`模块不需要手动设置这些属性，而是自动应用合理的开发时的配置。
因为在开发Spring MVC和Spring WebFlux应用程序时需要有关Web请求的更多信息，所以开发人员工具将启用`DEBUG`日志`web`记录组的日志记录。这将为你提供有关传入请求，处理程序正在处理它，响应结果等信息。如果你希望记录所有请求详细信息（包括可能的敏感信息），则可以打开`spring.http.log-request-details`配置属性。
> 如果你不想被应用属性默认值可以设置`spring.devtools.add-properties`到`false`你`application.properties`.

###### 自动重启
如果应用使用`spring-boot-dectools`，则只要classpath下的文件有变动，他就会自动重启。这在使用IDE时非常有用，因为可以很快得到代码改变的反馈。默认情况下，classpath下任何指向文件夹的实体都会被监控，注意一些资源的修改比如静态assets，师徒模板不需要重启应用。
```
触发重启
由于Devtools监视类路径资源，因此触发重新启动的唯一方法是更新路径。导致更新路径的方式取决于你使用的IDE。在Eclipse中，保存修改后的文件会导致更新类路径并触发重新启动。在IDE中，构建项目（`Build -> Build Project`）具有相同的效果。
```
> 只要启用了分叉，你也可以使用受支持的构建插件（Maven和Gradle）启动应用程序，因为Devtools需要一个独立的应用程序类加载器才能正常运行。默认情况下，Gradle和Maven在类路径上检测到Devtools时会这样做。

> 与LiveReload一起使用时，自动重启非常有效。吐过使用JRebel，则禁用自动重新启动以支持动态类重新加载。其他devtools功能仍然可以使用。

> DevTools依赖应用程序上下文的关闭钩子来在重启期间关闭它。如果禁用了shutdown hook（`SpringApplication.setRegisterShutdownHook(false)`）,它将无法正常工作

> 当决定是否在类路径中的条目应该触发重新启动时，它的变化，DevTools自动忽略命名的项目`spring-boot`，`spring-boot-devtools`,`spring-boot-autoconfigure`,`spring-boot-actuator`,和`spring-boot-starter`。

> Devtools需要自定义`ResourceLoader`使用的`ApplicationContext`。如果你的应用程序已经提供了一个，它将被包装。不支持直接覆盖`getResource`方法`ApplicationContext`。

```
Restart vs Reload
Spring Boot提供的重启技术使用两个类加载器。不更改的类（例如，来自第三封jar的类），将加载到基 类加载器中。您正在积极开发的类将加载到重新启动的 类加载器中。重新启动应用程序时，将重新启动重新启动的类加载器并创建一个新的类加载器。这种方法意味着应用程序重新启动通常比“冷启动”快得多，因为基本类加载器已经可用并已填充。

如果您发现重新启动对于您的应用程序来说不够快，或者遇到类加载问题，您可以考虑从ZeroTurnaround 重新加载JRebel等技术 。这些工作通过在加载类时重写类以使它们更适合重新加载。
```
###### 记录条件评估中的变化
默认情况下，每次应用程序重新启动时，都会记录一个显示条件评估增量的报告。该报告显示了在进行更改（例如添加或删除Bean以及设置配置属性）时应用程序自动配置的更改。
要禁用报告的日志记录，请设置一下属性：
```
spring.devtools.restart.log-condition-evaluation-delta=false
```
###### 排除资源
某些资源在更改时不一定需要出发重启。例如，可以就地编辑Thymeleaf模板。默认情况下，在改变资源`/META-INF/maven`, `/META-INF/resources`,`/resources`或 `/templates`不会触发重启但会触发实时加载，如果要自定义这些排除项，可以使用该`spring.devtools.restart.exclude`属性。例如，要仅排除`/static`,`/public`你将设置以下属性：
```
spring.devtools.restart.exclude=static/**,public/**
```
> 如果要保留这些默认值并添加其他排除项，请改用该`spring.devtools.restart.additional-exclude`属性。

###### 查看其他路径
当你对不在路径中的文件进行更改时，你可能希望重新启动或重新加载应用程序。为此，请使用该`spring.devtools.restart.additional-paths`属性配置其他路径以监视更改。你可以使用前面描述的`speing.devtools.restart.exclude`属性来控制其他路径下的更改是触发完全重新启动还是事实及重新加载
###### 禁用重启
如果你不想使用重新启动功能，可以使用该`spring.devtools.restart.enabled`属性将其禁用。在打多少情况下，你可以在你的设置中设置此属性`application.properties`(这样做扔会初始化重新启动的类加载器，但它不会监视文件更改)。
如果需要完全禁用重新启动支持（例如，因为他不能与特定库一起使用），则需要在调用之前将`spring.devtools.restart.enabled` `system`属性设置为，如下示例所示:`false` `SpringApplication.run(...)`
```
public static void main(String[] args){
    System.setProperty("spring-devtools.restart.enabled","false");
    SpringApplication.run(MyApp.class,args);
}
```
###### 使用触发文件
如果使用不断编译已更改文件的IDE，则可能更喜欢仅在特定时间触发重新启动。为此，你可以使用"触发器文件",这是一个特殊we年，当你想要实际触发重新启动检查时，必须对其进行修改。更改文件只会触发检查，只有在Devtools检测到必须执行某些操作时才会重新启动。触发文件可以手动更新，也可以使用IDE插件更新。
要使用触发器文件，请将该`spring.devtools.restart.trigger-file`属性设置为触发器文件的路径。
> 你可能希望将其设置`spring.devtools.restart.trigger-file`为全局设置，以便所有项目的行为方式相同。

###### 自定义restart类加载器
如前面在Restart vs Reload部分中所述，使用两个类加载器实现了重启功能。对于大多数应用程序，这种方法很有效。但是，它有事会导致类加载器问题。
默认情况下，IDE中任何打开项目都使用“restart”类加载器加载，并且任何常规`.jar`文件都使用“base”类加载器加载。如果你处理多模块项目，并且并非每个模块都导入到IDE中，则可能需要自定义内容。为此，你可以创建一个`META-INF/spring-devtools.properties`文件。
该`spring-devtools.properties`文件可以包含前缀为`restart.exclude`和的属性`restart.include`。该`include`元素是应该被拉高到重启的类加载器的项目，以及`exclude`要素是应该向下推入基地类加载器的项目。属性的值是应用于类路径的正则表达式模式，如以下示例所示：
```
restart.exclude.companycommonlibs=/mycorp-common-[\\w-]+\.jar
restart.include.projectcommon=/mycorp-myproj-[\\w-]+\.jar
```
> 所有属性键必须是唯一的。只要以`restart.include`或者`restart.exclude`开头的都会考虑进去。所有来自`classpath`的`META-INF/spring-devtools.properties`都会被加载，你可以将文件打包进工程或者工程使用的库里。

###### 已知限制
对于使用标准反序列化的对象，重新启动功能不起作用`ObjectInputStream`。如果你需要反序列化的数据，你可能需要使用Spring的`ConfigurableObjectInputStream`结合`Thread.currentThread().getContextClassLoader()`。
不幸的是，几个第三方库反序列化而不考虑上下文类加载器。如果你发现此类问题，则需要原作者请求修复。
###### LiveReload
该`spring-boot-devtools`模块包括一个嵌入式LiveReload服务器，可用于在更改资源时触发浏览器刷新。LiveReload浏览器扩展程序可从[livereload.com](http://livereload.com/extensions/)免费用于Chrome，Firefox和Safari。
如果你不想在应用程序运行时启动LiveReload服务器，则可以将`spring.devtools.livereload.enabled`属性设置为`false`。
> 你一次只能运行一个LiveReload服务器。在启动应用程序之前，请确保没有其他LiveReload服务器正在运行。如果从IDE启动多个应用程序，则只有第一个局哟LiveReload支持。

###### 全局设置
你可以通过添加一个文件名为配置全局devtools设置`.spring-boot-devtools.properties`你的`$HOME`文件夹（注意：文件名开头“.”）。添加到此文件的任何属性都适用于及时算计上使用devtools的所有Spring Boot应用程序。例如，要将restart配置为始终使用触发器文件，你需要添加以下属性：
**~/.spring-boot-devtools.properties. **
```
spring.devtools.reload.trigger-file=.reloadtrigger
```
> 激活的配置文件`.spring-boot-devtools.properties`不会影响特定于配置文件的加载。

###### 远程应用程序
Spring Boot开发人员工具不仅限于本地开发。远程运行运用程序时，你还可以使用多个功能。远程支持是选择加入。要启用它，你需要确保它`devtools`包含在重新打包的存档中，如下面所示：
```
<build>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
			<configuration>
				<excludeDevtools>false</excludeDevtools>
			</configuration>
		</plugin>
	</plugins>
</build>
```
然后，你需要设置`spring.devtools.remote.secret`属性，如下所示：
```
spring.devtools.remote.secret = mysecret
```
> `spring-boot-devtools`在远程应用程序上启用存在安全风险。你永远不应该在生产部署上启用支持。

远程devtools支持分为两个部分：接受连接的服务器端端点和在IDE运行的客户端应用程序。`spring.devtools.remote.secret`设置属性后，将自动启用服务器组件。必须手动启动客户端组件。
###### 运行远程客户端应用程序
远程客户端应用程序旨在从IDE中运行。你需要`org.springframework.boot.devtools.RemoteSpringApplication`使用与连接到远程项目相同的类路径运行。应用程序的单个必须参数是它连接的远程URL。
例如，如果你使用的是Eclipse或STS，并且你有一个名为`my-app`已经部署到Cloud Foundry的项目，那么你将执行以下操作：
- 选择`Run Configurations...`从`Run`菜单。
- 创建一个新的`Java Application`启动配置。
- 浏览`my-app`项目。
- 用`org.springframework.boot.devtools.RemoteSpringApplications`做主类。
- 添加`https://myapp.cfapp.io`到`Program arguments`(或任何远程URL)。

正在运行的远程客户端可能类似于以下列表：
```
 .   ____          _                                              __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _          ___               _      \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` |        | _ \___ _ __  ___| |_ ___ \ \ \ \
 \\/  ___)| |_)| | | | | || (_| []::::::[]   / -_) '  \/ _ \  _/ -_) ) ) ) )
  '  |____| .__|_| |_|_| |_\__, |        |_|_\___|_|_|_\___/\__\___|/ / / /
 =========|_|==============|___/===================================/_/_/_/
 :: Spring Boot Remote :: 2.1.3.RELEASE

2015-06-10 18:25:06.632  INFO 14938 --- [           main] o.s.b.devtools.RemoteSpringApplication   : Starting RemoteSpringApplication on pwmbp with PID 14938 (/Users/pwebb/projects/spring-boot/code/spring-boot-devtools/target/classes started by pwebb in /Users/pwebb/projects/spring-boot/code/spring-boot-samples/spring-boot-sample-devtools)
2015-06-10 18:25:06.671  INFO 14938 --- [           main] s.c.a.AnnotationConfigApplicationContext : Refreshing org.springframework.context.annotation.AnnotationConfigApplicationContext@2a17b7b6: startup date [Wed Jun 10 18:25:06 PDT 2015]; root of context hierarchy
2015-06-10 18:25:07.043  WARN 14938 --- [           main] o.s.b.d.r.c.RemoteClientConfiguration    : The connection to http://localhost:8080 is insecure. You should use a URL starting with 'https://'.
2015-06-10 18:25:07.074  INFO 14938 --- [           main] o.s.b.d.a.OptionalLiveReloadServer       : LiveReload server is running on port 35729
2015-06-10 18:25:07.130  INFO 14938 --- [           main] o.s.b.devtools.RemoteSpringApplication   : Started RemoteSpringApplication in 0.74 seconds (JVM running for 1.105)
```
> 因为远程客户端使用与真实应用程序相同的类路径，所以它可以直接读取应用程序属性。这是如何`spring.devtools.remote.secret`读取属性并将其床底到服务器以进行身份验证。

> 始终建议使用`https://`连接协议，以便加密流量并且不会截获密码。

> 如果需要使用代理来访问远程应用程序，请配置`spring.devtools.remote.proxy.host`和`spring.devtools.remote.proxy.port`属性。

###### 远程更新
远程客户端与本地重新启动相同的当时监视应用程序类路径以进行更改。任何更新的资源都会被推送到远程应用程序，并且（如果需要）会触发重新启动。如果你迭代使用本地没有的云服务的功能，这将非常有用。通常，远程额更新和重新启动比完全重建和部署周期快得多。

> 仅在远程客户端运行时监视文件。如果在启动远程客户端之前更改文件，则不会将其推送到远程服务器。