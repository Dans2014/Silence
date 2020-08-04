# JAVA

### 反射

> JVM为每个加载的`class`创建了对应的`Class`实例，并保存了该`class`的所有信息，包括类名、包名、父类、实现的接口、所有方法、字段等。
>
> 通过`Class`实例获取`class`信息的方法即为 **反射**。

**动态加载** 

> JVM并不是一次性把所有用到的class全部加载到内存，而是在第一次需要使用到class时才加载。

**获取Class实例**

```java
Class cls1 = String.class;
Class cls2 = "Lcy".getClass();
Class cls3 = Class.forName("java.lang.String");
```

**访问字段**

```java
Person p = new Person("Lcy");
Class cls = p.getClass();
Field f = cls.getDeclaredField("name");    //获取非public字段
f.setAccessible(true);      			//访问非public字段
Object o = f.get(p);        			//获取字段值
System.out.println(f.get(p));       	//"Lcy"
f.set(p, "Lcyan");             			//设置字段值
System.out.println(f.get(p));       	//"Lcyan"
```

**调用方法**

```java
Method m = cls.getDeclaredMethod("setName", String.class);
m.setAccessible(true);
m.invoke(p, "Lcyannaycl");
System.out.println(f.get(p));
```

**调用构造方法**

```java
Constructor c = Person.class.getConstructor(String.class);
Person p2 = (Person) c.newInstance("lcy");
```

**继承与接口**

```java
System.out.println(String.class.getSuperclass());

Class[] cls = String.class.getInterfaces();
for(Class c: cls){
	System.out.println(c);
}
```



### 异常



### 注解



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

