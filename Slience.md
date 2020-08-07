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

异常继承关系如下：

![](D:\Silence\assets\Throwable.PNG)

自定义异常

```java
public class MyException extends Exception {
    public MyException() {
    }
    public MyException(String str){
        super(str);
    }
}
```

抛出及捕获

```java
public static void Func1() throws MyException {
	throw new MyException("This is a test Exception");
}

public static void Func2() throws MyException {
	Func1();
}

public static void main(String[] args) {
    try{
    	Func2();
    }catch (MyException e){
    	e.printStackTrace();
    }
}
```

输出

```
com.lcy.MyException: This is a test Exception
	at com.lcy.MyException.Func1(MyException.java:11)
	at com.lcy.MyException.Func2(MyException.java:15)
	at com.lcy.MyException.main(MyException.java:20)
```



### 注解



### 泛型

泛型类实际上是Object

泛型的继承关系：可以把`ArrayList`向上转型为`List`（`T`不能变！），但不能把`ArrayList`向上转型为`ArrayList`（`T`不能变成父类）



### 网络



### 并发



### 锁





# JVM







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



### 文件上传下载

上传

```java
@RequestMapping("/upload")
public String httpUpload(@RequestParam("files")MultipartFile files[]){
    System.out.println(123);
    for(int i=0;i<files.length;i++){
        String fileName = files[i].getOriginalFilename();  // 文件名
        File dest = new File(filePath+fileName);
        System.out.println(dest);
        if (!dest.getParentFile().exists()) {
            dest.getParentFile().mkdirs();
        }
        try {
            files[i].transferTo(dest);
        } catch (Exception e) {
            return "failed";
        }
    }
    return "success";
}
```

下载

```java
@GetMapping("/download")
public String fileDownLoad(HttpServletResponse response, @RequestParam("fileName") String fileName){
    File file = new File(filePath+fileName);
    if(!file.exists()){
        return "下载文件不存在";
    }
    response.reset();
    response.setContentType("application/octet-stream");
    response.setCharacterEncoding("utf-8");
    response.setContentLength((int) file.length());
    response.setHeader("Content-Disposition", "attachment;filename="+fileName );

    try(BufferedInputStream bis = new BufferedInputStream(new FileInputStream(file));) {
        byte[] buff = new byte[1024];
        OutputStream os  = response.getOutputStream();
        int i = 0;
        while ((i = bis.read(buff)) != -1) {
            os.write(buff, 0, i);
            os.flush();
        }
    } catch (IOException e) {
        e.printStackTrace();
        return "下载失败";
    }
    return "下载成功";
}
```



### 自动装配





# Redis

为什么快？

>内存操作
>
>IO多路复用机制
>
>文件事件处理器单线程避免上下文切换

五种普通数据结构

> String(SDS)	List	Hash	Set	Sorted Set

问题

缓存穿透->布隆过滤器

缓存击穿->互斥锁

缓存雪崩->随机失效时间



# Nginx

多进程模型

> 一个Master进程，多个Worker进程

### 配置文件



### IO多路复用

select poll epoll



# MQ

异步、削峰、降低耦合性

AMQP协议

生产者消费者模型/发布订阅模型(Topic)

分布式事务



# Zookeeper

服务于分布式的文件系统

数据结构





# 数据结构

