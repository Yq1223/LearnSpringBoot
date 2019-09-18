###### Spring Boot的使用：配置类
Spring Boot提倡基于Java配置。尽管你可以使用XML源调用`SpringApplication.run()`,不过还是建议你使用`@Configuration`类作为主要配置源。通常定义了`main`方法的类也是使用`@Configuration`注解的一个很好的替补。
> 虽然网络上有很多使用XML配置的Spring示例，但你应该尽可能的使用基于Java的配置，搜索查看`enable*`注解是一个好的开端。
###### 导入其他配置类
你不需要把所有的`@Configuration`东西都放在一个类上。该`@Import`注释可以用于导入其他配置类。或者，你可以使用`@ComponentScan`自动获取所有Spring组件，包括`@Configuration`类。
###### 导入XML配置
如果你必须使用基于XML的配置，我们建议你仍然从一个`@Configuration`类开始。然后你可以使用`@ImportResource`注释来加载XML配置文件。