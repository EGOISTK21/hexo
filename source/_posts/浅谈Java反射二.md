---
title: 浅谈Java反射二
date: 2016-10-13 15:40:58
tags: [Java, 笔试题]
---

昨天说的要更深入地探究一下Java反射，看了[嘟嘟MD ](http://www.jianshu.com/users/a7f72d78fe0d)前辈的这篇文章[Java基础与提高干货系列——Java反射机制](http://www.jianshu.com/p/1a60d55a94cd)顿时神清气爽，觉得没多大必要再重复写他写过的了。所以今天就改成详细讲解反射的一众方法好了。

---

先补充一下昨天所讲的创建 Class 对象的方法，其实一共有三种（昨天漏说了第三种，代码引用自[嘟嘟MD](http://www.jianshu.com/users/a7f72d78fe0d)）：

```java
//这说明任何一个类都有一个隐含的静态成员变量class，这种方式是通过获取类的静态成员变量class得到的
Class c1 = Code.class;
//code1是Code的一个对象，这种方式是通过一个类的对象的getClass()方法获得的
Class c2 = code1.getClass();
//这种方法是Class类调用forName方法，通过一个类的全量限定名获得
Class c3 = Class.forName("com.trigl.reflect.Code");
```
<!-- more -->

下面列出 Class 类的常用方法集：

| 返回值              | 方法名及参数                                   |
| ---------------- | ---------------------------------------- |
| Field            | getDeclaredField(String name)            |
|                  | 返回一个 Field 对象，该对象反映此Class对象所表示的类或接口的指定已声明字段。 |
| Field[]          | getDeclaredFields()                      |
|                  | 返回 Field 对象的一个数组，这些对象反映此 Class 对象所表示的类或接口所声明的所有字段。 |
| Method           | getDeclaredMethod(String name, Class<?>... parameterTypes) |
|                  | 返回一个 Method 对象，该对象反映此 Class 对象所表示的类或接口的指定**已声明**方法。 |
| Method[]         | getDeclaredMethods()                     |
|                  | 返回 Method 对象的一个数组，这些对象反映此 Class 对象表示的类或接口声明的所有方法，包括公共、保护、默认（包）访问和私有方法，但不包括继承的方法。 |
| Method[]         | getMethods()                             |
|                  | 返回一个包含某些 Method 对象的数组，这些对象反映此 Class 对象所表示的类或接口（包括那些由该类或接口声明的以及从超类和超接口继承的那些的类或接口）的公共 member 方法。 |
| Field            | getField(String name)                    |
|                  | 返回一个 Field 对象，它反映此 Class 对象所表示的类或接口的指定公共成员字段。 |
| Field[]          | getFields()                              |
|                  | 返回一个包含某些 Field 对象的数组，这些对象反映此 Class 对象所表示的类或接口的所有可访问公共字段。 |
| Method           | getMethod(String name, Class<?>... parameterTypes) |
|                  | 返回一个 Method 对象，它反映此 Class 对象所表示的类或接口的指定公共成员方法。 |
| String           | getName()                                |
|                  | 以String的形式返回此 Class 对象所表示的实体（类、接口、数组类、基本类型或 void）名称。 |
| String           | getSimpleName()                          |
|                  | 返回源代码中给出的底层类的简称。                         |
| Class<? super T> | getSuperclass()                          |
|                  | 返回表示此Class所表示的实体（类、接口、基本类型或 void）的超类的 Class。 |
| T                | newInstance()                            |
|                  | 创建此Class对象所表示的类的一个新实例。                   |
Field 类的常用方法：

| 返回值    | 方法名及参数                |
| ------ | :-------------------- |
| String | getName()             |
|        | 返回此 Field 对象表示的字段的名称。 |

Method 类的常用方法：

| 返回值    | 方法及参数                              |
| ------ | :--------------------------------- |
| String | getName()                          |
|        | 以 String 形式返回此 Method 对象表示的方法名称。   |
| Object | invoke(Object obj, Object... args) |
|        | 对带有指定参数的指定对象调用由此 Method 对象表示的底层方法。 |

下面我们通过一些例子来认识一下以上的方法：

```java
public class Person {
    private String name;
    private int age;

    public Person() {
        this.name = "nameless";
        this.age = 20;
    }

    public String getName() {
        return name;
    }
  
    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return this.getClass().getSimpleName() + " " + getName() + " is " + getAge() + " years old.";
    } 
}

public class Programmer extends Person {}

public class JavaProgrammer extends Programmer {
    public static void main(String[] args) {
        Person j = new JavaProgrammer();//试一下多态
        System.out.println(j);//输出JavaProgrammer nameless is 20 years old.
        System.out.println(JavaProgrammer.class.getSimpleName());//输出JavaProgrammer
        System.out.println(JavaProgrammer.class.getSuperclass().getSuperclass().getName());//输出com.egoistk.Person
        try {
            Method m = Person.class.getDeclaredMethod("toString");
            //Method m = JavaProgrammer.class.getDeclaredMethod("toString");错误，toString方法在 JavaProgrammer 类中没有被声明
            Method mm = JavaProgrammer.class.getMethod("toString");//正确，toString方法是 JavaProgrammer 类中的公共成员方法
            System.out.println(m.invoke(JavaProgrammer.class.newInstance()));//输出JavaProgrammer nameless is 20 years old. 在反射中的体现方法的动态绑定完美地解释了多态
            System.out.println(JavaProgrammer.class.getSuperclass().getSuperclass().newInstance());//输出Person nameless is 20 years old.
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
如果一个类中的方法是私有的我们又怎么访问，且看如何用反射调用私有属性和方法(setAccessible)：

```java
public class Person {
    private String name;
    private int age;

    public Person() {
        this.name = "nameless";
        this.age = 20;
    }

    public String getName() {
        return name;
    }

    private void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    private void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return this.getClass().getSimpleName() + " " + getName() + " is " + getAge() + " years old.";
    }
}

public class Main{
    public static void main(String[] args) {
        Method[] methods = Person.class.getDeclaredMethods();
        for(Method m:methods){
            System.out.println(m.getName()); 
        }//输出main
        //toString
        //getName
        //setName
        //getAge
        //setAge
        try {
            Method m = Person.class.getDeclaredMethod("getName");
            System.out.println(m.invoke(new Person()));//输出nameless
            Method mm = Person.class.getDeclaredMethod("setName", String.class);
            Person p = new Person();
            //mm.invoke(p, "EGOISTK");错误，setName方法是private方法，无法调用
            mm.setAccessible(true);//打开setName的权限
            mm.invoke(p, "EGOISTK");//可以调用了
            System.out.println(m.invoke(p));//输出EGOISTK
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

**与你共勉**

