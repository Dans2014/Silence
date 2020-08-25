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

### 动态代理



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

> JAVA中有两种加锁的方式：`synchronized`关键字和`Lock`接口的实现类
>
> JAVA中只要出现锁对象，从应用层面上看就是悲观锁（虽然底层可能使用CAS）
>
> 乐观锁例子：`java.util.concurrent.atomic`包中原子类

```java
public final int getAndAddInt(Object var1, long var2, int var4) {
    int var5;
    do {
        var5 = this.getIntVolatile(var1, var2);
    } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));
    return var5;
}
```

##### 悲观锁（阻塞事务）

> 拿数据时认为别人**会**修改，每次拿数据时都会上锁。
>
> 别人想拿数据时会被阻塞，直到悲观锁释放。

##### 乐观锁（回滚重试）

> 拿数据时认为别人**不会**修改，所以不会上锁。
>
> 更新数据前检查读取至更新时间段内有没有修改。如果修改过则重新读取，重新尝试更新，循环直至成功。

##### CAS

> compare-and-swap 比较并替换
>
> 读取A，更新为B前，检查原值是否任为A，如果是则更新为B，否则什么都不做
>
> 两步是原子性的，用CPU指令（硬件层面保证原子性），是一步操作
>
> CAS做为乐观锁的基础，整个过程没有加锁和解锁操作，仅仅是一个循环重试的CAS算法而已，因此乐观锁策略也被称为无锁编程。

##### 自旋锁

> 加锁时`while(true)`循环

##### synchronized锁升级

> 偏向锁->轻量级锁->重量级锁，也叫**锁膨胀**
>
> 初次执行到synchronized代码块时，锁对象成为**偏向锁**（偏向于第一个获得它的线程），执行完同步代码块后，不会主动释放偏向锁
>
> 第二个线程加入**锁竞争**，偏向锁就升级为**轻量级锁（自旋锁）**。
>
> 轻量级锁状态下继续锁竞争，没抢到锁的线程**自旋**，即**忙等待（busy-waiting）**，短时间忙等换取线程用户态、内核态切换开销是一种折衷想法。
>
> 忙等是有限度的，达到最大自旋次数（虚拟机参数配置）的线程，会将轻量级锁升级为**重量级锁**。后续线程尝试获取锁时发现被占用的锁是重量级锁，直接将自己挂起，等待被唤醒。

##### 可重入锁

> 递归锁，允许同一个线程多次获取同一把锁。例如递归函数中加锁不会阻塞自己。
>
> JDK提供现有Lock实现类，包括synchronized关键字锁都是可重入的。

##### 公平锁、非公平锁

> 公平锁释放时，先申请线程先获得锁
>
> 非公平锁释放时，随机或按照其他优先级排序获得锁
>
> synchronized是一种非公平锁，一般情况下，非公平锁的吞吐量比公平锁大，没有特殊要求优先使用非公平锁

##### 可中断锁

> 可以相应中断的锁，B等待A的可中断锁，B向A发中断请求，A自行中断。
>
> synchronized不是可中断锁，Lock接口定义了`lockInterruptibly()`方法，故实现类都是可中断锁

##### 读写锁

> 读写锁是一对锁，读锁是**共享锁**，写锁是**互斥锁**
>
> 都是悲观锁策略



### 网络

```java
//TCPClient
Socket s = new Socket("127.0.0.1", 9001);
OutputStream out = s.getOutputStream();
out.write("from client".getBytes());

InputStream in = s.getInputStream();
byte[] data = new byte[1024];
int len = in.read(data);
System.out.println(new String(data, 0, len));

s.close();
```

```java
ServerSocket ss = new ServerSocket(9001);
Socket s = ss.accept();
InputStream in = s.getInputStream();
byte[] data = new byte[1024];
int len = in.read(data);
System.out.println(new String(data, 0, len));

OutputStream out = s.getOutputStream();
out.write("accepted by serve".getBytes());

s.close();
ss.close();
```



# I/O

### select

![](D:\Silence\assets\select_model.png)

> 每次调用select，都需要将fd（事件/连接）集合从用户态拷贝至内核态，fd很多时开销很大
>
> 同时每次调用select都需要在内核遍历传进来的所有fd
>
> select支持的文件描述符数量只有1024

### poll

> 原理和select一样，由于底层数据结构是链表，poll可以支持大于1024个文件描述

### epoll

> event poll，线程安全
>
> 内部使用mmap共享用户态和内核态部分空间（简易文件系统/B+树），避免数据来回拷贝
>
> 基于事件驱动，epoll_create建立一个epoll对象，调用epoll_ctl向epoll对象中注册事件及回调函数，epoll_wait收集发生事件并回调

### I/O多路复用

