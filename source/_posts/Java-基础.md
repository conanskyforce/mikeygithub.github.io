---
title: Java 基础
date: 2020-07-07 09:46:31
index_img: /resource/img/javaer-job-press.jpeg
category: Java
tags: 基础
---

# JAVA 异常分类及处理
概念  
>如果某个方法不能按照正常的途径完成任务,就可以通过另一种路径退出方法。在这种情况下
 会抛出一个封装了错误信息的对象。此时,这个方法会立刻退出同时不返回任何值。另外,调用
 这个方法的其他代码也无法继续执行,异常处理机制会将代码执行交给异常处理器。

![avatar](/resource/img/exception.png)

异常分类  
Error  
Exception(RuntimeException、CheckedException)  
异常的处理方式  
遇到问题不进行具体处理,而是继续抛给调用者 (throw,throws)  
try catch 捕获异常针对性处理方式  
Throw 和 throws 的区别:  
位置不同:
功能不同:  
# JAVA 反射  
51.2.1. 动态语言  
51.2.2. 反射机制概念 (运行状态中知道类所有的属性和方法)  
反射的应用场合  
编译时类型和运行时类型  
的编译时类型无法获取具体方法  
Java 反射 API  
反射 API 用来生成 JVM 中的类、接口或则对象的信息。  
反射使用步骤(获取 Class 对象、调用对象方法)  
获取 Class 对象的 3 种方法  
调用某个对象的 getClass()方法  
调用某个类的 class 属性来获取该类对应的 Class 对象  
使用 Class 类中的 forName()静态方法(最安全/性能最好)  
创建对象的两种方法  
Class 对象的 newInstance()  
调用 Constructor 对象的 newInstance()  
# JAVA 注解  
概念  
种标准元注解
@Target 修饰的对象范围  
@Retention 定义 被保留的时间长短  
@Documented 描述-javadoc  
@Inherited 阐述了某个被标注的类型是被继承的  
注解处理器  
# JAVA 内部类  
# JAVA 泛型  
静态内部类  
成员内部类  
局部内部类(定义在方法中的类)  
匿名内部类(要继承一个父类或者实现一个接口、直接使用 new 来生成一个对象的引用) 
泛型方法(<E>)  
泛型类<T>  
类型通配符?  
类型擦除  
# JAVA 序列化 ( 创建可复用的 Java 对象 )  
保存(持久化)对象及其状态到内存或者磁盘  
序列化对象以字节数组保持-静态成员不保存  
序列化用户远程对象传输  
Serializable 实现序列化  
ObjectOutputStream 和 ObjectInputStream 对对象进行序列化及反序列化  
writeObject 和 readObject 自定义序列化策略  
序列化 ID  
序列化并不保存静态变量  
序列化子父类说明  
Transient 关键字阻止该变量被序列化到文件中  
# JAVA 复制  114
直接赋值复制  
浅复制(复制引用但不复制引用的对象)  
深复制(复制对象和其应用对象)  
序列化(深 clone 一中实现) 