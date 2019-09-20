###### Spring Boot的特性
在这里将为你介绍Spring Boot的详细信息。在这里，你可以了解可能想要使用和自定义的主要功能。
###### SpringApplication
SpringApplication类提供了一种快捷方式，用于从main（）方法启动Spring应用。多说情况下，你只需要将该任务委托给SpringApplication.run静态方法：
```
public static void main(String[] args){
    SpringApplocation.run(MySpringConfiguration.class,args);
}
```
当你的应用程序启动时，你应该看到类似于以下输出内容：
```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::   v2.1.3.RELEASE

2013-07-31 00:08:16.117  INFO 56603 --- [           main] o.s.b.s.app.SampleApplication            : Starting SampleApplication v0.1.0 on mycomputer with PID 56603 (/apps/myapp.jar started by pwebb)
2013-07-31 00:08:16.166  INFO 56603 --- [           main] ationConfigServletWebServerApplicationContext : Refreshing org.springframework.boot.web.servlet.context.AnnotationConfigServletWebServerApplicationContext@6e5a8246: startup date [Wed Jul 31 00:08:16 PDT 2013]; root of context hierarchy
2014-03-04 13:09:54.912  INFO 41370 --- [           main] .t.TomcatServletWebServerFactory : Server initialized with port: 8080
2014-03-04 13:09:56.501  INFO 41370 --- [           main] o.s.b.s.app.SampleApplication            : Started SampleApplication in 2.992 seconds (JVM running for 3.658)
```
默认情况下，`INFO`会显示日志记录消息，包括一些相关的启动详细信息，例如启动应用程序的用户。如果你需要其他日志级别`INFO`，则可以进行设置。
###### 启动失败
如果你的应用程序无法启动，则已注册`FailureAnalyzers`有机会提供专用错误信息和具体操作来解决问题。例如，如果你在段扩上启动Web应用程序`8080`并且该端口已在使用中，你应该会看到类似于以下消息的内容：
```
***************************
APPLICATION FAILED TO START
***************************

Description:

Embedded servlet container failed to start. Port 8080 was already in use.

Action:

Identify and stop the process that's listening on port 8080 or configure this application to listen on another port.
```
> Spring Boot提供了许多`FailureAnalyzer`实现，你自己实现也很容易。

如果没有故障分析器能够处理异常，你仍然可以显示完整的条件报告，以便于更好的了解出现了什么问题。要做到这一点，因此你需要启用`org.springframework.boot.autoconfigure.logging.ConditionEvaluationReportLoggingListener`的`debug`属性或开启DEBUG日志级别。
例如，使用java -jar运行应用时，你可以通过如下你命令启debug属性：
```
D:\desktop\Spring Boot\target> java -jar .\myproject-0.0.1-SNAPSHOT.jar --debug
```
###### 自定义Banner
通过在classpath下添加一个banner.txt或设置banner.location来指定相应的文件可以改变启动过程中打印的banner。如果这个文件有特殊的编码，你可以使用banner.encoding设置它（默认为UTF-8）。除了文本文件，你也可以添加一个banner.gif，banner.jpg或banner.png图片，或设置banner.image.location属性。图片会转换为字符画（ASCII art）形式，并在所有文本banner上方显示。
在banner.txt中可以使用如下占位符:
| 变量                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ${application.version}                                       | 应用程序的版本号，如声明中所声明的`MANIFEST.MF`。例如`Implementation-Version: 1.0`打印为`1.0` |
| ${application.formatted-version}                             | `MANIFEST.MF`中声明的被格式化后的应用版本号（被括号包裹且以v作为前缀），用于显示，例如`(v1.0)` |
| ${spring-boot.version}                                       | 当前`Spring Boot`的版本号，例如`1.4.1.RELEASE`               |
| ${spring-boot.formatted-version}                             | 当前`Spring Boot`被格式化后的版本号（被括号包裹且以v作为前缀）, 用于显示，例如`(v1.4.1.RELEASE)` |
| ${Ansi.NAME}（或${AnsiColor.NAME}，${AnsiBackground.NAME}, ${AnsiStyle.NAME}） | `NAME`代表一种`ANSI`编码，具体详情查看`AnsiPropertySource`   |
| ${application.title}                                         | `MANIFEST.MF`中声明的应用`title`，例如`Implementation-Title: MyApp`会打印`MyApp` |
> `SpringApplication.setBanner(...)`如果要以变成方式生成横幅，则可以使用该方法。使用`org.springframework.boot.Banner`界面并实现自己的`printBanner()`方法。

