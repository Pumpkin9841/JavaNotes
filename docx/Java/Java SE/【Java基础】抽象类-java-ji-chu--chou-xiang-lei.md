---
title: 【Java基础】抽象类
date: 2021-09-14 13:34:44.538
updated: 2021-09-14 13:34:44.538
url: https://pumpkn.xyz/archives/java-ji-chu--chou-xiang-lei
categories: 
tags: 学习 | Java
---

# 抽象类
当父类的某些方法，需要声明，但是又不确定具体实现时，可以将其声明为抽象方法，那么这个类就是抽象类。

# 用法
当父类的一些方法不能确定时，可以用```abstract```关键字来修饰该方法，这个方法就是抽象方法。
</br>
用```abstract```来修饰该类就是抽象类。

例如把Animal做成抽象类，并让子类Cat类实现

```Java
public class Test {
    public static void main(String[] args) {
        Cat cat = new Cat();
        System.out.println(cat.age);
    }
}


abstract  class Animal{
    int age = 1;
    String name ;
    abstract public void cry();
}

class  Cat extends Animal {

    @Override
    public void cry() {
        System.out.println("猫哭");
    }
}
```

输出
![image.png](https://pumpkn.xyz/upload/2021/09/image-fe0f28b09ae1427dacdc92aae61676d1.png)

# 抽象类的介绍

- 用abstract关键字来修饰一个类时，这个类就叫抽象类。

- 用abstract关键字来修饰一个方法时，这个方法就是抽象方法。抽象方法没有方法体

- 抽象类的价值更多作用是在于设计，是设计者设计好后，让子类继承并实现抽象类

- 抽象类，在框架和设计模式使用较多


# 抽象类使用细节

- **抽象类不能被实例化**
- 抽象类不一定要包含abstract方法。也就是说，抽象类可以没有abstract方法
- 一旦类包含了abstract方法，则这个类必须声明为abstract
- abstract只能修饰类和方法，不能修饰属性和其他的
- 抽象类可以有任意成员【**抽象类本质还是类**】，比如：非抽象方法、构造器、静态属性等等
- 抽象方法不能有方法体
- 如果一个类继承了抽象类，则它必须实现抽象类的所有抽象方法，除非他自己也声明为abstract类
- 抽象方法不能使用private、final、static来修饰，因为这些关键字都是和重写相违背的