---
title: Lambda表达式(三)
date: '2019-06-18 19:23'
sticky: 0
keywords: java基础 lambda
categories:
  - java基础
  - lambda
tags:
  - lambda
  - java基础
abbrlink: 54405
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gxnz1dd4baj31ib0u03zd.jpg'
top_img:
description:
---

## 1、如何使用 lambda表达式

### 1.1  构造器引用

```java
public class ConstructMethodExample {

    public static void main(String [] args) {
        // 构造方法方法引用--无参数(可以使用方法引用)
        Supplier<Method> supplier = Method::new;
        System.out.println(supplier.get());
        // 等价于
        Supplier<Method> supplier2 = () -> new Method();
        System.out.println(supplier2.get());

        // 构造方法方法引用--有参数
        Function<String, Method> uf = name -> new Method(name);
        Method method = uf.apply("一个优秀的废人");
        System.out.println(method.toString());
    }
}
```

### 2、变量作用域

 lambda 表达式只能引用标记了 final 的外层局部变量，这就是说不能在 lambda 内部修改定义在域外的局部变量，否则会编译错误。 

```java
public class VariableScopeTest {

    // 定义一个接口
    public interface Converter<T1, T2> {
        void convert(int i);
    }
    public static void main(String [] args) {

        // 定义为 final 强制不能修改
        final int num = 1;
        Converter<Integer, String> s = (param) -> System.out.println(String.valueOf(param + num));
        // 输出结果为 3
        s.convert(2);  
    }
}
```

