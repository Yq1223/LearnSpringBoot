###### 构建你的代码
Spring Boot不需要任何特定的代码那部剧即可工作。但是，有一些最佳实践可以提供帮助。
###### 使用default（默认）的包
当一个类没有声明`package`时，它被认为处于`default package`下。通常不推荐使用`default package`，因为对于使用`@ComponentScan`，`@EntityScan`或`@SpringBootApplication`注解的Spring Boot应用来说，他会扫描每个jar中的类，这回造成一定的问题。
> 我们建议您遵循Java推荐的包命名约定并使用反向域名（例如，`com.example.project`）。

###### 定位主应用程序类
通常建议将应用的main类放在其他类所在包的顶层（root package），并将`@EnableAutoConfiguration`注解到你的main类上，这样就隐式定义了一个基础的包搜索路径（search package），以搜索某些特定的注解实体（比如@Service，@Component等）。例如，如果你正在编写一个JPA应用，spring将搜索`@EnableAutoConfiguration`注解的类所在包下的`@Entity`实体。
采用root package方式，你就可以使用`@ComponentScan`注解而不需要指定`basePackage`属性，你可以使用`@SpringBootApplication`注解，只要将main类放到root package中。
下面是一个典型的结构：
```
com
    +- example
        +- myapplication
            +- Application.java
            |
            +- customer
            |   +- Customer.java
            |   +- CustomerController.java
            |   +- CustomerService.java
            |   +- CustomerRepository.java
            |
            +- order
                +- Order.java
                +- OrderController.java
                +- OrderService.java
                +- OrderRepository.java
```
`Application.java`将声明`main`方法，还有基本的`@Configuration`。如下所示：
```
package com.example.myapplication;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application{
    
    public static void main(String[] args){
        SpringApplication.run(Application.class,args);
    }
}
```