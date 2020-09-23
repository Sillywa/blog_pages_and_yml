---
title: Java 方法可变参数列表
date: 2019-11-04 22:12:18
top: 10
tags:
- Java 方法
categories:
- Java
---

和其他编程语言一样，当方法的形参个数不确定时，Java 语言也提供一种可变参数列表。具体如下

<!-- more -->

```java
public class NewVarArgs {
    static void printArray(Object... args) {
        for(Object obj : args) {
            System.out.print(obj + " ");
        }
        System.out.println();
    }

    static void printStr(String... args) {
		for(String str : args)
			System.out.print(str + " ");
		
		System.out.println();
	}

    static void printInt(int... args) {
        for(int i : args) {
            System.out.print(i + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        // 接受一系列形参
        printArray(new Integer(47), new Float(3.15), new Double(11.11));
        printArray(47,3.15F,11.11);
        printArray("one", "two", "three");

        // 或者数组形式
        printArray((Object[])new Integer[]{1, 2, 3, 4});
        printArray();


        printStr("I","am");
        printStr("Sillywa");

        printInt(1,2);
        printInt(1,2,3);
    }


}

```

当函数参数指定了一种类型之后，无论是传入该类型的包装类型还是基本类型，该函数都可以正常工作。

同样可变参数也会发生函数重载。

```java
public class OverLoadingVarargs {

    static void f(String... args) {
    	System.out.print("first: ");
		for(String str : args)
			System.out.print(str + " ");
		
		System.out.println();
	}

    static void f(int... args) {
    	System.out.print("second: ");
        for(int i : args) {
            System.out.print(i + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        f("I", "am", "Sillywa");
        f(1,2,3);
        f(); // error
    }
}

```
当发生函数重载时，编译器会自动匹配应该调用的方法，匹配规则与传入的形参有关，所以当不传入参数时，编译器就无法匹配正确的方法，因而报错。

因此我们可能会想到添加一个非可变参数类解决问题：

```java
public class OverLoadingVarargs {
    static void f(float i, Character... args) {
    	System.out.println("first");
    }
    
    static void f(Character... args) {
    	System.out.println("second");
    }
    public static void main(String[] args) {
        f(1,'a');
        f('a','b'); // error
    }
}

```

但是这样依然会报错，如果给这两个函数都添加一个非可变参数，问题就得以解决。

```java
public class OverLoadingVarargs {
    static void f(float i, Character... args) {
    	System.out.println("first");
    }
    
    static void f(char c, Character... args) {
    	System.out.println("second");
    }
    public static void main(String[] args) {
        f(1,'a');
        f('a','b'); // error
    }
}

```
