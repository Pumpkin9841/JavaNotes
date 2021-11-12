---
title: 【Java基础】详解main方法
date: 2021-08-16 15:02:23.995
updated: 2021-08-16 15:02:23.995
url: https://pumpkn.xyz/archives/java-ji-chu--xiang-jie-main-fang-fa
categories: 
tags: 学习 | Java
---

# 深入理解main方法

```Java
    public static void main(String[] args) {
        
    }
```


- main方法是**Java虚拟机**调用的。

- Java虚拟机需要调用类的main()方法，所以该方法的访问权限必须是```public```。

- Java虚拟机在执行main()方法时不必创建对象，所以该方法必须是static。

- 该方法接收String类型的数组参数，该数组中保存执行Java命令时传递给所运行的类的参数。

## 特别注意

- 在main()方法中，可以直接调用main方法所在类的静态方法或静态属性。

- 但是，不能直接访问该类中的非静态成员，必须创建一个该类的实例对象后，才能通过这个对象去访问该类中的非静态成员。


## 示例

```Java
public class Test {

    public static String name = "周" ;
    public int age = 22 ;

    public static void main(String[] args) {
        //main方法所在类的静态变量可以直接调用
        System.out.println(name);

        //该类的非静态成员，需要先实例化对象，再通过对象调用
        Test test = new Test();
        System.out.println(test.age);

    }
}

```
