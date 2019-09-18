###### Spring Boot的使用：Spring Beans和依赖注入
你可以自由的使用任何标准的Spring框架技术定义beans和他们注入的依赖。简单起见，我们经常使用`@ComponentScan`注解搜索beans，并结合`@Autowired`构造器注入。
如果遵循以上的建议组织代码结构（将应用的main类放到包的最上层，即root package），那么你就可以添加`@ComponentScan`注解而不需要任何参数，所有应用组件（`@Component`,`@service`,`@repository`,`@controller`等）都会自动注册成Spring Beans。
下面是一个`@service`Bean的实例，它使用构建器注入获取一个需要的`RiskAssessor`bean。
```
package com.example.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class DatabaseAccountService impements AccountService{

    private final riskAssessor riskAssessor;
    
    @Autowired
    public DatabaseAccountService(RiskAssessor riskAssessor){
        this.riskAssessor = riskAssessor;
    }
    
    // ...
    
}
```
如果一个bean有一个构造函数，则可以省略`@Autowired`，如下示例所示：
```
@service 
public class DatabaseAccountService imPlements AccountService{
    
    private final RiskAssessor riskAssessor;
    
    public DatabaseAccountService(RiskAssessor riskAssessor){
        this.riskAssessor = riskAssessor;
    }
    
    // ...
    
}
```

> 注意使用构建起注入允许`riskAssessor`字段被标记`final`，这以为这`riskAssessor`后续是不可能改变的。