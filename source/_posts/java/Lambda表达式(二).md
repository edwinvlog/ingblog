---
title: Lambda表达式(二)
date: '2019-06-18 18:42'
sticky: 0
keywords: java基础 lambda
categories:
  - java基础
  - lambda
tags:
  - lambda
  - java基础
abbrlink: 17447
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gxnz1dd4baj31ib0u03zd.jpg'
top_img:
description:
---

## 1、如何使用 lambda表达式

### 1.1  lambda语法

```java
// 格式遵循: (接口参数)->表达式(具体实现的方法)
(paramters) -> expression 或 (parameters) ->{ expressions; }
```

![](https://tva1.sinaimg.cn/large/008i3skNly1gxnzlzb3paj313g0gy771.jpg)

具体解释，如上图。此外，lambda 语法注意点：

- 可选类型声明：方法参数不需要声明参数类型，编译器可以统一识别参数值。
- 可选的参数圆括号：一个参数无需定义圆括号，但无参数或多个参数需要定义圆括号。
- 可选的大括号：如果具体实现方法只有一个语句，就不需要使用中括号{}。
- 可选的返回关键字：如果具体实现方法只有一个表达式，则编译器会自动返回值，如果有多个表达式则，中括号需要指定明表达式返回了一个数值。

具体使用示例:

```java
public class Example {

    // 定义函数式接口，只能有一个抽象接口，否则会报错
    // 希望在编译期检出报错，请加 @FunctionalInterface 注解
    public interface Hello {
        String hi();
    }

    public interface Hello2 {
        String hei(String hello);
    }

    public interface Hello3 {
        String greet(String hello, String name);
    }

    public static void main(String[] args) {

        // 入参为空
        Hello no_param = () -> "hi, no param";
        Hello no_param2 = () -> {
            return "hi, no param";
        };
      
        System.out.println(no_param.hi());
        System.out.println(no_param2.hi());

        // 单个参数,一条返回语句,可以省略大括号和 return
        Hello2 param = name -> name;
        Hello2 param2 = name -> {
            return name;
        };

        // 打印
        System.out.println(param.hei("hei, 一个优秀的废人"));
        System.out.println(param2.hei("hei, 一个优秀的废人"));

        // 多个参数
        Hello3 multiple = (String hello, String name) -> hello + " " + name;

        // 一条返回语句，可以省略大括号和 return
        Hello3 multiple2 = (hello, name) -> hello + name;

        // 多条处理语句，需要大括号和 return
        Hello3 multiple3 = (hello, name) -> {
            System.out.println(" 进入内部 ");
            return hello + name;
        };

        // 打印
        System.out.println(multiple.greet("hello,", "祝2019暴富"));
        System.out.println(multiple2.greet("hello,", "祝2019暴富"));
        System.out.println(multiple3.greet("hello,", "祝2019暴富"));
    }
}
```

## 2、 方法引用

```java
Consumer<String> sc = System.out::println;
sc.accept("一个优秀的废人");

// 等价于
Consumer<String> sc2 = (x) -> System.out.println(x);
sc2.accept("一个优秀的废人");
```

Consumer 函数式接口源码：

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

分析一波：Consumer 是一个函数式接口，抽象方法是 void accept(T t)，参数都是 T。那我们现在有这样一个需求，我想利用这个接口的抽象方法，做一下控制台打印。正常情况下，我们需要实现这个接口，实现它的抽象方法，来实现这个需求：

```java
public class ConsumerImpl implements Consumer<String> {
    @Override
    public void accept(String s) {
        System.out.println(s);
    }
}
```

**实现之后，这个抽象方法变具体了。作用就是控制台打印，那就意味着抽象方法刚好可以用实际方法： System.out.println(s) 来实现，所以我们可以使用方法引用。**

总结：**函数式接口的抽象方法实现恰好可以通过调用一个实际方法来实现时，就可以用方法引用。**

方法引用的三种形式：

```java
// 将抽象方法参数当做实际方法的参数使用
对象：：实例方法 objectName::instanceMethod
// 将抽象方法参数当做实际方法的参数使用
类：：静态方法 ClassName::staticMethod
// 将方法参数的第一个参数当做方法的调用者，其他的参数作为方法的参数
类：：实例方法  ClassName::instanceMethod 
```

自定义一个方法类：

```java
public class Method {
    // 静态方法
    public static void StaticMethod(String name) {
        System.out.println(name);
    }

    // 实例方法
    public void InstanceMethod(String name) {
        System.out.println(name);
    }

    // 无参构造方法
    public Method() {
    }

    // 有参数构造
    public Method(String methodName) {
        System.out.println(methodName);
    }
}
```

测试用例：

```java
public class MethodExample {

    public static void main(String[] args) {

        // 静态方法引用--通过类名调用
        Consumer<String> consumerStatic = Method::StaticMethod;
        consumerStatic.accept("静态方法");
        // 等价于
        Consumer<String> consumerStatic2 = (x) -> Method.StaticMethod(x);
        consumerStatic2.accept("静态方法");

        System.out.println("--------------------------");
        //非静态方法引用--通过实例调用
        Method method = new Method();
        Consumer<String> consumerInstance = method::InstanceMethod;
        consumerInstance.accept("对象的实例方法");
        // 等价于
        Consumer<String> consumerInstance2 = (x) -> method.InstanceMethod(x);
        consumerInstance2.accept("对象的实例方法");

        System.out.println("--------------------------");
        //ClassName::instanceMethod  类的实例方法：把表达式的第一个参数当成 instanceMethod 的调用者，其他参数作为该方法的参数
        BiPredicate<String, String> sbp = String::equals;
        System.out.println("类的实例方法 " + sbp.test("a", "A"));
        // 等效
        BiPredicate<String, String> sbp2 = (x, y) -> x.equals(y);
        System.out.println("类的实例方法 " + sbp2.test("a", "A"));
    }
}
```

输出结果：

```java
静态方法
静态方法
--------------------------
对象的实例方法
对象的实例方法
--------------------------
类的实例方法false
类的实例方法false
```















