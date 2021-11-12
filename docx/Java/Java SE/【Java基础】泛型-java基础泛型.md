---
title: 【Java基础】泛型
date: 2021-09-19 11:02:23.219
updated: 2021-09-19 14:05:09.925
url: https://pumpkn.xyz/archives/java基础泛型
categories: 
tags: 学习 | Java
---

# 泛型

## 不使用泛型的问题分析

- 不能对加入到集合```ArrayList```中的数据类型进行约束（不安全）
- 便利的时候，需要进行类型转换，如果集合中的数据量较大，对效率有影响

例如，这种写法中的arrayList就只能添加```Dog```类型的数据
```Java
 ArrayList<Dog> arrayList = new ArrayList<Dog>() ;
```

## 泛型的好处
- 编译时，检查添加元素的类型，提高了安全性
- 减少了类型转换的次数，提高效率

## 对泛型的理解

- 泛型又称为参数化类型，是JDK50出现的新特性，解决数据类型的安全问题
- 在**类声明**或**实例化**时只要指定好需要的具体的类型即可
- Java泛型可以保证如果程序在编译时没有发出警告，运行时就不会产生```ClassCastException```异常。同时，代码更加简洁、健壮
- 泛型的作用是：可以在**类声明**时通过一个标识表示类中某个属性的类型，或者是某个方法的返回值类型，或者是参数类型。

## 泛型的声明与实例化

```Java
public class 泛型声明 {
    public static void main(String[] args) {
        //泛型的实例化
        //此时E为String类型
        Person<String> person = new Person<String>();
    }
}

//泛型类的声明
class Person<E>{
    //E表示s的数据类型，该数据类型在定义person对象的时候指定，即在编译期间，就确定E是什么类型
    E s ;

    public Person() {
    }

    public Person(E s) { //E也可以是参数类型
        this.s = s;
    }

    public E f(){ //返回类型为E
        return s ;
    }
}

```

# 泛型使用的细节

- ```interface List<T>{}```，```public class HashSet<E>{}```...等等

	- **T,E**只能是**引用类型**

```Java
        ArrayList<int> arrayList = new ArrayList<int>(); //错误
        ArrayList<Integer> arrayList1 = new ArrayList<>(); //正确
```

- 给泛型指定具体类后，可以传入该类型或者其子类型
- 如果这样写```List list3 = new ArrayList()```，默认给他的泛型就是Object


# 自定义发泛型类

## 基本语法

```Java
//...表示可以有多个泛型
class 类名<T,R...>{

}
```

- 普通成员可以使用泛型（属性，方法）
- 使用泛型的数组，不能初始化
- 静态方法中不能使用类的泛型
- 泛型类的类型，实在创建对象时确定的（因为创建对象时，需要指定确定类型）
- 如果在创建对象时，没有指定类型，默认为Object


# 自定义泛型接口

## 基本语法

```Java
interface 接口名<T,R...>{

}
```

- 接口中，静态成员也不能使用泛型（这个和泛型类规定一样）
- 泛型接口的类型，在**继承接口**或者**实现接口**时确定
- 没有指定类型，默认为Object

# 自定义泛型方法

## 基本语法

```Java
修饰符<T,R...> 返回类型 方法名(参数列表){

}
```

## 细节

- 泛型方法，可以定义在普通类中，也可以定义在泛型类中。
- 当泛型方法被调用时，类型会确定
- public void eat(E e){} ,修饰符后没有<T,R...> eat方法不是泛型方法，而是使用了泛型

```Java
public class 泛型介绍 {
    public static void main(String[] args) {

        泛型介绍 fanxing = new 泛型介绍();
        fanxing.fly("猫" , 100); //当调用方法时，传入参数，编译器，却会确定类型
    }

    //泛型方法
    //<R,T>就是泛型
    public <R,T> void fly(R r , T t){
        System.out.println( r );
        System.out.println( t );
    }

}
```
# 泛型的继承和通配符

- 泛型不具备继承性
- ```<?>```： 支持任意泛型类型
- ```<? extends A>```: 支持A类以及A类的子类，规定了泛型的上限
- ```<? super A>```: 支持A类以及A类的父类，不限于直接父类，规定了泛型的下限
