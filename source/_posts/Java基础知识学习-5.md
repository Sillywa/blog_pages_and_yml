---
title: Java 继承
date: 2019-03-15 12:39:53
top: 10
tags: 
- Java基础
categories:
- Java
---

## 继承

在 Java 中，类的继承只能是单一继承，也就是说，一个子类只能拥有一个父类。

Java 中用 extends 关键字来实现继承。

<!-- more -->

```java
// 父类
public class Father {
    private String name;
    private int age;
    // 构造函数
    public Father(String name,int age) {
      this.name = name;
      this.age = age;
    }  
    public String getName() {
      return name;
    }
    public void setName(String name) {
      this.name = name;
    }
    public int getAge() {
      return age;
    }
    public void setAge(int age) {
      this.age = age;
    }
}
```

```java
// 子类
public class Son extends Father {

}
```

继承的特点：

1. 子类拥有父类非 private 的属性和方法
2. 子类可以拥有自己的属性和方法
3. 子类可以用自己的方式实现父类的方法
4. Java 的继承是单继承，但是可以多重继承，单继承就是一个子类只能继承一个父类，多重继承就是，例如A类继承B类，B类继承C类，所以按照关系就是C类是B类的父类，B类是A类的父类

Java 中用 extends 关键字来实现继承，用 super 关键字来实现对父类成员的访问，用来引用当前对象的父类，用 this 关键字指向自己的引用。

```java
// 子类
public class Son extends Father {
    private String fatherName = "baba";
    public Son(String name,int age,String fatherName) {
      super(name,age);
      this.fatherName = fatherName;
    }
    public String getFatherName() {
      return fatherName;
    }


    public void setFatherName(String fatherName) {
      this.fatherName = fatherName;
    }


    public static void main(String[] args) {
      // TODO Auto-generated method stub
      Son s = new Son("sillywa",23,"myfatyher");
      System.out.println(s.getName());
      System.out.println(s.getAge());
      System.out.println(s.getFatherName());
    }

}
```

**子类是不能继承父类的构造方法的，它只是隐式调用。如果父类的构造方法带有参数，则必须在子类的构造器中显式通过 super 关键字调用父类的构造方法并配有适当的参数。且必须在子类构造方法的第一行***

如果父类构造方法没有参数，则在子类的构造方法中不需要使用 super 关键字调用父类构造方法，系统会自动调用父类的无参构造方法。

Java 中所有的类都继承 Object 类，如果一个类没有使用 extends 关键字明确标识继承另一个类，那么这个类默认继承 Object 类。

Object 类的 toString() 方法返回对象的哈希 code 码（对象地址字符串）。

Object 类的 equals() 方法比较对象的引用是否指向同一块地址。

## 覆盖方法

当父类中的有些方法对子类并不适用时，子类可以重写父类的方法。

```java
public class Father {
    private String name;
    private int age;
    public Father(String name,int age) {
      this.name = name;
      this.age = age;
    }
    public String showDescription() {
      return "我是父亲：" + name;
    }
    public String getName() {
      return name;
    }
    public void setName(String name) {
      this.name = name;
    }
    public int getAge() {
      return age;
    }
    public void setAge(int age) {
      this.age = age;
    }
}

````

```java
public class Son extends Father {
  private String fatherName = "baba";
    public Son(String name,int age,String fatherName) {
      super(name,age);
      this.fatherName = fatherName;
    }

    @Override
    public String showDescription() {
      // TODO Auto-generated method stub
      return "我是儿子：" + this.getName();
    }



    public String getFatherName() {
      return fatherName;
    }


    public void setFatherName(String fatherName) {
      this.fatherName = fatherName;
    }


    public static void main(String[] args) {
      // TODO Auto-generated method stub
      Son s = new Son("sillywa",23,"myfatyher");
      System.out.println(s.getName());
      System.out.println(s.getAge());
      System.out.println(s.getFatherName());
      System.out.println(s.showDescription());
    }

}


```

在覆盖一个方法的时候，子类方法不能低于超类方法的可见性。特别地，如果超类方法是 public，子类方法一定要声明为 public。

## 阻止继承：final类和方法

有时候可能希望阻止人们利用某个类定义子类。不允许扩展的类被称为 final 类。如果定义类时使用了 final 修饰符就表明这个类是 final 类。例如希望阻止人们定义 Son 类的子类，就可以在定义这个类时使用 final 修饰符。

```java
public final class Son extends Father {
  ...
}
```

**类中的方法也可以被声明为 final。如果这样做，子类就不能重写这个方法（final 类中的所有方法自动地成为 final 方法，不包括成员变量）。**