你也可以使用spring.main.banner-mode属性决定将banner打印到何处，System.out（console），配置的logger（log）或都不输出（off)。
打印的banner将注册成一个名为springBootBanner的单例bean。
> 注 YAML会将off映射为false，如果想在应用中禁用banner，你需要确保off添加了括号：
```
spring:
    main:
        banner-mode:"off"
```

###### 自定义SpringApplication
如果`SpringApplication`默认值不符合你的要求，你可以改为创建本地实例并对其进行自定义。例如，想要关闭banner你可以这样写：
```
public static void main(String[] args) {
    SpringApplication app = new SpringApplication(MySpringConfiguration.class);
    app.setBanner(Banner.Mode.OFF);
    app.run(args);
}
```
> 传递给的构造函数参数`SpringApplication`是Spring bean的配置源。在大多数情况下，这些是对`@Configuration`类的引用，但他们也可以是对XML配置或应用扫描的包的应用。

也可以`SpringApplication`使用`application.properties`文件配置。

###### 流式构建API
如果需要创建一个分层的`ApplicationContext`（多个具有父子关系的上下文），或只是喜欢使用流式（fluent）构建API，那你可以使用`SpringApplicationBuilder`。`SpringApplicationBuilde`允许你以链式方式调用多个方法，包括parent和child方法，这样就可以创建多层次结构，例如：
```
new SpringApplicationBuilder()
    .sources(Parent.class)
    .child(Application.class)
    .bannerMode(Banner.Mode.OFF)
    .run(args);
```
> 创建`ApplicationContext`层次结构时存在一些限制。例如，web组件必须包含在子上下文中，并且`Environment`父组件和子上下文都使用相同的组件。

###### SpringApplication事件和监听器
除了常见的Spring框架事件，比如ContextRefreshedEvent，`SpringApplication`也会发送其他的application事件。
> 有些事件实际上是在`ApplicationContext`创建钱触发的，所以你不能再那些事件（处理类）中通过`@Bean`注册监听器，只能通过`SpringApplication.addListeners(...)`或`SpringApplicationBuilder.listeners(...)`方法注册。如果想让监听器自动注册的，而不关心应用的创建方式，你可以在工程中添加一个`META-INF/spring.factories`文件，并使用`org.springframework.context.ApplicationListener`作为key指向那些监听器，如下：
```
org.springframework.context.ApplicationListener = com.example.project.MyListener
```

应用程序运行时，应按一下顺序发送应用程序事件：
1、在运行开始，但除了监听器注册和初始化意外的任何处理之前，会发送一个`ApplicationStartedEvent`。
2、在Environment将被用于一直的上下文，但在上下文被创建前，会发送一个`ApplicationEnvironmentPreparedEvent`。
3、在refresh开始前，但在bean定义以备加载后，会发送一个`ApplicationPreparedEvent`。
4、在refresh之后，相关的回调处理完，会发送一个`ApplicationReadyEvent`，标识应用准备好接受请求了。
5、启动过程中如果出现异常，会发送一个`ApplicationFailedEvent`。
> 通常不需要使用application事件，但知道他们的存在是有的（在某些场合可能会使用到），比如，在Spring Boot内部会使用事件处理各种任务。

使用`Spring Framework`的事件发布机制发送应用程序事件。此机制的一部分确保发布到子上下文中的侦听器的事件也发布到任何祖先上下文中的侦听器。因此，如果您的应用程序使用`SpringApplication`实例层次结构，则侦听器可能会收到相同类型的应用程序事件的多个实例。

