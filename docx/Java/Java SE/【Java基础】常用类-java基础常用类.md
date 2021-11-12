---
title: 【Java基础】常用类
date: 2021-09-15 19:55:42.977
updated: 2021-09-16 15:29:03.523
url: https://pumpkn.xyz/archives/java基础常用类
categories: 
tags: 学习 | Java
---

# 包装类

- 针对8种基本数据类型相应的引用类型——包装类
- 有了类的特点，就可以调用类中的方法

|基本数据类型|包装类|
|-------|-------|
|boolean|Boolean|
|char|Character|
|byte|Byte|
|short|Short|
|int|Integer|
|long|Long|
|float|Float|
|double|Double|

## 各包装类的继承关系

![4.jpg](https://pumpkn.xyz/upload/2021/09/4-152b74c6762b4d10a14eddd804a8d306.jpg)

![3.jpg](https://pumpkn.xyz/upload/2021/09/3-82579fa113084d8e9f7c1766be721cb2.jpg)

![2.jpg](https://pumpkn.xyz/upload/2021/09/2-6d24c0fe054c4e5dba3a8b5d32183ebc.jpg)


## 包装类和基本数据的转换

### 演示 **包装类** 和**基本数据类型**的相互转换，这里以```int```和```Integer```演示。

- JDK5前的手动装箱和拆箱方式，装箱：基本类型->包装类型，反之，为拆箱
- JDK5以后（含JDK5），自动装箱和自动拆箱
- 自动装箱底层调用的是valueOf()方法。比如```Integer.valueOf(n)```

```Java
public class z1_自动手动装箱 {
    public static void main(String[] args) {
        int n = 99 ;
        //JDK5以前 手动装箱
        //int -> Integer
        Integer integer = new Integer(n);
        Integer integer1 = Integer.valueOf(n);

        //手动拆箱
        int i = integer.intValue();
        System.out.println(i);

        //JDK5以后 自动装箱
        Integer integer2 = 99 ;
        //自动拆箱
        int i2 = integer2 ;

        //其他包装类的用法类似
    }

}

```

### 包装类型和String类型的相互转换

以```Integer```与```String```转换为例

```Java
public class z2_String与包装类的转换 {
    public static void main(String[] args) {
        //Integer -> String
        Integer integer  = 99 ;

        //方式一
        String s = integer + "";
        //方式二
        String s1 = integer.toString();
        //方式三
        String s2 = String.valueOf(integer);

        //String -> Integer
        String str = "123456" ;
        //方式一
        int i = Integer.parseInt(str);
        //方式二
        Integer integer1 = new Integer(str);
    }
}

```


### Integer相关问题

```Java
public class z3_Integer类面试题 {
    public static void main(String[] args) {
        //一
        Integer integer = new Integer(127);
        Integer integer1 = new Integer(127);
        System.out.println(integer == integer1); //false

        //二
        Integer integer2 = new Integer(128);
        Integer integer3 = new Integer(128);
        System.out.println(integer2 == integer3); //false

        //三
        Integer integer4 = 127 ;
        Integer integer5 = 127 ;
        System.out.println( integer4 == integer5 ); //true 具体看Integer.valueOf源码

        //四
        Integer integer6 = 128 ;
        Integer integer7 = 128 ;
        System.out.println(integer6 == integer7); //false

        //五
        Integer integer8 = 127 ;
        Integer integer9 = new Integer(127);
        System.out.println(integer8 == integer9); //false

        //六
        Integer integer10 = 127 ;
        int i11 = 127 ;
        //只要有基本类型的数据，== 判断的都是指是否相同
        System.out.println( integer10 == i11 ); //true

        //七
        Integer i13 = 128 ;
        int i14 = 128 ;
        System.out.println( i13 == i14 ); //true



    }
}

```


# ```String```类

String类继承关系
![10.jpg](https://pumpkn.xyz/upload/2021/09/10-214985becb9d46e3870d01c539a086db.jpg)

## ```String```类的理解和创建对象

- String对象用于保存字符串，也就是一组字符序列
- 字符串常量对象是用双引号括起来的字符序列。
- 字符串的字符使用```Unicode```字符编码，一个字符（不区分字母还是汉字）占两个字节
- String类较常用构造方法
- String是final类，不能被其他类继承
- String有属性private final char value[] ;用于存放字符串内容
- 一定要注意：value是一个final类型，不可以修改：即value不能指向新的地址，但是value[]中的字符内容是可以修改的。

```Java
String s1 = new String() ;
String s2 = new String(String original) ;
String s3 = new String(char[] a) ;
String s4 = new String(char[] a , int start , int count) ;
```

## 两种创建```String```对象的区别

- **方式一**： 直接赋值String s = "zf" ;

	- 方式一：先从常量池查看是否有"zf"数据空间，如果有，直接指向；如果没有，则重新创建，然后指向。s最终指向的是**常量池**的空间地址。

- **方式二**：调用构造器String s2 = new String("zf") ;

	- 先在堆中创建空间，里面维护了value属性，指向常量池的zf空间，如果常量池没有"zf"，则创建，如果有，直接通过value指向。最终指向的是**堆**中的空间地址

![image.png](https://pumpkn.xyz/upload/2021/09/image-adfac068765a4fd4ac08a171045b34d1.png)

## 思考题1

```Java
public class String思考题 {
    public static void main(String[] args) {
        Person person = new Person();
        person.name = "zf" ;
        Person person1 = new Person();
        person1.name = "zf" ;

        System.out.println(person.name.equals(person1.name)); //true
        System.out.println(person.name == person1.name); //true
        System.out.println(person.name == "zf"); //true

        String s1 = new String("a") ;
        String s2 = new String("a") ;
        System.out.println(s1 == s2); //false
    }
}

```

## 字符串特性

- String是一个final类，代表不可变的字符序列
- 字符串是不可变的。一个字符串对象一旦被分配，其内容是不可变的。

```Java
 //以下语句创建几个对象？画出内存布局图
String s1 = "hello" ;
s1 = "haha" ;
```

![image.png](https://pumpkn.xyz/upload/2021/09/image-c45e04b1113a430aa8cd80b1c797a265.png)


## 思考题2
```Java
String a = "hello" ; //创建a对象
String b = "abc" ; //创建b对象
String c = a + b ; //创建几个对象？画出内存图？
//关键就是要分析String c = a + b ;到底是如何执行的?
```

一共有三个对象，内存图如下

![image.png](https://pumpkn.xyz/upload/2021/09/image-ca44be88994b4910ad8c290a0cb59f39.png)


## 思考题2小结

底层是
```Java
StringBuilder sb = new StringBuilder(); sb.append(a) ; 
sb.append(b) ;
```
sb是在堆中，并且append实在原来的字符串的基础上追加的。
</br>
**重要规则，String c1 = "ab" + "cd"  ; 常量相加，看的是池。String c1 = a +b ;变量相加，是在堆中**


## 思考题3

```Java
@SuppressWarnings({"all"})
public class 思考题3 {
    String str = new String("zf") ;
    final char[] ch = {'j' , 'a' , 'v' , 'a'} ;

    public void change( String str , char ch[] ){
        str = "java" ;
        ch[0] = 'h' ;
    }

    public static void main(String[] args) {
        思考题3 thought = new 思考题3();
        thought.change(thought.str , thought.ch);
        System.out.println(thought.str + " and ");
        System.out.println(thought.ch);
    }
}
```

输出结果
![image.png](https://pumpkn.xyz/upload/2021/09/image-7597bf2ffb54494ea035aa444a19155a.png)

其内存图如下

![image.png](https://pumpkn.xyz/upload/2021/09/image-6fa6fc401b2c4db190f06aa79a6790f9.png)


# ```String```类的常见方法

## 说明
String类是保存字符串常量的。每次更新都需要重新开辟空间，效率极低，因此Java设计者提供了'''StringBuilder```和```StringBuffer```来增强String的功能，并提高效率。

## String类的常见方法

- ```equals```： 区分大小写，判断内容是否相等
- ```equalsIgnoreCase```: 忽略大小写判断内容是否相等
- ```length```：获取字符的个数，字符串的长度
- ```indexOf```: 获取字符在字符串中第1次出现的索引，索引从0开始，如果找不到，返回-1
- ```lastIndexOf```: 获取字符在字符串中最后1次出现的索引，索引从0开始，如果找不到，返回-1
- ```substring```: 截取指定范围的字串
- ```trim```: 去前后的空格
- ```charAt```: 获取某索引处的字符，注意不能使用Str[index]这种方式。

	- ```Java
		String str = "hello" ;
		//str[0] 不对
	        str.charAt(0) //获取h
	  ```

- ```toUpperCase```: 字母全部转成大写
- ```toLowerCase```: 字母全部转成小写
- ```concat```: 返回拼接的字符串。
- ```replace```: 替换字符串中的字符
- ```split```: 分割字符串，对于某些分割字符，需要转义 比如```|\\等```
- ```compareTo```: 比较两个字符串的大小
- ```toCharArray```: 转换成字符数组
- ```format```: 格式字符串,```%s```字符串，```%c```字符，```%d```整型，```%0.2f%浮点型

# ```StringBuffer```类

## 基本介绍

```java.lang.StringBuffer```代表**可变的字符序列**，可以对字符串内容进行增删。很多方法与String相同，但StringBuffer是可变长度的。**StringBuffer是一个容器**

## ```String``` VS ```StringBuffer```

- String保存的是字符串常量，里面的值不能更改，每次String类的更新实际上就是更改地址，效率极低。
- StringBuffer保存的是字符串变量，里面的值可以更改，每次StringBuffer的更新实际上可以更新内容，不用每次更新地址，效率较高。

```Java
@SuppressWarnings({"all"})
public class String_StringBuffer {
    public static void main(String[] args) {

        //String-->StringBuffer
        String str = "hello zf" ;
        //方式一
        StringBuffer stringBuffer = new StringBuffer(str);
        //方式二
        StringBuffer stringBuffer1 = new StringBuffer();
        stringBuffer1 = stringBuffer1.append(str);
        System.out.println(stringBuffer1);


        //StringBuffer-->String
        StringBuffer stringBuffer2 = new StringBuffer("zf");
        //方式一
        String string = stringBuffer2.toString();

        //方式二
        String s = new String(stringBuffer2);
        System.out.println(s);
    }
}
```

## StringBuffer类常见方法

- ```append``` ： 增
- ```delete(start , end)``` : 删
- ```replace(start , end , string)``` : 将start--end间的内容替换掉，不含end
- ```indexOf```: 查找子串在字符串第1次出现的索引，如果找不到返回-1
- ```insert```: 插
- ```length```： 获取长度


# StringBuilder类

## 基本介绍

- 一个可变的字符序列，此类提供一个与```StringBuffer```兼容的API，**但不保证同步(StringBuilder不是线程安全的)**。该类被设计用作StringBuffer的一个简易替换。**用在字符串缓冲区被单个线程使用的时候**。如果可能，建议优先采用该类，因此在大多数实现中，他比StringBuffer要快
- 在StringBuilder上的主要操作是append和insert方法，可重载这些方法以接受任意类型的数据。
- ```StringBuilder```继承```AbstractStringBuilder```类
- 实现了```Serializable```，说明```StringBuilder```对象可以序列化（对象可以网络传输，可以保存到文件）
- ```StringBuilder```是final类，不能被继承
- ```StringBuilder```对象字符序列仍然是存放在父类```AbstractStringBuilder```类中的```char[] value```，因此字符序列在堆中
- ```StringBuilder```的方法，没有做互斥的处理，即没有```synchronized```关键字，因此在单线程的情况下使用

# ```String```、```StringBuffer```、```StringBuilder```比较


- ```StringBuilder```和```StringBuffer```非常类似，均代表可变的字符序列，而且方法也一样
- ```String```: 不可变字符序列，效率极低，但是复用率高。
- ```StringBuffer```: 可变字符序列，效率较高(增删)、线程安全。
- ```StringBuilder```: 可变字符序列，效率最高，线程不安全

# 使用原则、结论

- 如果字符串存在大量的修改操作，一般使用```StringBuffer```或```StringBuilder```
- 如果字符串存在大量的修改操作，并在单线程的情况，使用```StringBuilder```
- 如果字符串存在大量的修改操作，并在多线程的情况，使用```StringBuffer```
- 如果字符串很少修改，被很多对象引用，使用String，比如配置信息等


# Math类

- abs  绝对值
- pow 求幂
- ceil 向上取整
- floor 向下取整
- round 四舍五入
- sqrt 开方
- random 随机数
	
	- [a-b]的随机数</br>

```Java
 int num = (int)(Math.random()*(b-a+1) + a) ;
```

- max 求两个数的最大值
- min 求两个数的最小值
 
# System类

- exit 推出当前程序
- arraycopu 复制数组元素，比较适合底层调用，一般使用Arrays.copyOf完成复制数组
- currentTimeMillens 返回当前时间距离1970-1-1的毫秒数
- gc 运行垃圾回收机制


# 日期类

## 第一代日期类

- ```Date``` : 精确到毫秒数，代表特定的瞬间（默认实在java.util包）
- ```SimpleDateFormat``` 格式化和解析日期的具体类。它允许进行格式化（日期->文本）、解析（文本->日期）和规范化。

![image.png](https://pumpkn.xyz/upload/2021/09/image-04a9c18c8fd74185b1df4ed3c1885eee.png)

```Java
public class 第一代日期类 {
    public static void main(String[] args) throws ParseException {
        Date date = new Date();
        //创建SimpleDateFormat对象，可以制定相应的格式

        SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 hh:mm:ss E") ;
        //date->String
        String format = sdf.format(date);
        System.out.println(format);

        //String->date
        String s = "2021年09月16日 02:44:24 周四" ;
        Date parse = sdf.parse(s) ;
        System.out.println(sdf.format(parse));
    }
}
```

## 第二代日期类

- 第二代日期类，主要就是```Calendar```类(日历)
- ```Calendar```类是一个抽象类，它为特定瞬间与一组诸如YEAR、MONTH、DAY_OF_MONTH、HOUR等日历字段之间的转换提供了一些方法，并为操作日历字段（例如获得下星期的日期）提供了一些方法。

- ```Calendar```是一个抽象类，并且构造器是private
- 可以通过```getInstance()```来获取实例


```Java
public class 第二代日期类 {
    public static void main(String[] args) {
        //Calendar是一个抽象类，并且构造器是private
        Calendar calendar = Calendar.getInstance(); //获取日历对象
        System.out.println(calendar);

        //获取日历类对象的某个日历字段
        System.out.println("年 " + calendar.get(Calendar.YEAR));
        //这里为什么要 + 1 ，因为Calendar返回月的时候，是按照0开始编号的
        System.out.println("月 " + (calendar.get(Calendar.MONTH)+1) );
        System.out.println("日 " + calendar.get(Calendar.DAY_OF_MONTH) );
        //如果我们需要按照24小时进制来获取时间，Calendar.HOUR == 改成 ==> Calendar.HOUR_OF_DAY
        System.out.println("时 " + calendar.get(Calendar.HOUR) );
        System.out.println("分 " + calendar.get(Calendar.MINUTE) );
        System.out.println("秒 " + calendar.get(Calendar.SECOND) );

        //Calendar没有专门的格式化方法，所以需要程序员自己来组合显示
    }
}
```

## 第三代日期类

### 前面两代日期类的不足分析
JDK1.0中包含了一个```Java.util.Date```类，但是它的大多数方法已经在JDK1.1引入```Calendar```类之后被弃用了。而```Calendar```也存在的问题是：

- 可变性：像日期和时间这样的类应该是不可变的
- 偏移性：Date中的年份是从1900开始的，二月份都从0开始
- 格式化：格式化只对Date有用，Calendar则不行
- 此外，他们也不是线程安全的；不能处理闰秒等


LocalDate(日期/年月日)、LocalTime（时间/时分秒）、LocalDateTime(日期时间/年月日时分秒),JDK8加入


- LocalDate只包含日期，可以获取日期字段
- LocalTime只包含时间，可以获取时间字段
- LocalDateTime包含日期+时间，可以获取日期和时间字段

```Java
public class 第三代日期类 {
    public static void main(String[] args) {
        LocalDateTime now = LocalDateTime.now();
        LocalDate localDate = LocalDate.now();
        LocalTime localTime = LocalTime.now();
        System.out.println(now);
        System.out.println(localDate);
        System.out.println(localTime);

        System.out.println("================================");
        System.out.println("年 " + now.getYear());
        System.out.println("月 " + now.getMonth());
        System.out.println("日 " + now.getDayOfMonth());
        //星期
        System.out.println("日 " + now.getDayOfWeek());
        //当前是一年的第多少天
        System.out.println("日 " + now.getDayOfYear());
        System.out.println("时 " + now.getHour());
        System.out.println("分 " + now.getMinute());
        System.out.println("秒 " + now.getSecond());

    }
}

```


## ```DateTimeFormatter```格式日期类
类似于SimpleDateFormat

```Java
        LocalDateTime localDateTime = LocalDateTime.now();
        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy年MM月dd日 hh:mm:ss");
        String format = dtf.format(localDateTime);
        System.out.println(format);  //2021年09月16日 03:23:05

```

## 第三代日期类更多的方法

- LocalDateTime类
- MonthDay类：检查重复事件
- 是否时闰年
- 增加日期的某个部分
- 使用plus方法测试增加时间的某个部分
- 使用minus方法测试查看一年前和一年后的日期

890天后是什么时候
```Java
        LocalDateTime localDateTime = LocalDateTime.now();
        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy年MM月dd日 hh:mm:ss");
        String format = dtf.format(localDateTime);
        System.out.println(format);  //2021年09月16日 03:23:05

        LocalDateTime localDateTime1 = localDateTime.plusDays(890);
        System.out.println(dtf.format(localDateTime1));
```

