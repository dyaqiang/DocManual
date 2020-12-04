

## 第一章、入门



### 1.1创建工程

用maven 创建工程

### 1.2导入依赖

```xml
 <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>   //web模块
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```



### 1.3编写主程序

作用启动springboot 应用

```java
@SpringBootApplication
public class HelloWorld {
    public static void main(String[] args) {
        SpringApplication.run(HelloWorld.class, args);
    }
}
```

### 1.4编写controller

```java
@Controller
public class HelloController {
    @ResponseBody
    @RequestMapping("/hello")
    public String hello() {
        return "hello";
    }
}
```

### 1.5运行测试

### 1.6简化部署

Package

### 1.7快速启动  initialize

## 第二章 springboot 的启动逻辑



### 1.1分析

#### 1.7.1启动器

##### pom 文件

```xml
父项目
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
</parent>
他的父项目
<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-dependencies</artifactId>
		<version>1.5.9.RELEASE</version>
		<relativePath>../../spring-boot-dependencies</relativePath>
</parent>
  管理springboot的所有依赖版本，以后倒入默认不需要写版本的
```

##### web启动器

功能场景都分离出来，加入启动器，用到什么场景就导入什么场景启动器

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

#### 1.7.2 自动配置

##### 主程序类

```java
@SpringBootApplication
public class HelloWorld {
    public static void main(String[] args) {
        SpringApplication.run(HelloWorld.class, args);
    }
}
```

@**SpringBootApplication**  

1、标注在那个类上 说明是springboot 的主配置类，2、springboot 就会运行这个类的main 方法启动应用。

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
```

@**SpringBootConfiguration**  springboot 的配置类

标注在某个类上说明是springboot 的配置类

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
}
```

@**Configuration**  配置类上的注解



==@**EnableAutoConfiguration** 开启自动配置功能==
        @**AutoConfigurationPackage** 自动配置包：@Import({Registrar.class})

​       ==将主配置类下的包及包下的所有组件扫面进入spring容器中==

```java
public class EnableAutoConfigurationImportSelector（） 导入自动配置类
```







