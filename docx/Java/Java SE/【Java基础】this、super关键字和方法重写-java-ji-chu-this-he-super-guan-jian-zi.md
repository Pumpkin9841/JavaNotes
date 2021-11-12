---
title: 【Java基础】this、super关键字和方法重写
date: 2021-07-27 18:38:53.538
updated: 2021-07-27 19:29:59.01
url: https://pumpkn.xyz/archives/java-ji-chu-this-he-super-guan-jian-zi
categories: 
tags: 
---

# this
Java虚拟机会给每个对象分配一个this，代表当前对象。简单地说，**哪个对象调用，this就代表哪个对象**。
![5this.png](https://pumpkn.xyz/upload/2021/07/5this-ad2d078970bf455da0c592a4de4d1bd9.png)

## this使用细节

- this关键字可以用来访问本类的属性、方法、构造器。
- this用于区分当前类的属性和局部变量
- 访问成员方法的语法

	```Java
	this.方法名(参数列表)
	```

- 构造器访问另一构造器语法：
	
	```Java
	  //注意只能在构造器中使用，且必须放在第一句
	  this(参数列表)
	```

- this不能在类定义的外部使用，只能在类定义的方法中使用


# super

## 基本使用
super代表父类的引用，用于访问父类的属性、方法、构造器。

## 使用细节

- 访问父类的属性，但不能访问父类的private属性

	```Java
	    super.属性名;
	```

- 访问父类的方法，不能访问父类的private方法

	```Java
   	   super.方法名(参数列表)
	```

- 访问父类的构造器

	```Java
 	    //只能放在构造器的第一句，只能出现一句
	    super(参数列表)
	```

例如
```Java
public class A {
    public int n1 = 100 ;
    protected  int n2 = 200 ;
    int n3 = 300 ;
    private  int n4 = 400 ;
    public void test100(){}
    protected  void test200(int age){}
    void test300(){}
    private  void test400(){}
}

public class B extends A{

    public void say(){
        System.out.println(super.n1);
        System.out.println(super.n2);
        System.out.println(super.n3);
        //访问不了n4属性，因为父类A中的n4被private修饰

        super.test100();
        super.test200(200);
        super.test300();
        //不能直接访问test400()方法，因为父类A中的test400()方法被private修饰
    }
}

```

## super的好处

- 调用父类的构造器的好处（分工明确，父类属性由父类初始化，子类的属性由子类初始化）
- 当子类中有和父类中的成员（属性和方法）重名时，为了访问父类的成员，必须通过super。如果没有重名，使用super、this、直接访问是一样的效果。
- super的访问不限于直接父类，如果爷爷类和本类中有同名的成员，也可以使用super去访问爷爷类的成员；如果多个上级类中都有同名成员，使用super访问遵循就近原则。

![image.png](https://pumpkn.xyz/upload/2021/07/image-693a75ec0afb4e46add96c9311a62f2d.png)

调用某一属性时，总是先从本类中找，找不到再去父类中找，父类中找不到再去父类的父类中找，直至最后。


![image.png](https://pumpkn.xyz/upload/2021/07/image-10e92849b2eb476291af0b1418a0787a.png)


# 方法重写/覆盖(override)

## 基本概念
方法重写就是子类有一个方法，和父类的某个方法的名称、返回类型、参数一样，那么我们就说子类的这个方法覆盖了父类的方法 

## 使用细节
方法重写，需要满足下列条件
- 子类方法的参数、方法名称，要和父类方法的参数,方法名称完全一样。
- 子类方法的返回类型和父类方法的返回类型一样，或者是父类返回类型的子类</br>*比如父类返回类型是Object，子类方法返回类型是String*
- 子类方法不能缩小父类方法的访问权限</br>*权限： public > protected > 默认 > private*