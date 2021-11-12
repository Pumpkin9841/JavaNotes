---
title: 【Java基础】final关键字
date: 2021-09-13 21:56:44.666
updated: 2021-09-13 23:37:01.458
url: https://pumpkn.xyz/archives/final关键字
categories: 
tags: 学习 | Java | final
---

# 基本介绍
final可以修饰类、属性、方法和局部变量</br>
在某些情况下，程序员可能有以下需求，就会使用到final

- **当不希望类被继承时** ， 可以用final修饰

- **当不希望父类的某个方法被子类覆盖/重写(override)时**，可以用final关键字修饰

- **当不希望类的某个属性的值被修改**，可以用final修饰

	- ```java
	    public final double TAX_RATE = 0.08	
         ```  

- **当不希望某个局部变量被修改**，可以使用final修饰
	- ```java
	    final double TAX_RATE = 0.08	
         ```  

# final使用注意事项和细节

- final修饰的属性又叫常量，一般用XX_XX大写英文命名

- final修饰的属性在定义时，必须赋初值，并且以后不能再修改，赋值可以在如下位置之一

	- 在定义时 ，如public final double TAX = 0.08
	- 在构造器中
	- 在代码块中  

- 如果final修饰的属性是静态的，则初始化的位置只能是

	- 定义时
	- 在静态代码块中
	- （不能在构造器中赋值） 

- final类不能继承，但是可以实例化对象
- 如果类不是final类，但是含有final方法，则该方法虽然不能重写，但是可以被继承。

- 一把来说，如果一个类已经是final类了，就没有必要再将方法修饰诚final方法
- final不能修饰构造方法
- final和static往往搭配使用，效率更高，不会导致类加载，底层编译器做了优化处理
- 包装类(Integer , Double , Float ,Boolean等都是final),String也是final类



final修饰引用变量时，只是限定了引用变量的引用不可改变，即不能将value再次引用另一个Value对象，但是引用的对象的值是可以改变的


