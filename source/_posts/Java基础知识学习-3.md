---
title: Java 类与对象
date: 2019-03-11 19:19:39
top: 10
tags:
- Java基础
categories:
- Java
---

## 面向对象的程序设计

面向对象的程序设计（简称OOP）是当今主流的程序设计范式，Java 是完全面向对象的语言。面向对象的程序是由对象组成，每个对象包含对用户公开的特定功能部分和隐藏的实现部分。在OOP中不必关心具体的实现，只要能够满足用户需求即可。

<!-- more -->

Alan Kay 曾经总结了第一个成功面向对象语言、同时也是 Java 所基于的语言之一的 Smalltalk 的五个基本特性，这些特性表现了一种纯粹的面向对象程序性设计的方式：

- **万物皆对象**。将对象视为奇特的变量，它可以存储数据，除此之外，你还可以要求它在自身上执行操作。理论上讲，可以抽取待求解问题的任何概念化构件，将其表示为程序中的对象。

- **程序是对象的集合，它们通过发送消息来告知彼此所要做的**。要想请求一个对象，就必须对该对象发送一条消息。更具体地来说。可以把消息想象为对某个特定对象的方法的调用请求。

- **每个对象都有自己的由其他对象所构成的存储**。换句话说，可以通过创建包含现有对象的包的方式来创建新类型的对象。因此，可以在程序中构建复杂的体系，同时将其复杂性隐藏在对象的简单性背后。

- **每个对象都拥有其类型**。即每个对象都是某个类的实例。

- **某一特定类型的所有对象可以接受同样的消息**。

面向对象的语言有三个重要的特征：封装、继承、多态。

## 类与对象

类(class)是一个模板，它描述一类对象的行为和状态。由类构造(construct)对象的过程称为创建类的实例(instance)。**对象具有状态、行为和标识**。这意味着每一个对象都可以拥有内部数据（它们给出了该对象的状态）和方法（它们产生的行为），并且每一个对象都可以唯一地与其它对象区分开来，具体来说就是每一个对象在内存中都有一个唯一的地址。

### 类之间的关系

- 依赖("uses-a")：一个类的方法操作另一个类的对象，应该尽可能地将相互依赖的类减至最少，也就是让类之间的耦合度最小。

- 聚合("has-a")：类A的对象包含类B的对象

- 继承("is-a")：类A扩展类B，类A包含类B的方法和属性

### 类的定义

在 Java 中使用 class 关键字来定义类，一个类的类名应该和文件名同名并且一般首字母大写。

```java
// Person.java
public class Person {
  int age = 10;
  String name = "";
  void sayAge() {
    System.out.println("age:" + age);
  }
}
```

一旦定义了一个类（在Java中你所做的全部工作就是定义类，产生那些类的对象，以及发送消息给这些对象），就可以在类中设置两种基本的元素：**字段**（有时也被称为数据成员）和**方法**（有时也被称为成员函数）。字段可以是任何类型的对象，可以通过其引用与其进行通信；也可以是基本类型的一种。如果字段是某个对象的引用，那么必须初始化该引用，以便使其与一个实际的对象（使用 new 来实现）相关联。

### 基本成员默认值（成员变量默认值）

若类的某个成员是基本数据类型，即使没有进行初始化，Java 也会确保它获得一个默认值，如下所示。但是这些初始化对于程序来说可能是不正确的，甚至是不合法的。所以最好明确地对变量进行初始化。

```java
// DefaultValue.java
public class DefaultValue {
  boolean b;
  char c;
  byte by;
  short s;
  int i;
  long l;
  float f;
  double d;

  void printDefaultValue() {
    System.out.println(b);    // false
    System.out.println(c);    // '\u0000'(null)
    System.out.println(by);   // 0
    System.out.println(s);    // 0
    System.out.println(i);    // 0
    System.out.println(l);    // 0
    System.out.println(f);    // 0.0
    System.out.println(d);    // 0.0
  }
}
```

需要注意的是只有成员变量才会赋默认值，局部变量并不会有默认值。

## 方法参数

在程序设计语言中将参数传递给方法有两种传递方式：**按值调用**表示方法接收的是调用者提供的值；**按引用调用**表示方法接受的是调用者提供的变量地址。一个方法可以修改传递引用所对应的变量值，而不能修改传递值调用所对应的变量值。

**Java语言总是采用按值传递。也就是说，方法得到的是所有参数值的一个拷贝，特别是，方法不能修改传递给它的任何参数变量的内容。**

假定一个方法试图将一个参数的值增加三倍：

```java
public static void tripleValue(double x) {
  x = 3*x;
}
```

然后调用这个方法：

```java
double percent = 10;
tripleValue(percent);
```

调用这个方法之后，percent 的值还是 10。下面看一下具体的执行过程：

