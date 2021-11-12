---
title: 【Java基础】内部类
date: 2021-09-14 18:03:28.689
updated: 2021-09-14 19:16:10.509
url: https://pumpkn.xyz/archives/java基础内部类
categories: 
tags: 学习 | Java
---

# 内部类介绍
一个类的内部又完整的嵌套了另一个类结构。被嵌套的类称为内部类(inner class)，嵌套其他类的类称为外部类(outer class)。</br>
内部类是我们类的第五大成员(属性、方法、构造器、代码块、内部类)，内部类最大的特点就是可以直接访问私有属性，并且可以体现类与类之间的包含关系。


# 基本语法

```Java
    class Outer{  //外部类
		class Inner{  //内部类
	
		}
    }

    class Other{  //外部其他类

    }
```

# 内部类的分类

## 定义在外部类的局部位置上（比如方法内）：

- 局部内部类（有类名）
- 匿名内部类（没有类名，重点！！！）

## 定义在外部类的成员位置上

- 成员内部类（没用static修饰）
- 静态内部类（使用static修饰）



# 局部内部类的使用
说明：局部内部类是定义在外部类的局部位置，比如方法中，并且有类名。

- 可以直接访问外部类的所有成员，包含私有的
- 不能添加访问修饰符，因为它的地位就是一个局部变量。**局部变量是不能使用修饰符的**。但是可以使用final修饰，因为局部变量也可以使用final
- 作用域：仅仅在定义它的方法或代码块中
- 局部内部类--->访问--->外部类的成员【访问方式：直接访问】
- 外部类--->访问--->局部内部类的成员【访问方式：创建对象，再访问(注意：必须在作用域内)】
- 外部其他类--->不能访问--->局部内部类(因为局部内部类地位是一个局部变量)
- 如果外部类和局部内部类的成员重名时，默认遵循就近原则，如果想访问外部类的成员，则可以使用(外部类名.this.成员)去访问。

```Java
public class z1_LocalInerClass { //外部其他类
    public static void main(String[] args) {
        Outer1 outer1 = new Outer1();
        outer1.m1();
    }
}

class Outer1{   //外部类
    private int n1 = 100 ; //私有属性
    private int n2 = 200 ;
    private void m2(){  //私有方法
        System.out.println("outer1 m2方法被调用");
    }

    public void m1(){ //方法
        //1.局部内部类是定义在外部类的局部位置，如方法中或代码块中
        //3.不能添加访问修饰符，但是可以使用final修饰
        //4.作用域：仅仅在定义他的方法或代码块中
        final class Inner01{ //局部内部类（本质仍然是一个类）
            private int n2 = 800 ;
            public void f1(){
                //2.可以直接访问外部类的所有成员，包含私有的
                System.out.println("n1=" + n1);
                //7.如果外部类和局部内部类的成员重名时，默认遵循就近原则，如果想要访问外部类的成员，则可以使用(外部类名.this.成员)
                //Outer01.this本质就是外部类对象，即哪个对象调用了m1，Outer01.this就是哪个对象
                System.out.println("内部类n2=" + n2 + "  外部类n2=" + Outer1.this.n2); //输出 内部类n2=800  外部类n2=200
                m2();
            }
        }

        //5.外部类在方法中，可以创建局部内部类的对象，然后调用方法
        Inner01 inner01 = new Inner01();
        inner01.f1();
    }

}
```

# 匿名内部类的使用（重要！）

- 本质是类
- 内部类
- 该类没有名字
- 同时还是一个对象


</br>
说明：匿名内部类是定义在外部类的局部位置，比如方法中，并且没有类名


# 匿名内部类的基本语法
```Java
 new 类或接口(参数列表){
	类体
};
```


示例

```Java
public class z2_AnonymousInnerClass {
    public static void main(String[] args) {
        Outer2 outer2 = new Outer2();
        outer2.method();
    }
}

class Outer2{
    public void method(){
        //1.需求：使用IA接口，并创建对象，调用方法
        //2.传统方式：写一个类，实现该接口，并创建对象
        Tiger tiger = new Tiger();
        tiger.cry();
        //3.现在的需求：Tiger类只使用一次，后面不再使用。若这样的需求多了，单独为其创建一个类相当繁琐
        //4. 可以使用匿名内部类解决
        //5. tiger1的编译类型 -> IA
        //6.tiger1的运行类型 -> 匿名内部类 xxx -> 外部类名称$1
        /*
        *   我们看底层
        *   class xxx implements IA{
        *       @Override
                public void cry() {
                    System.out.println("匿名内部类--老虎叫");
                }
        *   }
        * */
        //7. JDK底层在创建匿名内部类Outer02$1后，立即马上就创建了Outer02$1实例，并把地址返回给tiger1
        IA tiger1 = new IA(){

            @Override
            public void cry() {
                System.out.println("匿名内部类--老虎叫");
            }
        };
        tiger1.cry();

        System.out.println("========分割线=========");
        //演示基于类的匿名内部类
        Father father = new Father("周"); // 正常实例化facther对象，运行类型为Father
        father.say();
        //编译类型为Father，运行类型为匿名内部类
        /*
        *   底层会创建匿名内部类
        *   class Outer02$2 extends Father{
        *       @Override
                public void say(){
                    System.out.println("匿名内部类--hello world");
                }
        *   }
        * */
        //参数列表("周")传入构造器
        Father father1 = new Father("周") { //后接{}，匿名内部类写法，运行类型为匿名内部类
            @Override
            public void say(){
                System.out.println("匿名内部类--hello world");
            }
        };
        father1.say();

        System.out.println(father.getClass()); //输出father的运行类型->Father
        System.out.println(father1.getClass()); //输出father1的运行类型->Outer2$2

    }
}

interface IA{
    public void cry() ;
}

class Tiger implements IA{

    @Override
    public void cry() {
        System.out.println("老虎叫.....");
    }
}

class Father{
    public Father( String name ){
        System.out.println("接收到了name=" + name);
    }

    public void say(){
        System.out.println("hello world");
    }
}

```