> 可以监听多个描述符的读/写等事件，一旦某个描述符就绪（一般是读或者写事件发生了），就能够将发生的 事件通知给关心的应用程序去处理该事件。

**网卡接收数据：**通过硬件传输，网卡接收的数据写入内存中，操作系统就可以去读。

**CPU中断：**数据写入内存后网卡向CPU发中断信号，操作系统便知道有新数据到来，通过网卡中断程序去处理数据

**进程调度：**进程阻塞不占用cpu资源



# Netty

### BIO

Blocking I/O

经典线程池模型

```java

{
    ExecutorService executor = Executors.newFixedThreadPool(100);
    ServerSocket ss = new ServerSocket(9001);
    while(!Thread.currentThread().isInterrupted()){
        Socket socket = ss.accept();
        executor.submit(new ConnectIOHandler(socket));
    }
}

class ConnectIOHandler extends Thread{
    private Socket socket;
    public ConnectIOHandler(Socket socket){
        this.socket = socket;
    }
    public void run(){
        while(!Thread.currentThread().isInterrupted()&&!socket.isClosed())
            //处理逻辑
    }
}
```



### NIO

系统I/O操作分为等待就绪和操作

BIO一直阻塞（我要读），NIO轮询非阻塞（我可以读了），AIO数据从网卡到网卡也是非阻塞的（读好了）

传统HTTP服务器原理：

> 1、创建ServerSocket，监听一个端口
>
> 2、客户端请求服务器
>
> 3、服务器Accept，获得来自客户端Socket连接对象
>
> 4、启动一个新线程处理连接：
>
> > > 读Socket字节流	解码协议得到Http请求对象	处理Http结果封装成HttpResponse对象	编码协议序列化字节流写Socket返回给客户端



# JVM

内存模型：堆、方法区、栈

堆模型：老年代、青年代（Eden，From Survivor，To Survivor 8:1:1）

### GC

判断对象存活：引用计数、可达性分析

GC算法：标记-清除法，复制算法，标记压缩算法，分代收集算法

GC收集器：

MinorGC FullGC



# JAVA WEB

### Tomcat

> 免费开源轻量级应用服务器

J2EE 13种规范只是实现了Servlet/JSP（WebLogic/JBOSS完全支持），故又称Servlet容器

### Servlet



### Tips

TCP/IP

> 传输层的网络协议族（三次握手四次挥手）

SOCKET

> 是操作系统实现的TCP/IP协议族的接口，实现socket/bind/listen/accept/connect等功能

RPC	(Remote Procedure Call) 即远程过程调用

> 应用间通信最基础的方法是基于TCP/IP通过Socket编程实现，因其难度较大衍生出的解决应用间通信的框架

HTTP

>基于TCP/IP之上有特殊规范要求的应用层协议

Webservice

> 原指所有基于web的服务器

SOAP(Simple Object Access Protocol) 简单对象访问协议

> 基于XML的轻量协议，约定使用WSDL (Web Service Description Language)服务描述语言，传输协议依可以是HTTP/SMTP

gRPC

> 使用Protocol Buffers（压缩率高的序列化协议），传输使用基于HTTP2.0的Netty

Rest

> 一种面向资源、无状态的架构风格，基于HTTP的文本类（JSON）传输方式

web服务器

> Nginx/Apache/IIS

应用服务器

> Tomcat/WebLogic/JBOSS



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



### 约定大于配置

父依赖`spring-boot-starter-parent`的父依赖`spring-boot-dependencies`放了很多依赖及版本号，故项目pom中引入一些依赖无需指定版本号

`spring-boot-starter-parent`中还指定了配置文件等资源库，故项目启动会读取`${basedir}/src/main/resources`路径下`application.yml`等配置文件

```xml
<resource>
  <directory>${basedir}/src/main/resources</directory>
  <excludes>
    <exclude>**/application*.yml</exclude>
    <exclude>**/application*.yaml</exclude>
    <exclude>**/application*.properties</exclude>
  </excludes>
</resource>
```

### 启动器



### 自动装配

主函数注解`@SpringBootApplication`





# SpringCloud

### CAP

> C（Consistency）：数据一致性
>
> A（Availability）：可用性
>
> P（Partition Tolerance）：分区容错性

### Eureka

> AP原则

### Zookeeper

> CP原则

### ElasticSearch

> 倒排索引，索引（数据库），类型（表），字段，文档（行）



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

> 生产者、消费者、交换机、队列

死信队列实现延迟队列

异步、削峰、降低耦合性

AMQP协议

生产者消费者模型/发布订阅模型(Topic)

分布式事务



# Zookeeper

服务于分布式的文件系统

数据结构





# 数据结构



# 计算机网络

五层网络模型

| 应用层     |
| ---------- |
| 传输层     |
| 网络层     |
| 数据链路层 |
| 物理层     |

