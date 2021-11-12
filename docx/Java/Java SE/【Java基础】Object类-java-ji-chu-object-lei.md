---
title: 【Java基础】Object类
date: 2021-08-02 16:01:16.955
updated: 2021-08-15 23:19:21.279
url: https://pumpkn.xyz/archives/java-ji-chu-object-lei
categories: 
tags: 学习 | Java
---

# Object类

## equals方法

### ==和equals的对比

- ==：既可以判断基本类型，又可以判断引用类型
- ==：如果判断基本类型，判断的是值是否相等
- ==：如果判断的是引用类型，判断的是地址是否相等，即判定是不是同一个对象。
- equals：是Object类中的方法，只能判断引用类型
- equals：默认判断的是地址是否相等，子类往往重写该方法，用于判断内容是否相等。比如String类。

### 重写Person类的equals方法，判断两个Person类对象是否相等

```Java
public class Person {
    public int age ;
    public String name ;

    public Person(int age, String name) {
        this.age = age;
        this.name = name;
    }

    @Override
    public boolean equals( Object o ){
        if( this == o ){
            return true ;
        }

        if( o instanceof Person ){
            Person p = (Person)o ; //向下转型
            return this.name.equals(p.name) && this.age == p.age ;
        }
        return false ;
    }

}

class testPerson{
    public static void main(String[] args) {
        Person p1 = new Person( 22 , "周");
        Person p2 = new Person( 22 , "周");
        Person p3 = new Person( 21 , "刘");

        System.out.println(p1.equals(p2)); // true
        System.out.println(p1.equals(p3)); //false

    }
}

```

## hashCode
- 提高具有哈希结构的容器的效率
- 两个引用，如果指向同一个对象，则哈希值肯定是一样的
- 两个引用，如果指向的是不同对象，则哈希值是不一样的
- 哈希值主要根据地址号来，不能完全将哈希值等价于地址


## toString

- 默认返回：全类名+@+哈希值的十六进制，子类往往重写toString方法，用于返回对象的属性信息。


</br>

toString源码

```Java
    public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }
```

- 重写toString方法，打印对象或拼接对象时，都会自动调用该对象的toString形式。
- 当直接输出一个对象时，toString方法会默认的被调用。

## finalize方法
- 当对象被回收时，系统自动调用该对象的finalize方法。子类可以重写该方法，做一些释放资源的操作。
- 什么时候被回收：当某个对象没有任何引用时，则jvm就认为这个对象是一个垃圾对象，就会使用垃圾回收机制销毁该对象，在销毁该对象前，会先调用finallize方法。
- 垃圾回收机制的调用，是由系统来决定，也可以通过System.gc()主动触发垃圾回收机制。


