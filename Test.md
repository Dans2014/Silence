### 网络

5'	 单次成功率50%的网络信道上一次HTTP请求成功率是多少？

10'	谈谈计算机网络五层模型、TCP/IP三次握手四次挥手。

### Linux

5'	文件系统节点用什么数据结构存储？

5'	kill命令是做什么的？

### 操作系统

5'	描述进程与线程区别，各自有哪几种状态，进程间通信的方式，进程调度的策略。

### java

```java
String a = "hello world";
String b = "hello world";

System.out.println(a.equals(b));
System.out.println(a==b);
System.out.println(new String("hello world")==new String("hello world"));
System.out.println(new String("hello world").equals(new String("hello world")));

public static boolean judge(String s){
    Stack<Character> stack = new Stack<>();
    for(int i=0;i<s.length();i++){
        if(s.charAt(i)=='(' || s.charAt(i)=='[' || s.charAt(i)=='{')
            stack.push(s.charAt(i));
        else
            if(stack.empty())
                return false;
        if((stack.peek()=='(' && s.charAt(i)==')') || (stack.peek()=='[' && s.charAt(i)==']') || (stack.peek()=='{' && s.charAt(i)=='}'))
            stack.pop();
    }
    if(!stack.empty())
        return false;
    return true;
}
```

5'	上述分别输出什么

5'	throw/throws有什么区别

5'	final/finally/finalize的区别

5'	servlet的生命周期

5'	简述同步异步、阻塞非阻塞

5'	你所知道的代码规范有哪些

5'	常见的设计模式，任选一种给出demo

### jvm

5’	查看JVM进程状况、统计信息、内存对象常用命令

10'	描述JVM内存模型，堆区内存划分，简述垃圾回收及常用算法。

### spring framework

10'	谈谈对spring的理解。介绍IOC、AOP及实现原理。

### 微服务

10'	什么是微服务架构？有什么优缺点？常见的解决方案、功能模块有哪些？