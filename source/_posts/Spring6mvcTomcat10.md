---
title: Spring6mvcTomcat10
katex: true
date: 2024-05-28 16:16:02
excerpt: jdk21 + spring6 mvc + tomcat10 + thymeleaf6 的配置文件
tag: java
---


在 idea 中配置 jdk21 + springmvc6 + tomcat10 +thymeleaf6 出现很多坑，下面是最终能跑通的配置

- Spring 6完全迁移到了Jakarta EE 9+，这意味着所有的包名都从javax前缀改为了jakarta前缀。例如，javax.servlet 变为 jakarta.servlet。
- Spring 6要求至少Java 17作为运行环境。这带来了对Java语言特性的更好支持和优化。
- Spring 6.0已经不再使用基于Apache Commons FileUpload的CommonsMultipartResolver，而是推荐使用基于Servlet标准的StandardServletMultipartResolver，这是基于Servlet 3.0+规范的实现，利用Servlet容器本身的multipart解析功能。它不依赖于外部库，而是直接使用Servlet API提供的multipart支持。



# pom.xml

创建 module，在pom中导入依赖配置，先修改packaging为war，springframework是6，tomcat10必须是jakarta.servlet，而不能是之前的javax，thymeleaf也要改成thymeleaf-spring6。最开始是网上找到配置文件，但一直跑500，最后根据warning调整了spring6的小版本跑通了

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.ys.mvc</groupId>
    <artifactId>HttpController</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>
    <properties>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <version>5.6.3</version>
            <scope>test</scope>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>6.0.7</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/ch.qos.logback/logback-classic -->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.4.12</version>
            <scope>test</scope>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
        <!--        <dependency>-->
        <!--            <groupId>javax.servlet</groupId>-->
        <!--            <artifactId>javax.servlet-api</artifactId>-->
        <!--            <version>4.0.1</version>-->
        <!--            <scope>provided</scope>-->
        <!--        </dependency>-->
        <!-- https://mvnrepository.com/artifact/jakarta.servlet/jakarta.servlet-api -->
        <dependency>
            <groupId>jakarta.servlet</groupId>
            <artifactId>jakarta.servlet-api</artifactId>
            <version>6.0.0</version>
            <scope>provided</scope>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.thymeleaf/thymeleaf-spring6 -->
        <!--        <dependency>-->
        <!--            <groupId>org.thymeleaf</groupId>-->
        <!--            <artifactId>thymeleaf-spring6</artifactId>-->
        <!--            <version>3.1.1.RELEASE</version>-->
        <!--        </dependency>-->
        <!-- https://mvnrepository.com/artifact/org.thymeleaf/thymeleaf-spring6 -->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring6</artifactId>
            <version>3.1.0.RELEASE</version>
        </dependency>
    </dependencies>

</project>


```
# web.xml

在src/main下创建webapp文件夹，在project structure中添加web.xml文件，记得修改文件路径，改到真正的webapp目录下，如果 pom.xml 中没有把packing改成war，这一步不会有web修改
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
```

```xml
    <!--编码过滤器，必须配置在最前面才能发挥作用，不然就是结果先通过过滤器，之后过滤器后设置编码，啥也没干-->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!--配置处理请求方式put和delete的HiddenHttpMethodFilter-->
    <filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>


    <!-- 前端控制器 -->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--        选择spring容器管理文件   管理mvc-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <!--        加载方式 程序运行时-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <!--        访问请求   / 接受所有请求  不包括.jps请求拒接   /* 接受所有请求 包括.jsp-->
        <url-pattern>/</url-pattern>
    </servlet-mapping>


```
扩展配置方式指定spring配置文件位置，这样可以把配置写到resources下


# springmvc.xml

在web.xml中指定spring配置文件springmvc.xml，添加配置文件之后web.xml中相应的字段就不会爆红

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <context:component-scan base-package="com.ys.mvc.controller"/>

    <!--    视图解析器-->
    <bean id="springResourceTemplateResolver" class="org.thymeleaf.spring6.templateresolver.SpringResourceTemplateResolver">
        <property name="prefix" value="/WEB-INF/templates/"/>
        <property name="suffix" value=".html"/>
        <property name="templateMode" value="HTML"/>
        <property name="characterEncoding" value="UTF-8"/>
    </bean>

    <bean id="templateEngine" class="org.thymeleaf.spring6.SpringTemplateEngine">
        <property name="templateResolver" ref="springResourceTemplateResolver"/>
    </bean>
    <bean id="viewResolver" class="org.thymeleaf.spring6.view.ThymeleafViewResolver">
        <property name="order" value="1"/>
        <property name="characterEncoding" value="UTF-8"/>
        <property name="templateEngine" ref="templateEngine"/>
    </bean>

    <mvc:view-controller path="/" view-name="index"></mvc:view-controller>
    <!--配置文件上传解析器，将上传文件自动封装为MutilpartFile对象-->
    <bean id="multipartResolver" class="org.springframework.web.multipart.support.StandardServletMultipartResolver"/>
    
    <!--配置tomcat默认的handler，用于开启静态资源的访问，如果Dispatcher找不到就会交给这个默认的去找-->
    <mvc:default-servlet-handler/>
    
    <!--开启注解驱动，很重要，有很多用处-->
    <mvc:annotation-driven/>

</beans>
```


在spring配置文件中指定组件扫描的位置，记得添加context的路径，指定thymeleaf的情况，记得class也要换成thymeleaf6的路径

根据thymeleaf中配置的前缀和后缀添加index.html
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
fuck yes
</body>
</html>

```

记得添加 `xmlns:th="http://www.thymeleaf.org"`

# pojo

```java
package com.ys.mvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloController {
    @RequestMapping("/")
    public String index(){
        return "index";
    }
}

```

将前端控制器的信号转发到index,html



