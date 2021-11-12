---
title: 【Java基础】代码块
date: 2021-08-16 15:12:54.634
updated: 2021-08-16 15:40:30.3
url: https://pumpkn.xyz/archives/java基础代码块
categories: 
tags: 学习 | Java
---

# 代码块

## 代码块概述

代码块又称为**初始化块** ， 属于类中的成员（即代码块也是类的一部分），类似于方法，将逻辑语句封装在方法中，通过```{}```包围起来。
</br>
但和方法不同的是，代码块没有方法名，没有返回值，没有参数，只有方法体，而且不能通过对象或类显示调用，而是加载类时或创建对象时隐式调用。

## 基本语法

```Java
    [修饰符]{
	代码
    } ;

```

注意：</br>

- 修饰符可写也可以不写，要写的话只能写```static```。

- 代码块分为两类，使用```static```修饰的叫```静态代码块```，没有static修饰的，叫```普通代码块```。

- 逻辑语句可以为任何逻辑语句（输入、输出、方法调用、循环、判断等）。

- 末尾```;```号可以写上，也可以省略。

> 我的理解：
> 1. 相当于另外一种形式的构造器（对构造器的补充机制），可以做初始化操作。
> 2. 场景：如果多个构造器中都有重复的语句，可以抽取到代码块中，提高代码的重用性。


## 示例

```Java
class Movie{


    private String name ;
    private double price ;
    private String author ;


    public Movie(String name) {
        System.out.println("电影设备打开......");
        System.out.println("广告开始.........");
        System.out.println("电影正式开始......");
        this.name = name;
        System.out.println("Movie(String name) 被调用");
    }

    public Movie(String name, double price) {
        System.out.println("电影设备打开......");
        System.out.println("广告开始.........");
        System.out.println("电影正式开始......");
        this.name = name;
        this.price = price;
        System.out.println("Movie(String name, double price) 被调用");
    }

    public Movie(String name, double price, String author) {
        System.out.println("电影设备打开......");
        System.out.println("广告开始.........");
        System.out.println("电影正式开始......");
        this.name = name;
        this.price = price;
        this.author = author;
        System.out.println("Movie(String name, double price, String author) 被调用");
    }
}

```

上面的三个构造器都有相同的语句，这样代码就会比较冗余，这时就可以把相同的语句放入到一个代码块中。</br>

这样不管调用哪个构造器创建对象，都会先调用代码块的内容。（**代码块在构造器之前被调用**）

```Java
public class CodeBlock1 {
    public static void main(String[] args) {
        Movie movie = new Movie("海贼王");
        System.out.println("================");
        Movie movie1 = new Movie("警察故事", 36);
    }
}

class Movie{


    private String name ;
    private double price ;
    private String author ;

    {
        System.out.println("代码块——电影设备打开......");
        System.out.println("代码块——广告开始.........");
        System.out.println("代码块——电影正式开始......");
    };

    public Movie(String name) {
        this.name = name;
        System.out.println("Movie(String name) 被调用");
    }

    public Movie(String name, double price) {
        this.name = name;
        this.price = price;
        System.out.println("Movie(String name, double price) 被调用");
    }

    public Movie(String name, double price, String author) {
        this.name = name;
        this.price = price;
        this.author = author;
        System.out.println("Movie(String name, double price, String author) 被调用");
    }
}
```

运行效果：
![image.png](https://pumpkn.xyz/upload/2021/08/image-99c059394fae400ab46b858277ae4fed.png)


# 静态代码块

## 静态代码块概述

- static代码块也叫静态代码块，作用就是对类进行初始化，而且它随着**类的加载**而执行，并且只会执行一次。**如果是普通代码块，没创建一个对象，就执行一次**。

- 类什么时候被加载？

	- 创建对象实例时(new)
	- 创建子类对象实例，父类也会被加载
	- 使用类的静态成员时（静态属性，静态方法）

- 普通代码块，在创建对象实例时，会被隐式调用。创建一个对象，就会被调用一次。</br> 如果只是使用类的静态成员时，普通代码块并不会执行。

- 创建一个对象时，在一个类中调用顺序是：(<font color = " red ">重要 </font>)

- 调用静态代码块和静态属性的初始化：

	- 静态代码块和静态属性的初始化调用优先级一样，如果有多个静态代码块和多个静态变量初始化，则按他们定义的顺序调用

- 调用普通代码块和普通属性的初始化：

	- 普通代码块和普通属性的初始化的优先级是一样的，如果有多个普通代码块和多个普通属性初始化，则按定义顺序调用。

- 调用构造方法。

- 构造器的最前面其实隐含了```super()```和调用```普通代码块```

- 静态相关的代码块、属性的初始化，在类加载时，就执行完毕了，因此是优先于普通代码块和构造器的。


## 示例：同一个类中调用顺序

```Java
public class 同一个类中调用顺序 {

    public static void main(String[] args) {
        //实例化对象时，优先级为 静态 > 普通 > 构造器
        //同一优先级则按代码顺序调用
        A a = new A();


    }
}

class A{
    //普通变量初始化
    private int a1 = getA1() ;

    //普通代码块
    {
        System.out.println("普通代码块被调用...");
    }

    public int getA1(){
        System.out.println("普通getA2被调用");
        return 100 ;
    }

    //静态变量初始化
    private static int a2 = getA2() ;

    //静态变量
    static {
        System.out.println("静态代码块被调用...");
    };

    public static int getA2(){
        System.out.println("静态getA2被调用....");
        return 200 ;
    }

    //构造器
    public A() {
        //隐含了super()
        //隐式调用了普通代码块
        System.out.println("构造器A()被调用...");
    }
}
```
执行结果：
![image.png](https://pumpkn.xyz/upload/2021/08/image-c006ffa0264347808f27a160fc770837.png)


## 示例：构造器隐含super

```Java
public class 构造器隐含super {
    public static void main(String[] args) {
        BB bb = new BB(); //调用顺序？
    }
}

class AA{
    {
        System.out.println("AA的普通代码块");
    }

    static {
        System.out.println("AA的静态代码块");
    }

    public AA() {
        //隐含了super(),由于AA继承Object类,所以没有输出
        //隐含了调用本类的普通代码块和普通属性初始化
        System.out.println("AA的构造器");
    }
}

class BB extends AA{
    {
        System.out.println("BB的普通代码块");
    }

    static{
        System.out.println("BB的静态代码块");
    }

    public BB() {
        //隐含了super()
        //隐含了调用本类的普通代码块和普通属性初始化
        System.out.println("BB的构造器");
    }
}


```

执行结果：
![image.png](https://pumpkn.xyz/upload/2021/08/image-e86ba4cd86e84c0480bb7a832272260a.png)

## 小结

如上示例所示，创建一个子类对象时（继承关系），它们的静态代码块、静态属性初始化，普通代码块，普通属性出书画，构造方法的调用顺序如下：

1. 父类的静态代码块和静态属性
2. 子类的静态代码块和静态属性
3. 父类的普通代码块和普通属性初始化
4. 父类的构造方法
5. 子类的普通代码块和普通属性初始化
6. 子类的构造方法


静态代码块只能直接调用静态成员（静态属性和静态方法），普通代码块可以调用任意成员。

 
 
