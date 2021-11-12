---
title: 【Java基础】接口
date: 2021-09-14 14:22:33.192
updated: 2021-09-14 14:22:33.192
url: https://pumpkn.xyz/archives/java-ji-chu--jie-kou
categories: 
tags: 学习 | Java
---

# 接口介绍
接口就是给出一些没有实现的方法，封装到一起，到某个类要使用的时候，再根据具体情况把这些方法实现。

# 语法

```Java
public interface MyInterface {
    //属性
    int age = 10 ;
    //方法(1.抽象方法，2.默认实现方法，3.静态方法)
}


public class MyInterfaceImpl implements MyInterface {
    //自己的属性
    //自己的方法
    //必须实现的接口的抽象方法
    public static void main(String[] args) {
        System.out.println(age);
    }
}
```

## 注意

- 在JDK7.0前，接口里的所有方法都没有方法体。
- 在JDK8.0后接口类可以有静态方法，默认方法，也就是说接口中可以有方法的具体实现。


# 使用细节

- 接口不能被实例化
- 接口中所有的方法是```public```方法，接口中抽象方法，可以不用abstract修饰

	- 在接口中，
		```Java
		void aaa() ;
		//实际上是
		abstract void aaa() ;
		```

- 一个普通类实现接口，就必须将该接口的所有方法都实现。
- 抽象类实现接口，可以不用实现接口的方法
- 一个类同时可以实现多个接口
- 接口中的属性，只能是final的，而且是public static final修饰的。比如: 
	
	- ```Java
	   int a = 1 ; //(必须初始化)
	    //实际上是
	    public static final int a = 1 ;
	  ```  

- 接口中属性的访问形式：接口名.属性名
- 接口不能继承其他的类，但是可以继承多个别的接口
- 接口的修饰符只能是public和默认，这点和类的修饰符是一样的。

# 实现接口VS继承类

## 接口和继承解决的问题不同
继承的价值主要在于: 解决代码的复用性和可维护性
接口的截至在于：设计号各种规范（方法），让其他类去实现这些方法。

## 接口比继承更加灵活
接口比继承更加灵活，继承是满足is-a的关系，而接口只需满足like-a的关系

## 接口在一定程度上实现代码解耦


#练习

设计一个usb接口，插上任何设备并识别

```Java
public interface USB {
    public void call() ;

    public void work() ;
}

public class Phone implements USB{
    @Override
    public void call() {
        System.out.println("手机插入。。。。");
    }

    @Override
    public void work() {
        System.out.println("手机开始工作......");
    }

    public void test(){
        System.out.println("手机测试成功......");
    }
}


public class Camera implements USB{
    @Override
    public void call() {
        System.out.println("相机插入....");
    }

    @Override
    public void work() {
        System.out.println("相机工作......");
    }

    public void back(){
        System.out.println("相机恢复正常......");
    }
}


public class Test {
    public static void main(String[] args) {
        USB[] usbs = new USB[2];
        usbs[0] = new Phone() ;
        usbs[1] = new Camera() ;
        test(usbs[0]);

        test(usbs[1]);

    }

    static public void test( USB usb ){
        usb.call();
        usb.work();
        if( usb instanceof Camera ){
            //向下转型
            Camera camera = (Camera) usb ;
            camera.back();
        }
        else if( usb instanceof Phone ){
            //向下转型
            Phone phone = (Phone) usb;
            phone.test();
        }
    }
}
```

//输出
![image.png](https://pumpkn.xyz/upload/2021/09/image-57f80e123b80403b9a968c698295d498.png)


# 多态传递
```Java
public class 多态传递 {
    public static void main(String[] args) {
        //接口类型的变量可以指向，实现了该接口的类的对象实例
        I2 i2 = new Teacher();
        //如果I2继承了I1接口，而Teacher类实现了I2接口
        //那么，实际上相当于Teacher类也实现了I1接口
        I1 i1 = new Teacher() ;
    }
}

interface I1{

}

interface I2 extends I1{

}

class Teacher implements I2{

}
```