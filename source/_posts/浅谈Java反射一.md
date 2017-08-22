---
title: 浅谈Java反射一
date: 2016-10-12 15:06:47
tags: [Java, 笔试题]

---

这学期刚开学的时候在睿思(我们学校的BBS)上看到了一个学长的求助，就收藏了，那时候自己还在搭建Hexo博客，没时间研究，昨天就去翻看了一下，原题如下：

```java
public class A {
    protected String getString() {
        return "A";
    }
}

public class B extends A {
    protected String getString() {
        return "B";
    }
}

public class C extends B {
}
```

要求在子类C的对象中访问其父类的父类A中的getString方法。

<!-- more -->

---

这题不能想当然地在C类里面加一个A类的成员，要访问一个编译时根本无法预知类型的对象，那必须使用反射。那今天我们就来讲一下用反射查看类的信息。

我们先来讲一下Java中Class这个类，好比类是一批拥有共同特征的对象的抽象，Class是这些类的抽象，也就是说Class是所有类的类。Class 类的实例表示正在运行的 Java 应用程序中的类和接口。Class 没有公共构造方法。Class 对象是在加载类时由 Java 虚拟机以及通过调用类加载器中的 defineClass 方法自动构造的。例如car.getClass().getName();　还可以使用一个类字面值来获取指定类型（或 void）的 Class 对象。例如，Car.class.getName(); 有了获取正在运行的 Java 应用程序中的类的方法，接下来就好办了。

```java
public class C extends B {
    public static void main (String args[]){
        Class a = C.class.getSuperclass().getSuperclass();
        try {
            Object o = a.newInstance();
            A c = (A)o;
            System.out.println(c.getString());
        } catch(Exception e) {
            e.printStackTrace();
        }
    } 
}
```

或者下面这个更好的

```java
public class C extends B {
    public static void main (String args[]){
        Class a = C.class.getSuperclass().getSuperclass();
        try {
            Method m = a.getDeclaredMethod("getString",null);
            System.out.println(m.invoke(a.newInstance(),null));
        } catch (Exception e) {
            e.printStackTrace();
        }
    } 
}
```

下一篇我来讲一下怎么用反射调用私有属性和方法(setAccessible)。

晚安。