1.x 被初始化为 percent 值的一个拷贝（也就是 10）；

2.x 被乘以 3 后等于 30。但是 percent 的值仍然是 10；

3.方法调用结束之后，参数变量 x 不再使用。

{% asset_img 1.png 执行过程 %}

然而方法参数共有两种：

- 基本类型

- 引用类型

我们已经知道一个方法不可能修改一个基本数据类型的参数。而对象引用作为参数就不同了，可以很容易地利用方法将一个人的年龄提高三倍：

```java
public static void tripleAge(Person x) {
  x.age = 3 * x.age;
}
```

当调用：

```java
Person p = new Person("sillywa",20);
tripleAge(p);
```

1.x 被初始化为 p 值的拷贝，这时 x 和 p 指向同一个对象；

2.当改变 x 的 age 时，即改变的是 x 和 p 共同指向的那个对象的 age；

3.方法结束之后，x 不再使用，但是 p 依然指向那个对象。

{% asset_img 2.png 执行过程 %}

我们已经看到，实现改变对象参数状态的方法并不是一件难事。理由很简单，方法得到的是对象引用的拷贝，对象引用及其它的拷贝同时引用同一个对象。

有些人可能会认为 Java 程序设计语言对对象采用的是引用调用，实际上，这种理解是不正确的。看一下例子：

首先编写一个交换两个Person对象的方法：

```java
public static void swap(Person x, Person y) {
  Person temp = x;
  x = y;
  y = temp;
}
```

如果 Java 对对象采用的是按引用调用，那么这个方法就应该能实现交换数据的效果：

```java
Person p1 = new Person("sillywa",20);
Person p2 = new Person("sw",20);
swap(p1,p2);
```

但是方法并没有改变存储在变量 p1 和 p2 中的对象引用。swap 方法的参数 x 和 y 被初始化为两个对象引用的拷贝，这个方法交换的是这两个的拷贝。

{% asset_img 3.png 执行过程 %}

最终在方法结束时参数变量 x 和 y都被丢弃了。原来的变量 p1 和 p2 仍然引用这个方法调用之前所引用的对象。

这个过程说明：Java程序设计语言对对象采用的不是引用调用，实际上对象引用是按值传递的。、

下面总结一下 Java 中方法参数的使用情况：

- 一个方法不能修改一个基本数据类型的参数；

- 一个方法可以改变一个对象参数的状态；

- 一个方法不能让对象参数引用一个新的对象。

## 构造器

在 Java 中每实例化一个类时都会调用类的构造器，也叫构造方法，用于确保类的初始化。

不接受任何参数的构造器叫做默认构造器，也叫无参构造器。但是和其他方法一样，构造器也能带参数，以便指定如何创建对象。

如果没有显式地为类定义构造方法，Java编译器将会为该类提供一个默认构造方法。

在创建一个对象的时候，至少要调用一个构造方法。**构造方法的名称必须与类同名，一个类可以有多个构造方法。**

```java
public class Person {
  int age = 10;
  String name = "";
  
  // 构造方法与类同名
  public Person(int myAge, String myName) {
    age = myAge;
    name = myName;
  }
  void sayAge() {
    System.out.println("age:" + age);
  }
}
```

### 方法重载

假设现在需要创建一个类，既可以用标准方法进行初始化，也可以从文件中读取信息来初始化。这就需要两个构造器：一个默认构造器，另一个取字符串作为形式参数。由于都是构造器，所以它们必须有相同的名字，即类名。**为了让方法名相同而形式参数不同的构造器同时存在，必须用到方法重载**。同时，尽管方法重载是构造器所必需的，但它也可以用于其他方法。

```java
public class Person {
  int age = 10;
  String name = "";
  
  // 构造方法与类同名
  public Person() {
  }
  public Person(int myAge, String myName) {
    age = myAge;
    name = myName;
  }
  void sayAge() {
    System.out.println("age:" + age);
  }
  void sayAge(int num) {
    System.out.println("num*age:" + age*num);
  }
}
```

**区分方法重载：**

要是几个方法有相同的名字，Java 如何才知道你指的是哪一个呢？其实规则很简单：**每个重载方法都必须有一个独一无二的参数列表**。甚至参数顺序不同也足以区分两个方法，不过一般情况下不要这样做，因为这会使代码难以维护。

但是需要注意的是：**不能根据方法的返回值来区分重载方法**。

## static 用于创建静态变量和静态方法

文件结构，一个 package 下面有以下三个类，一个 package 下的所有类都是相互可见的:

- `Main.java`     程序入口
- `Person.java`   Person类
- `Dog.java`      Dog类

**对各种变量而言，成员变量只在本类中可以访问，而用 static 声明的静态变量或方法在同一个 package 下的所有类都可以访问，相当于该 package 下的全局变量或方法。**

