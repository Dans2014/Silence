# JAVA

### 反射

> JVM为每个加载的`class`创建了对应的`Class`实例，并保存了该`class`的所有信息，包括类名、包名、父类、实现的接口、所有方法、字段等。
>
> 通过`Class`实例获取`class`信息的方法即为 **反射**。





# Springboot

### Hello World

新建maven项目，pom文件添加

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.6.RELEASE</version>
</parent>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

主启动类

```java
@SpringBootApplication
@RestController
public class MainApplication {

    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class, args);
    }

    @RequestMapping("/")
    public String test(){
        return "Hello World";
    }
}
```

### 自动装配

