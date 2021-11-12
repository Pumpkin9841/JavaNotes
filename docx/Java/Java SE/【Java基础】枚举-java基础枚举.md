---
title: 【Java基础】枚举
date: 2021-09-14 19:57:08.52
updated: 2021-09-14 20:08:03.38
url: https://pumpkn.xyz/archives/java基础枚举
categories: 
tags: 学习 | Java
---

# 枚举

- 枚举对应英文(enumeration，简写enum)
- 枚举是一组常量的集合
- 可以这样理解：枚举属于一种特殊的类，里面只包含一组有限的特定的对象

# 枚举的两种实现方式

- **自定义类实现枚举**
- **使用enum关键字实现枚举**

## **自定义类**实现枚举

- 不需要提供setXxx方法，因为枚举对象值通常为只读
- 对枚举对象/属性使用final + static共同修饰，实现底层优化
- 枚举对象名通常使用全部大写，常量的命名规范
- 枚举对象根据需要，也可以有多个属性
- 私有化构造器，防止外部直接new

```Java
public class Enum {
    public static void main(String[] args) {
        System.out.println(Season.SPRING);
        System.out.println(Season.SUMMER);
    }

}

class Season{
    private String name ;
    private String desc ;

    public static final Season SPRING = new Season("春天" , "温暖") ;
    public static final Season SUMMER = new Season("夏天" , "炎热") ;
    public static final Season AUTUMN = new Season("秋天" , "凉爽") ;
    public static final Season WINTER = new Season("冬天" , "寒冷") ;

    //1.私有化构造器，防止直接new
    //2.不要setXxx方法，防止属性被更改
    //3.在Season内部，直接创建固定对象
    private Season(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }

    public String getName() {
        return name;
    }

    public String getDesc() {
        return desc;
    }

    @Override
    public String toString() {
        return "Season{" +
                "name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                '}';
    }
}

```

## 小结
自定义类实现枚举，有如下特点：

- 构造器私有化
- 本类内部创建一组对象
- 对外暴露对象（通过为对象添加public final static修饰）
- 可以提供get方法，但不要提供set


## **enum关键字**实现枚举

使用enum来实现前面的枚举案例。

```Java
public class Enum02 {
    public static void main(String[] args) {
        System.out.println(Season2.SPRING);
    }
}

enum Season2{

    //如果使用enum关键字来实现枚举类
    //1.使用关键字 enum 代替 class
    //2.用SPRING("春天" , "温暖")代替 public static final Season SPRING = new Season("春天" , "温暖") ;
    //3.语法格式：常量名(实参列表)
    //4.如果有多个常量(对象) 使用逗号隔开，最后一个用分号
    //5.如果使用enum关键字来实现枚举，要求将定义常量(对象)写在最前面
    SPRING("春天" , "温暖"),
    SUMMER("夏天" , "炎热"),
    AUTUMN("秋天" , "凉爽"),
    WINTER("冬天" , "寒冷");

    private String name ;
    private String desc ;

    private Season2(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }

    public String getName() {
        return name;
    }

    public String getDesc() {
        return desc;
    }

    @Override
    public String toString() {
        return "Season2{" +
                "name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                '}';
    }
}

```

## enum关键字实现枚举注意事项

- 当我们使用enum关键字开发一个枚举类时，默认会继承Enum类

- 传统的public static final Season2 SPRING = new Season2("春天", "温暖");简化成SPRING("春天","温暖")，这里必须知道，他调用的是哪个构造器。
- 如果使用无参构造器创建枚举对象，则实参列表和小括号都可以省略
- 当有多个枚举对象时"，"使用,间隔，最后用一个";"结尾
- 枚举对象必须放在枚举类的行首


# enum常用方法
使用关键字enum时，会隐式继承Enum类，这样我们就可以使用Enum类相关的方法。

![image.png](https://pumpkn.xyz/upload/2021/09/image-be71515899934dcf994f7edf8d50abbb.png)

- ```toString```: Enum类已经重写过了，返回的是当前对象名，子类可以重写该方法，用于返回对象的属性信息

- ```name```: 返回当前对象名（常量名），子类中不能重写
- ```ordinal``` : 返回当前对象的位置号，默认从0开始
- ```values```: 返回当前枚举类中的所有常量
- ```valeOf```: 将字符串转换成枚举对象，要求字符串必须为已有的常量名，否则报异常！
- ```compareTo```: 比较两个枚举常量，比较的就是位置号


```Java

public class EnumMethod {
    public static void main(String[] args) {
        //使用Season2 枚举类演示各种方法
        Season2 autumn = Season2.AUTUMN;
        //name(): 输出枚举对象的名字
        System.out.println(autumn.name());  //输出 AUTUMN

        //ordinal(): 输出的是该枚举对象的次序/编号，从0开始
        //AUTUMN在Season2中是第三个枚举对象
        System.out.println(autumn.ordinal()); //输出 2

        //values(): 返回含有所有枚举对象的数组
        /* 输出
        * Season2{name='春天', desc='温暖'}
          Season2{name='夏天', desc='炎热'}
          Season2{name='秋天', desc='凉爽'}
          Season2{name='冬天', desc='寒冷'}
        * */
        Season2[] values = Season2.values();
        for (Season2 season : values) {
            System.out.println(season);
        }

        //valueOf: 将字符串转换成枚举对象，要求字符串必须为已有的常量名，否则报错
        //执行流程：
        //1.根据输入的“AUTUMN”到Season2中寻找
        //2.如果找到，就返回，没有找到，就报错
        Season2 autumn1 = Season2.valueOf("AUTUMN");
        System.out.println(autumn1); // 输出 Season2{name='秋天', desc='凉爽'}
        //注意: autumn1 与 autumn 是同一个对象

    }
}


```

# 示例

声明Week枚举类，其中包含星期一至星期日的定义,使用values返回所有枚举组并遍历，输出左图效果

```Java
public enum Week {

    MONDAY("星期一") ,
    TUESDAY("星期二") ,
    WEDNESDAY("星期三"),
    THURSDAY("星期四"),
    FRIDAY("星期五") ,
    SATURDAY("星期六") ,
    SUNDAY("星期天") ;

    private String dayTime ;
    private Week(String dayTime){
        this.dayTime = dayTime ;
    }

    @Override
    public String toString() {
        return dayTime;
    }
}


public class EnumTest {
    public static void main(String[] args) {
        Week[] values = Week.values();
        for (Week week : values) {
            System.out.println(week);
        }
    }
}
```

# 注意事项

- 使用enum关键字后，就不能再继承其他类了，因为enum会隐式继承Enum，而Java是单继承机制
- 枚举类和普通类一样，可以实现接口。