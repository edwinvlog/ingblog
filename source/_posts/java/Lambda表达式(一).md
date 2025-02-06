---
title: Lambda表达式(一)
date: '2019-06-18 18:21'
sticky: 0
keywords: java基础 lambda
categories:
  - java基础
  - lambda
tags:
  - lambda
  - java基础
abbrlink: 33923
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gxnz1dd4baj31ib0u03zd.jpg'
top_img:
description:
---

## 1、什么是lambda表达式

Java8 是我们使用最广泛的稳定 Java 版本，lambda 就是其中最引人瞩目的新特性。lambda 是一种闭包，它允许把函数当做参数来使用，是面向函数式编程的思想，可以使代码看起来更加简洁。是不是听得一脸懵逼？我举个栗子你就明白了。

烂掉牙的例子，在没有 lambda 时候，我们是这样写的：

```java
// 内部类写法
public class InnerClassMain {
    public static void main(String[] args) {
        //匿名内部类写法
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("内部类写法");
            }
        }).start();
    }
}
```

有 lambda 之后，我们就用 lambda 写：

```java
// lambda 写法
public class LambdaMain {
    
    public static void main(String[] args) {
        //lambda 写法
        new Thread(() -> System.out.println("Runnable-->lambda写法")).start();
    }
    
}
```

我们应该知道，实现线程有两种方法，**一是继承 Thread 类，二是实现 Runnable 接口。那这里采用的就是后者，后者是一个函数式接口。**

## 2、函数式接口

```java
@FunctionalInterface
public interface Runnable {
    /**
     * When an object implementing interface <code>Runnable</code> is used
     * to create a thread, starting the thread causes the object's
     * <code>run</code> method to be called in that separately executing
     * thread.
     * <p>
     * The general contract of the method <code>run</code> is that it may
     * take any action whatsoever.
     *
     * @see     java.lang.Thread#run()
     */
    public abstract void run();
}
```

从 Runnable 源码可以看到，它是一个**函数式接口。**这类接口的特点是：用 @FunctionalInterface 注解修饰（主要用于编译级错误检查，加上该注解，当你写的接口不符合函数式接口定义的时候，编译器会报错），有且只有一个抽象方法。在原生 JDk 中的这类接口就可以使用 lambda 表达式。

上面的概念提到，把函数当做参数来使用。上面的 lambda 例子中，Thread 类的参数就是一个 Runnable 接口，lambda 就是实现这个接口并把它当做参数使用。所以上面的 () -> System.out.println("lambda写法") 就是一个整个 lambda 表达式的参数（注意与后面的方法参数区分开，后面会讲）。细品加粗这句话，可以总结出，**lambda 表达式就是创建某个类的函数式接口的实例对象。**如：

```java
Runnable runnable = () -> System.out.println("lambda写法");
```

## 3、为什么需要lambda表达式

明白了什么是 lambda 表达式，那为什么要使用它呢？注意到使用 lambda 创建线程的时候，我们并不关心**接口名，方法名，参数名。我们只关注他的参数类型，参数个数，返回值。所以原因就是简化代码，提高可读性**。













































