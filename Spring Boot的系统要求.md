###### Spring Boot的系统要求
Spring Boot 2.1.3.RELEASE需要<font color=#0099ff>Java 8</font>，并且与Java 11兼容（包括在内）。还需要<font color=#09f>Spring Framework 5.1.5.RELEASE</font>或更高版本。
为以下构建工具提供了显示构建支持:
| 构建工具 | 版本 |
| -------- | ---- |
| Maven    | 3.3+ |
| Gradle   | 4.4+ |
###### Servlet容器
Spring Boot支持以下嵌入式servlet容器：
| 名称         | servlet版本 |
| ------------ | ----------- |
| Tomact 9.0   | 4.0         |
| Jetty 9.4    | 3.1         |
| Undertow 2.0 | 4.0         |
你还可以将Spring Boot应用程序部署到任何Servlet3.1+兼容容器。