###### Spring Boot的使用:使用@SpringBootApplication注释
很多Spring Boot开发者经常使用`@configuration`,`@EnableAutoConfiguration`,`@ComponentScan`注解他们的main类，由于这些注解如此频繁的一块使用（特别是遵循以上最佳实践的时候），Spring Boot就提供`@SpringBootApplication`可以使用单个注释来用这三个功能，即：
- `@EnableAutoConfiguration`:启用Spring Boot的自动配置机制
- `@ComponentScan`:`@Component`在应用程序所在的报上启用扫描
- `@Configuration`:允许在上下文中注册额外的bean或导入其他配置类的`@SpringBootApplication`注释是相当于使用`@Configuration`,`@EnableAutoConfiguration`以及`@ComponrntScan`与他们的默认属性，如下面的例子：

```
package com.exmple.myapplication;

import org.springframework.SpringApplication;
import org.springframework.boot.sutoconfigure.SpringBootApplication;

@SpringBootApplication // same as @Configuration @EnableAutoConfiguration @ComponentScan
public class Application {
        
    public static void main(String[] args){
        SpringApplication,run(Application.class,args);
    }
}
```
> `@springBootApplication`注解也提供了用于自定义`@EnableAutoConfiguration`和`@ComponentScan`属性的别名（aliases）

> 这些功能都不是必须的，你可以选择通过它启用的任何动能替换此单个注释。例如：你可能不想在应用程序中使用组件扫描：
```
package com.example.myapplication;

import org.springframework.boot.SpringApplication;
import org.springframework.context.annotation.ComponentScan
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

@Configuration
@EnableAutoConfiguration
@Import({ MyConfig.class, MyAnotherConfig.class })
public class Application {

	public static void main(String[] args) {
			SpringApplication.run(Application.class, args);
	}

}
```
> 在此实例中，`Application`与任何其他Spring Boot应用程序一样，除了`@Component`未自动检测到注释类并且显示导入用户定义的bean。