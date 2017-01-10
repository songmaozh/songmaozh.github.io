---
title: SpringBoot Help
date: 2017-01-10 14:39:17
tags:
---
# Spring Boot MVC
## 使用Spring Boot
你可以像使用标准的Java库文件一样使用Spring Boot。简单的将需要的 spring-boot-*.jar 添加到classpath即可。
 Spring Boot不要求任何特殊的工具集成，所以可以使用任何IDE，甚至文本编辑器。
 
只是，仍然建议使用build工具：Maven 或 Gradle。
 
    Spring Boot依赖 使用 org.springframework.boot groupId 。
    通常，让你的Maven POM文件继承 spring-boot-starter-parent，并声明一个或多个 Starter POMs依赖即可。Spring Boot也提供了一个可选的 Maven Plugin来创建可执行的jars。 
```python
    <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.4.3.RELEASE</version>
</parent>
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

## 环境准备

一个称手的文本编辑器（例如Vim、Emacs、Sublime Text）或者IDE（Eclipse、Idea Intellij）
Java环境（JDK 1.7或以上版本）
Maven 3.0+（Eclipse和Idea IntelliJ内置，如果使用IDE并且不使用命令行工具可以不安装）
一个最简单的Web应用

## pom配置依赖
使用Spring Boot框架可以大大加速Web应用的开发过程，首先在Maven项目依赖中引入spring-boot-starter-web：

pom.xml

```python
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework</groupId>
    <artifactId>gs-maven</artifactId>
    <packaging>jar</packaging>
    <version>0.1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.4.3.RELEASE</version>
    </parent>
    <dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>

    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.7.0</version>
        </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-annotations</artifactId>
        <version>2.7.0</version>
    </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                </configuration>
            </plugin>
        </plugins>
 
    </build>


</project>
```


## Controller 含main方法
SampleController.java
```python
package hello;

import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.stereotype.*;
import org.springframework.web.bind.annotation.*;
import org.springframework.ui.Model;
import org.springframework.context.annotation.ComponentScan;

@RestController
@Controller
@ComponentScan
@EnableAutoConfiguration


// @Configuration  
// @ComponentScan  
public class SampleController {

    @RequestMapping("/")
    @ResponseBody
    String home() {
        return "Hello World222!";
    }

    @RequestMapping("/hello")
    public String hello() {
        return "Hello World!";
    }

    @RequestMapping("/index")
    public String index(Model model){
 
        model.addAttribute("name","Ryan");
 
        return "index";
    }

    public static void main(String[] args) throws Exception {
        SpringApplication.run(SampleController.class, args);
    }

}
```

## Controller 不含main 的rest风格
HelloController.java

```python
package hello;
 
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.stereotype.*;
import org.springframework.web.bind.annotation.*;
import org.springframework.ui.Model; 
import java.util.HashMap;
import java.util.Map;
 
/**
 * Created by miaorf on 2016/6/19.
 */
@Controller
@RestController
@RequestMapping("/hello")
public class HelloController {
 
    @RequestMapping("/index")
    public String index(Model model){
 
        model.addAttribute("name","Ryan");
 
        return "index";
    }
 
 
    @RequestMapping("/json")
    @ResponseBody
    public Map<String,Object> json(){
        Map<String,Object> map = new HashMap<String,Object>();
        map.put("name","Ryan");
        map.put("age","18");
        map.put("sex","man");
        return map;
    }
}
```


## 运行应用：mvn spring-boot:run或在IDE中运行main()方法，在浏览器中访问http://localhost:8080

## 简单分析
我们从程序的入口SpringApplication.run(Application.class, args);开始分析：

* SpringApplication是Spring Boot框架中描述Spring应用的类，它的run()方法会创建一个Spring应用上下文（Application Context）。另一方面它会扫描当前应用类路径上的依赖，例如本例中发现spring-webmvc（由 spring-boot-starter-web传递引入）在类路径中，那么Spring Boot会判断这是一个Web应用，并启动一个内嵌的Servlet容器（默认是Tomcat）用于处理HTTP请求。

* Spring WebMvc框架会将Servlet容器里收到的HTTP请求根据路径分发给对应的@Controller类进行处理，@RestController是一类特殊的@Controller，它的返回值直接作为HTTP Response的Body部分返回给浏览器。

* @RequestMapping注解表明该方法处理那些URL对应的HTTP请求，也就是我们常说的URL路由（routing)，请求的分发工作是有Spring完成的。例如上面的代码中http://localhost:8080/ 根路径就被路由至greeting()方法进行处理。如果访问http://localhost:8080/hello ，则会出现404 Not Found错误，因为我们并没有编写任何方法来处理/hello请求。

* 例如HelloController中，method方法匹配的URL是/classPath/methodPath"。

 提示

可以定义多个@Controller将不同URL的处理方法分散在不同的类中