为了允许侦听器区分其上下文的事件和后代上下文的事件，它应该请求注入其应用程序上下文，然后将注入的上下文与事件的上下文进行比较。`ApplicationContextAware`如果监听器是`bean`，则可以通过使用 来注入上下文`@Autowired`。
###### 网络环境
`SpringApplication`尝试以`ApplicationContext`你的名义创建正确的类型。用于确定a的算法`WebEnvironmentType`非常简单：
- 如果存在Spring MVC，`AnnotationConfigServletWebServerApplicationContext`则使用一个
- 如果Spring MVC不存在并且Spring WebFlux存在，`AnnotationConfigReactIveWebApplicationContext`则使用一个
- 否则，使用`AnnotationConfigApplicationContext`

这意味着如果你`WebClient`在同一个应用程序中使用Spring MVC和Spring WebFlux中的新功能，默认情况下会使用Spring MVC。你可以通过调用轻松的覆盖它`setWebApplicationType（WebApplicationType）`。
也可以完全控制ApplicationContext呼叫使用类型setApplicationContextClass（...）。
> 在JUnit测试中setWebApplicationType(WebApplicationType.NONE)使用时经常需要调用`SpringApplication`。

###### 访问应用程序参数
如果需要访问传递给的应用程序参数，`SpringApplication.run(...)`可以注入一个`org.springframework.boot.applicationArguments`类型的bean。`SpplicationArguments`接口即提供对原始String[]参数的访问，也提供对解析成option和non-option参数访问:
```
import org.springframework.boot.*;
import org.springframenwork.beans.factory.annotation.*;
import org.springframenwork.stereotype.*;

@Component
public class Mybean {
    
    @Autowired
    public MyBean(ApplicationArguments args){
        boolean debug = args.containsOption("debug");
        List<String>files = args.getNonOptionArgs();
        // if run with "--debug logfile.txt" debug = true,files = ["logfile.txt"]
    }
    
}
```

> Spring Boot也会注册一个包含`Spring Environment`属性的`ConnandLinePropertySource`,这就允许你使用@Value注解注入单个应用参数。

###### 使用ApplicationRunner或CommandLineRunner
如果需要在`SpringApplication`启动或执行一些特殊的代码啊，你可以实现`ApplicationRunner`或`CommandLineRunner`接口，这两个接口工作方式相同，都只是提供单一的run方法，该方法仅在`SpringApplication.run(...)`完成之前调用。
`CommandLineRunner`接口能够访问string数组类型的应用参数，而`ApplicationRunner`使用的是上面描述过的`ApplicationArguments`接口：
```
import org.springframework.boot.*;
import org.springframework.stereotype.*;

@Component
public class MyBean implements CommandLineRunner {
    
    public void run(String.. args){
        // Do something...
    }
}
```
如果某些定义的`CommandLineRunner`或`ApplicationRunner` beans需要以特定的顺序调用，你可以实现`org.springframework.core.Ordered`接口或使用`org.springframework.core.annotation.Order`注解。
###### Application退出
每个`SpringApplication`向JVM注册一个关闭挂钩以确保`ApplicationContext`退出时正常关闭。所有标准的Spring生命周期回调（例如`DisposableBean`接口或`@PreDestroy`注释）都可以使用。另外,`org.springframework.boot.ExitCodeGenerator`如果`bean`在`SpringApplication.exit()`调用时希望返回特定的退出代码，它们可以实现该接口。然后可以将此退出代码传递给`System.exit()`状态代码，如以下示例所示:
```
@SpringBootApplication
public class ExitCodeApplication {
    
    @Bean
    public ExitCodeGenerator exitCodeGenerator(){
        return () -> 42;
    }
    
    public static void main(String[] args) {
        System.exit(SpringApplications.exit(SpringApplication.run(ExitCodeApplication.class,args)));
    }
    
}
```
而且，ExitCodeGenerator界面可以通过例外来实现。遇到这样的异常时，Spring Boot将返回由实现的getExitCode()方法提供的退出代码。

###### 管理功能
通过制定`spring.application.admin.enabled`属性可以为应用程序启用与管理的相关功能。这暴露`SpringApplicationAdminMXBean`了平台上`MBeanServer`。你可以使用此功能远程管理你的`Spring Boot`应用程序。此功能对于任何服务包装器实现也可能有用。
如果你想知道应用程序在哪个HTTP端口上运行，请使用秘钥获取属性`loca.server.port`。

> **警告：**启用此功能时要小心，因为MBean公开了关闭应用程序的方法。