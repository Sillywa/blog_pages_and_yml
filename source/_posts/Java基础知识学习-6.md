---
title: Java 多态
date: 2019-11-23 18:56:24
top: 10
tags:
- Java基础
categories:
- Java
---

一个变量对象可以指示多种实际类型的现象称为多态。在运行时能够自动选择调用哪个方法的现象称为自动绑定。

以下我们定义两个类 Employee 类和其子类 Manager 类。

<!-- more -->

```java
import java.time.LocalDate;

public class Employee {
    /**
    * Employee类为员工类，其含有姓名，薪水，雇佣日期这些成员变量
    * 有获取姓名、薪水、雇佣日期的方法
    * 同时还有涨工资的方法
    */
    private String name;
    private double salary;
    private LocalDate hireDay;

    public Employee(String name, double salary, int year, int month, int day) {
        this.name = name;
        this.salary = salary;
        this.hireDay = LocalDate.of(year, month, day);
    }

    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }

    public LocalDate getHireDay() {
        return hireDay;
    }

    public void raiseSalary(double byPercent) {
        double raise = salary * byPercent / 100;
        salary += raise;
    }


}


```

```java
public class Manager extends Employee {
    /**
    * Manager类为经理类，其继承员工类，除了员工的基本工资以外，经理还有奖金
    */
    private double bonus;
    public Manager(String name, double salary, int year, int month, int day) {
        super(name, salary, year, month, day);
        this.bonus = 0;
    }
    @Override
    public double getSalary() {
        // TODO Auto-generated method stub
        double baseSalary = super.getSalary();
        return baseSalary + bonus;
    }
    public void setBonus(double bonus) {
        this.bonus = bonus;
    }


}

```

```java
public class ManagerTest {
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        Manager boss = new Manager("Sillywa", 6000, 2009, 12, 5);
        boss.setBonus(1000);

        Employee[] staff = new Employee[3];
        staff[0] = boss;
        staff[1] = new Employee("xin", 3000, 2012, 3, 4);
        staff[2] = new Employee("yuan", 3000, 2012, 4, 5);

        for(Employee e:staff) {
            System.out.println(e.getName() + ":" + e.getSalary());
        }
    }

}
```

在上面的代码中，我们定义了一个 Employee 类型的数组，并且把 staff[0] 的类型设置为 Manager 类型，此时并没有报错。因为 Manager 类继承于 Employee 类，所以一个 Manager 类也是一个 Employee 类，这就是多态的典型例子。

换句话说，一个 Employee 变量既可以引用一个 Employee 类对象，也可以引用一个 Employee 类的任何一个子类的对象，如 Manager 对象。

在最后的遍历中，当 e 引用 Employee 类的对象时，e.getSalary() 调用的是 Employee 类中的 getSalary 方法；当 e 引用 Manager 对象时，e.getSalary() 调用的是 Manager 类中的 getSalary 方法。虚拟机知道 e 的实际引用的对象类型，因此能够正确调用相应的方法。

在上面例子中：

```java
Manager boss = new Manager(...);
Employee[] staff = new Employee[3];
staff[0] = boss;
```

在这个例子中，变量 staff[0] 与 boss 引用的同一个对象。但是编译器将 staff[0] 看成 Employee 对象。

这意味着：

```java
boss.setBonus(1000); // OK
```

但是不能这样调用：

```java
staff[0].setBonus(1000); // Error
```

这是因为 staff[0] 声明的类型是 Employee，而 setBonus 不是 Employee 类的方法。
