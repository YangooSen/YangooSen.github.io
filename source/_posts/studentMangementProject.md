---
title: studentMangementProject
katex: true
date: 2024-11-06 23:17:04
excerpt: springboot3+vue3练习
tag: java
---

# 注意点

## 配置
![config](config.png)

- 除了必用的那些，其他配置的作用是什么

## application.properties
```python
spring.application.name=StudentMangement

spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=Ww1043790919!
spring.datasource.url=jdbc:mysql://localhost:3306/student_mangement?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8&useSSL=true
#最小空闲连接数量
spring.datasource.hikari.minimum-idle=10
#连接池最大连接数目
spring.datasource.hikari.maximum-pool-size=10
#空闲连接存活最大事件
spring.datasource.hikari.idle-timeout=120000
#数据库连接超时时间
spring.datasource.hikari.connection-timeout=30000
spring.datasource.hikari.connection-test-query=Select 1

#视频里用的jpa没有写，自己用的是mybatis

```
- url中后面跟的参数是什么
- hikari数据源的配置是什么东西

## base

### BaseResult
- 为什么要实现Serializable接口
- 改进：可以把状态码，异常什么的做成枚举类，不需要自己写

### BadRequestException

```java
package com.ys.exception;


import lombok.Getter;
import org.springframework.http.HttpStatus;
@Getter
public class BadRequestException extends RuntimeException {
    private Integer status= HttpStatus.BAD_REQUEST.value();
    public BadRequestException(String message) {
        super(message);
    }
    
    public BadRequestException(String message, HttpStatus status) {
        super(message);
        this.status = status.value();
    }
}

```
- org.springframework.http.HttpStatus 的使用，没见过
- RuntimeException 实现子类

###
```java
package com.ys.exception;

import java.io.PrintStream;
import java.io.PrintWriter;
import java.io.StringWriter;

public class ThrowableUtil {

    public static String getStackTrace(Throwable throwable){
        StringWriter stringWriter = new StringWriter();
        try(PrintWriter pw=new PrintWriter(stringWriter)){
            throwable.printStackTrace(pw);
            return stringWriter.toString();
        }
    }
}

```
- getStackTrace 的实现中用的的东西，和为什么要实现这个

### annotation

这个似乎是用Spring Data JPA的，但自己改成了用mybatis，因此没写

### ConfigurerAdapter

```java
package com.ys.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class ConfigurerAdapter implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedHeaders("*")
                .allowedMethods("*")
                .maxAge(1800)
                .allowCredentials(true)
                .allowedOriginPatterns("*");
    }
}
```
- 配置跨域，这个没用过

### pinia-plugin-persistedstate

```typescript
import { createPinia } from "pinia";
import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'

const pinia=createPinia()
pinia.use(piniaPluginPersistedstate)

export default pinia
```

- 没用过

### element-plus
```typescript
import 'element-plus/dist/index.css'
//导入所有的element-plus图标
import * as ElementPlusIconsVue from '@element-plus/icons-vue'
for (const [key,component] of Object.entries(ElementPlusIconsVue)){
    app.component(key,component)
}


app.use(ElementPlus)
```
- 没用过

### svgicon组件
- 没用过，需要配置插件和组件

### nprogress
- 没用过
- 配置路由拦截守卫是什么


