---
title: 【Java基础】异常
date: 2021-09-15 16:19:32.144
updated: 2021-09-15 17:02:39.998
url: https://pumpkn.xyz/archives/java基础异常
categories: 
tags: 学习 | Java
---

# 异常介绍
Java语言中，将程序执行中发生的不正常情况称为“异常”。（开发过程中的语法错误和逻辑错误不是异常）

# 执行过程中所发生的异常事件可分为两类

- Error（错误）：Java虚拟机无法解决的严重问题。如：JVM系统内部错误、资源耗尽等严重问题。比如：StackOverflowError[栈溢出]和OOM(out of memory)，Error是严重错误，程序会崩溃。

- Exception：其他因编程错误或偶然的外在因素导致的一般性问题，可以使用针对性的代码进行处理。例如：空指针访问，试图读取不存在的文件，网络连接中断等等。

- **Exception分为两大类**

	- 运行时异常
	- 编译时异常


# 异常体系结构

- 异常分为两大类，运行时异常和编译时异常
- 运行时异常，编译器不要求强制处置的异常。一般是指编程时的逻辑错误，是程序员应该避免其出现的异常。</br>```java.lang.RuntimeException类及它的子类```都是运行时异常
- 对于运行时异常，可以不做处理，因为这类异常很普遍，若全处理可能会对程序的可读性和运行效率产生影响
- 编译时异常，是编译器要求必须处置的异常


![3.jpg](https://pumpkn.xyz/upload/2021/09/3-73327fe3f1144edb83ef941cccacf5f7.jpg)


# 常见的运行时异常

- ```NullPointerException```空指针异常

	- 当应用程序试图在需要对象的地方使用```null```时，抛出该异常

- ```ArithmeticException```数字运算异常

	- 当出现异常的运算条件时，抛出此异常。例如：一个整数“除以零”时，抛出此类的一个实例。

- ```ArrayIndexOutOfBoundsException```数组下标越界异常

	- 用非法索引访问数组时抛出的异常。如果索引为负或大于等于数组大小，则该索引为非法索引

- ```ClassCastException```类型转换异常

	- 当试图将对象强制转换为不是实例的子类时，抛出该异常。

- ```NumberFormatException```数字格式不正确异常

	- 当应用程序试图将字符串转换成一种数值类型，但该字符串不能转换为适当格式时，抛出该异常，抛出该异常-->使用异常我们可以确保输入是满足条件的数字

# 编译异常

编译异常是指在编译期间，就必须处理的异常，否则代码不能通过编译。

# 常见的编译异常

- ```SQLException``` 操作数据库时，查询表可能发生异常
- ```IOExceotion``` 操作文件时，发生的异常
- ```FileNotFoundException``` 当操作一个不存在的文件时，发生异常
- ```ClassNotFoundException``` 加载类，而该类不存在时，异常
- ```EOFException``` 操作文件，到文件末尾，发生异常
- ```IllegalArguementException``` 参数非法异常


# 异常处理

## 异常处理介绍
异常处理就是当异常发生时，对异常处理的方式

## 异常处理的方式

- ```try-catch-finally```

	- 程序员在代码中捕获发生的异常，自行处理

- ```throws```

	- 将发生的异常抛出，交给调用者（方法）来处理，最顶级的处理者就是JVM


## ```try-catch```方式处理异常说明

- Java提供try和catch块来处理异常。try块用于包含可能出错的代码。catch块用于处理try块中发生的异常。可以根据需要在程序中有多个数量的try...catch块。
- 基本语法

	- ``` Java
		try{
		  //可疑代码
		  //将异常生成对应的异常对象，传递给catch块	
		}catch(异常){
		  //对异常的处理
		}
         ```


## ```try-catch```方式处理异常注意事项

- 如果异常发生了，则异常发生后面的代码不会执行，直接进入到catch块。
- 如果异常没有发生，则顺序执行try的代码块，不会进入到catch块
- 如果希望不管是否发生异常，都执行某段代码（比如关闭连接，释放资源等）则使用```finally{}```

	- ``` Java
		try{
		  //可疑代码
		  //将异常生成对应的异常对象，传递给catch块	
		}catch(异常){
		  //对异常的处理
		}finally{
		 //....
		}
         ``` 

- 可以有多个catch语句，捕获不同的异常（进行不同的业务处理），要求父类异常在后，子类异常在前，比如(Exception在后，NullPointerException在前)，如果发生异常，只会匹配一个catch。

- 可以进行```try-finally```配合使用，这种用法相当于没有捕获异常，因此程序会直接崩掉/推出。应用场景：执行一段代码，不管是否发生异常，都必须执行某个业务逻辑。

## 练习
如果用户输入的不是一个整数，就提示他反复输入，知道输入一个整数为止。

```Java
public class z1_try_catch实践 {
    public static void main(String[] args) {
        int num ;
        while( true ){
            Scanner in = new Scanner(System.in) ;
            System.out.println("请输入一个整数");
            try {
                num = Integer.parseInt(in.next());
                break;
            } catch (NumberFormatException e) {
                System.out.println("你输入的不是数字");
            }
        }
        System.out.println(num);
    }
}
```


# throws异常处理

## throws基本介绍

- 如果一个方法（中的语句执行时）可能生成某种异常，但是并不能确定如何处理这种异常，则此方法应显示地声明抛出异常，表明该方法将不对这些异常进行处理，而由该方法的调用者负责处理
- 在方法声明中throws语句可以声明抛出异常的列表，throws后面的异常类型可以是方法中产生的异常类型，也可以是它的父类

## throws注意事项

- 对于编译异常，程序中必须处理，比如```try-catch```或者```throws```
- 对于运行时异常，程序中如果没有处理，默认就是throws的方式处理
- 子类重写父类的方法时，对抛出异常的规定：**子类重写的方法，所抛出的异常类型要么和父类抛出的异常一致，要么为父类抛出的异常的类型的子类型**
- 在throws过程中，如果有方法```try-catch```，就相当于处理异常，就可以不必trhows


# 自定义异常

## 基本概念
当程序中出现了某些“错误”，但该错误信息并没有在Throwable子类中描述处理，这个时候可以自己设计异常类，用于描述该错误信息

## 自定义异常步骤

- 自定义类：自定义异常类名 继承 ```Exception```或```RuntimeException```
- 如果继承Exception，属于编译异常
- 如果继承RuntimeException，属于运行时异常。（一般来说，继承RuntimeException）

## 自定义异常练习

当接受到年龄时，要求范围在18-120之间，否则抛出一个自定义异常。

```Java
public class z2_自定义异常引用 {
    public static void main(String[] args) {
        int age = 80 ;
        if( age <= 120 && age >= 18 ){
            throw new CustomException("年龄错误") ;
        }
        System.out.println(age);
    }
}

class CustomException extends RuntimeException{
    public CustomException(String message) {
        super(message);
    }
}
```


# ```throw```和```throws```的区别

||意义|位置|后面跟的东西|
|-------|-------|-------|-------|
|throws|异常的处理方式|方法声明处|异常类型|
|throw|手动生成异常对象的关键字|方法体中|异常对象|