# 匿名内部类的使用
- 匿名内部类的语法比较奇特，因为匿名内部类既是一个类的定义，同时它本身也是一个对象，因此从语法上看，它既有定义类的特征，也有创建对象的特征，对前面代码分析可以看出这个特点，因此可以调用匿名内部类的方法。

```Java
public class z3_Detail {
    public static void main(String[] args) {
        //调用匿名内部类中的方法的两种方式
        //第一种
        new A(){
            @Override
            public void say(){
                System.out.println("方式1--匿名内部类---hello");
            }
        }.say();

        //第二种
        A a = new A() {
            @Override
            public void say(){
                System.out.println("方式2--匿名内部类---hello");
            }
        };
        a.say();
    }


}

class A{
    public void say(){
        System.out.println("A--hello");
    }
}

```

# 匿名内部类的应用
有一个铃声接口Bell，里面有个ring方法，有一个手机类Cellphone，具有闹钟功能alarmclock，参数是Bell类型，测试手机类的闹钟功能，通过匿名内部类（对象）作为参数，打印：“学习时间到了”，再传入另一个匿名内部类（对象），打印：下班了

```Java
public class z4_匿名内部类的应用 {
    public static void main(String[] args) {
        Cellphone cellphone = new Cellphone();
        cellphone.alarmclock(new Bell() {
            @Override
            public void ring() {
                System.out.println("学习时间到....");
            }
        });

        cellphone.alarmclock(new Bell() {
            @Override
            public void ring() {
                System.out.println("下班饿了.........");
            }
        });


    }
}

interface Bell{
    void ring() ;
}

class Cellphone{
    public void alarmclock(Bell bell){
        bell.ring();
    }
}

```

# 成员内部类的使用
说明：成员内部类是定义在外部类的成员位置，并且没有static修饰

- 可以直接访问外部类的所有成员，包含私有的

![image.png](https://pumpkn.xyz/upload/2021/09/image-97cb0fa3b57e49318c67f63e54c16fb0.png)

- 可以添加任意访问修饰符(public、protected、默认、private)，因为它的地位就是一个成员
- 作用域和外部类的其他成员一样，为整个类体，在外部类的成员方法中创建成员内部类对象，再调用方法。
- 成员内部类--->访问--->外部类（比如属性）【访问方式：直接访问】
- 外部类--->访问--->内部类【访问方式：创建对象，再访问】
- 外部其他类--->访问--->成员内部类

![image.png](https://pumpkn.xyz/upload/2021/09/image-8e2cbd3007384c7eb6ec03419feae035.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-e8019b086875446cb4753edab0cb8b07.png)

# 静态内部类
静态内部类是定义在外部类的成员位置，并且有static修饰

- 可以直接访问外部类的所有静态成员，包含私有的，但不能直接访问非静态成员
- 可以添加任意访问修饰符(public、protected、默认、private)，因为它的地位就是一个成员
- 作用域：同其他的成员，为整个类体
- 静态内部类--->访问--->外部类(比如：静态属性)【访问方式：直接访问所有静态成员】
- 外部类--->访问--->静态内部类【访问方式：创建对象，在访问】
- 外部其他类--->访问--->静态内部类


![image.png](https://pumpkn.xyz/upload/2021/09/image-c74b643b731143aba9c189b337a440e1.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-71e9f51366874c25963af9dbc1c42ed1.png)


![image.png](https://pumpkn.xyz/upload/2021/09/image-713a1638a04f48ad98524412193a11db.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-5beb3db6cedd44c6b5e953c077ae8b6e.png)

- 如果外部类和静态内部类的成员重名时，静态内部类访问的时候，默认遵循就近原则，如果想访问外部类的成员，则可以使用(外部类.成员)去访问

![image.png](https://pumpkn.xyz/upload/2021/09/image-66386b839009485fbdfff4cc7f9c0ff7.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-5a7c697f8cb14f2c9a0aa534f3619ce0.png)