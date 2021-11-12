---
title: 【Java基础】可变参数与对象创建流程
date: 2021-07-27 18:11:53.209
updated: 2021-07-27 18:21:11.482
url: https://pumpkn.xyz/archives/java基础可变参数与对象创建流程
categories: 
tags: 学习 | Java
---

# 可变参数

## 基本概念
Java允许将同一个类中多个同名同功能，参数同类型但个数不同的方法，封装成一个方法。可以通过可变参数实现。

## 基本语法 
```Java
访问修饰符 返回类型 方法名(数据类型... 形参名){

}
```
例如求多个数的和
```Java
public class 可变参数 {
    public static void main(String[] args) {
        varParameter(1,2,4,5) ;        //输出12

    }

    //int... 为多值参数
    public static int varParameter( int... vp ){
        int sum = 0 ;
        //多值参数的使用方法和数组相同
        for (int i = 0; i < vp.length; i++) {
            sum += vp[i];
        }
        
        System.out.println(sum);
        return 0 ;
    }

}
```

## 使用细节

- 可变参数的实参可以为0个或任意多个
- 可变参数的实参可以为数组
- 可变参数的本质就是数组
- 可变参数可以和普通类型的参数一起放在形参列表，但必须保证可变参数在最后

```Java
    public static void varP(String name , int age , int... score){   
    }
```

-  一个方法的参数只能有一个可变参数，且必须放在参数列表的最后。

# 对象创建的流程

有Person类
```Java
class Person{
    int age = 90 ;
    String name ;
    Person(String name , int age){  //构造器
        this.name = name ;
        this.age = age ;
    }
}
```
对Person类对象创建进行流程分析
```Java
Person p = new Person("周" , 22) ;
```

## 流程分析

- 加载Person类信息(Person.class)，只会加载一次
- 在堆中分配空间（地址）
- 完成对象的初始化

	- 默认初始化 age = 0 , name = null
	- 显式初始化 age = 90 , name = null 
	- 构造器初始化 age = 22 , name = "周"

- 将对象在堆中的地址，返回给p(p是对象名，可以理解成对象的引用)	