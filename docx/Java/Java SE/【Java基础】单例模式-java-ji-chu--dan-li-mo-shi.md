---
title: 【Java基础】单例模式
date: 2021-08-16 23:55:42.543
updated: 2021-08-16 23:55:42.543
url: https://pumpkn.xyz/archives/java-ji-chu--dan-li-mo-shi
categories: 
tags: 学习 | Java
---

# 什么是单例模式
单例：单个实例

- 所谓的单例设计模式，就是采取一定的方法保证在整个的软件系统中，对某个类只能存在一个对象实例，并且该类只提供一个取得其对象实例的方法。

- 单例模式有两种方式

	- 饿汉式
	- 懒汉式


 
# 单例模式应用实例

## 饿汉式和懒汉式实现步骤

- 构造器私有化 -> 防止直接```new```新对象

- 类的内部创建对象

- 向外暴露一个静态的公共方法返回该对象实例 -> getInstance 

## 饿汉式代码实现
饿汉式解释：还没用到该单例对象时，就已经创建好了，饿汉式可能造成创建了对象，但是没有使用

```Java

public class 单例模式1饿汉式 {
    public static void main(String[] args) {
        GirlFriend girFriend = GirlFriend.getGirFriend();
        System.out.println(girFriend);
        GirlFriend girFriend1 = GirlFriend.getGirFriend();
        System.out.println(girFriend1);
    }
}

class GirlFriend{
//饿汉式解释：还没用到该单例对象时，就已经创建好了
//饿汉式可能造成创建了对象，但是没有使用
    private String name ;
    private static GirlFriend gf = new GirlFriend("鑫鑫") ;

    private GirlFriend(String name) {
        this.name = name;
    }

    public static GirlFriend getGirFriend(){
        return gf ;
    }

    @Override
    public String toString() {
        return "GirlFriend{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

## 懒汉式代码实现
懒汉式：只有当用户使用getInstance方法时，才会创建单例对象，然后再次调用时，会返回上次创建的对象。

```Java
public class 单例模式懒汉式 {
    public static void main(String[] args) {
        Cat cat = Cat.getCat();
        System.out.println(cat.getName());
        Cat cat1 = Cat.getCat();
        System.out.println(cat == cat1);
    }
}

class Cat{

    private String name  ;

    private static Cat cat = null ;
    private Cat(String name) {
        System.out.println("构造器被调用....");
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public static Cat getCat(){
        if( cat == null ){
            cat = new Cat("笨笨");
        }
        return cat ;
    }
}

```

# 饿汉式VS懒汉式

- 二者最主要的区别在于创建对象的**时机**不同

	- 饿汉式是在类加载时就创建了对象实例
	- 懒汉式实在使用该对象时才创建

- 饿汉式不存在线程安全问题，懒汉式存在线程安全问题

- 饿汉式存在浪费资源的可能。因为如果程序员一个对象实例都没有使用，那么饿汉式创建的对象就浪费了，懒汉式是使用时才创建对象，就不存在这个问题

- 在JavaSE标准类中，java.lang.Runtime就是经典的单例模式


 


