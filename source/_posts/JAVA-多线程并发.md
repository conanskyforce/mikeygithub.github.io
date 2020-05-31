---
title: JAVA 多线程并发
date: 2020-05-30 12:29:53
index_img: /resource/img/more_thread.jpeg
category: Java
tags: Thread
---

# Java 并发知识库

![avatar](/resource/img/thread.png)

# Java线程实现/创建方式

## 继承Thread
>Thread类本质上是实现了`Runnable`接口的一个实例，代表一个线程的实例。启动线程的唯一方法就是通过`Thread`类的`start`实例方法。start方法是一个native方法，它将启动一个新的线程，并执行run()方法。

```java
public class MyThread extends Thread {
    public void run() {
        System.out.println("MyThread.run()");
    }
}

MyThread myThread = new MyThread();
myThread.start();
```

## 实现Runnable接口
>实现Runnable接口

如果自己的类已经extends另一个类，就无法直接extends Thread此时，可以实现一个Runnable接口。
```java
public class MyThread extends OtherClass implements Runnable {
    public void run () {
        System.out.println("MyThread.run()");
    }
}

//启动 MyThread,需要首先实例化一个 Thread,并传入自己的 MyThread 实例:

MyThread myThread = new MyThread();
Thread thread = new Thread(myThread);
thread.start();

//事实上,当传入一个 Runnable target 参数给 Thread 后,Thread 的 run()方法就会调用
target.run()
public void run() {
    if (target != null) {
        target.run();
    }
}
```
## 执行器方式
>有返回值的任务必须实现 Callable 接口,类似的,无返回值的任务必须 Runnable 接口。执行Callable 任务后,可以获取一个 Future 的对象,在该对象上调用 get 就可以获取到 Callable 任务返回的 Object 了,再结合线程池接口 ExecutorService 就可以实现传说中有返回结果的多线程了。


```java

```
## 线程池方式