**因此，对于静态变量或静态方法，应使用类名访问。**

```java
// Main.java
public class Main {
  public static void main(String[] args) {
    System.out.println(Person.allAge);
    System.out.println(Dog.allAge);
  }
}
```

```java
// Person.java
public class Person {
  int age = 10;
  String name = "";
  // 声明静态变量
  static int allAge = 70;
  // 构造方法与类同名
  public Person(int myAge, String myName) {
    age = myAge;
    name = myName;
  }
  void sayAge() {
    System.out.println("person age:" + age);
  }
}
```

```java
// Dog.java
public class Dog {
  int age = 10;
  String name = "";
  // 声明静态变量
  static int allAge = 10;
  // 构造方法与类同名
  public Dog(int myAge, String myName) {
    age = myAge;
    name = myName;
  }
  void sayAge() {
    System.out.println("dog age:" + age);
  }
}
```

**静态方法可以直接调用同类中的静态成员，但不能直接调用非静态成员。**

**普通方法中可以直接使用静态或非静态变量或方法。**

```java
// Person.java
public class Person {
  int age = 10;
  String name = "";
  static int allAge = 70;
  // 构造方法与类同名
  public Person(int myAge, String myName) {
    age = myAge;
    name = myName;
  }
  void sayName() {
    // 普通方法中可以直接使用静态或非静态变量或方法
    System.out.println("person name:" + name);
    System.out.println("person allAge:" + allAge);
  }
  
  static void sayAllAge() {
    System.out.println("person allAge:" + allAge);
  }
  // 静态方法可以直接调用同类中的静态成员，但不能直接调用非静态成员。
  static void sayAge() {
    System.out.println("person age:" + age);    // 调用非静态变量，报错
    sayName();                                  // 调用非静态方法，报错
    sayAllAge();                                // 调用静态方法，成功
  }
}
```

**如果在静态方法中想要调用非静态成员，需先实例化对象。**

```java
// Person.java
public class Person {
  int age = 10;
  String name = "";
  static int allAge = 70;
  // 构造方法与类同名
  public Person(int myAge, String myName) {
    age = myAge;
    name = myName;
  }
  void sayName() {
    System.out.println("person name:" + name);
    System.out.println("person allAge:" + allAge);
  }
  
  static void sayAllAge() {
    System.out.println("person allAge:" + allAge);
  }
  static void sayAge() {
    // 在静态方法中想要调用非静态成员，需先实例化对象。
    Person person = new Person(12,"Sillywa");           // 实例化对象
    System.out.println("person age:" + person.age);    // 调用非静态变量，成功
    person.sayName();                                  // 调用非静态方法，成功
    sayAllAge();                                // 调用静态方法，成功
  }
}
```

## 使用 static 静态初始化块

**需要特别注意：静态初始化块只在类加载时执行，且只会执行一次，同时静态初始化块只能给静态变量赋值，不能初始化普通的成员变量。**

```java
// Person.java
public class Person {
  int age;
  static String name;
  static int allAge;
  {
    age = 10;
    System.out.println("为普通变量赋值");
  }
  static {
    allAge = 90;
    name = "Sillywa";
    System.out.println("为静态变量赋值");
  }
  
}
```

```java
// Main.java
public class Main {
  public static void main(String[] args) {
    Person person1 = new Person();
    Person person2 = new Person();
  }
}
```

输出结果：

```c
为静态变量赋值
为普通变量赋值
为普通变量赋值
```

**可以看出，静态赋值最先执行，当实例化两个对象时，静态初始化只被执行一次。**

## 抽象类

如果某个类只将它作为派生其他类的基类，而不想实例化它，那么可以将其设为抽象类。即抽象类不能被实例化，同时具有抽象方法的类必须声明为抽象类。

可以使用 abstrsct 关键字来声明抽象类和抽象方法。

```java
public abstract class Person {
  ...
  public abstract String getDescription();
}
```

抽象方法在抽象类中可以不必实现，但是继承抽象类的类必须实现抽象类的抽象方法。

除了抽象方法外，抽象类中还可以包含具体的数据和具体方法。

## 类的设计技巧

1. 一定要保证数据私有
绝对不要破坏封装性，因此需要编写访问器方法和更改器方法。本文代码为了简便没有按照此规范，千万不要学习这种写法。

2. 一定要对数据进行初始化
Java不会对局部变量进行初始化，但是会对成员变量进行初始化。最好不要依赖于系统的默认值，而要显示初始化所有数据。

3. 不要在类中使用过多的基本类型
尽量用其他类代替多个相关基本类型的变量的使用，这样使得类更容易理解和修改。

4. 不是所有的成员变量都需要有访问器或更改器

5. 将职责过多的类进行分解

6. 类名和方法名要有含义

7. 优先使用不可变类
