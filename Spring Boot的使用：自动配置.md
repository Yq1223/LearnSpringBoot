###### Spring Boot的使用：自动配置
Spring Boot自动配置（Auto-configuration）尝试根据添加jar依赖自动配置你的Spring应用。例如，如果classPath下存在`HSQLDB`，并且你没有手动配置任何数据库连接beans，那么Spring Boot将自动配置一个内存星（in-memory）数据库。
实现自动配置有两种可选方式，分别将`@EnableAutoConfiguration`或`@SpringBootApplication`注解到`@configguration`类上。
> 你应该只添加一个`@EnableAutoConfiguration`注解，通常建议将它添加到主配置（primary`Configuration`）上。

###### 逐步替换自动配置
自动配置（Auto-configguration）是非侵入性的，任何时候你都可以定义自己的配置类来替换的特定部分。例如，如果你添加自己的`DataSource`bean，默认的内嵌数据库支持不考虑。
如果需要查看当前应用启动了那些自动配置项，你可以在运行应用时打开`--debug`开关，这将为核心入职开启debug日志级别，并将自动配置相关的日志输出到控制台。
###### 禁用特定的自动配置类
如果发现启用了不想要的自动配置项，你可以使用`@EnableAutoConfiguration`注解的wxclude属性禁用他们，如以下示例所示：
```
import org.springframework.boot.autoconfigure.*;
import org.springframework.boot.autoconfigure.jbdc.*;
import org.springframework.comtext.annotation.*;

@Configuration
@EnableAutoConfiguration(exclue={DataSoureAutoConfiguration.class})
public class MyConfiguration{
    
}
```
如果该类不在classpath中，你可以使用该注解的excludeName属性，并指定全限定名来达到相同效果。最后，你可以通过`spring.autoconfigure.exclude`属性exclude多个自动配置项（一个自动配置项集合）。

> 通过注解级别或exclude属性可以定义排除项。