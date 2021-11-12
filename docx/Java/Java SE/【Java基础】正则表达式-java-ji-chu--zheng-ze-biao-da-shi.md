---
title: 【Java基础】正则表达式
date: 2021-09-20 13:47:34.223
updated: 2021-09-20 13:47:34.223
url: https://pumpkn.xyz/archives/java-ji-chu--zheng-ze-biao-da-shi
categories: 
tags: 学习 | Java
---

# 正则表达式

## 介绍

- 一个正则表达式，就是用某种模式去匹配字符串的一个公式。
- 正则表达式不是Java才有的，实际上很多编程语言都支持正则表达式进行字符串操作！

## 示例

```Java
public class 正则表达式1 {
    public static void main(String[] args) {
        String content = "Java是1998Java是1998Java是1998Java是1998Java是1998Java是1998Java是1998Java是1998Java是1998Java是1998" ;
        //目标  匹配所有4个数字
        //1. \\d表示任一个数字
        String regStr = "\\d\\d\\d\\d" ;
        //2. 创建模式对象
        Pattern pattern = Pattern.compile(regStr);
        //3. 创建匹配器
        //说明 创建匹配器matcher，按照正则表达式的规则去匹配content字符串
        Matcher matcher = pattern.matcher(content);
        //4. 开始匹配
        /**
         * matcher.find()完成的任务（考虑分组）
         * 1. 根据指定的规则，定位满足规则的子字符串(比如1998)
         * 2. 找到后，将子字符串的开始的索引记录到matcher对象的属性int[] groups;
         *    2.1 groups[0] = 0 ，把该子字符串的结束的索引+1的值记录到groups[1] = 4
         *    2.2 记录1组()匹配到的字符串 groups[2]=0 groups[3]=2
         *    2.3 记录2组()匹配到的字符串 groups[4]=2 groups[5]=4
         *    2.4 如果有更多的分组....
         * 3. 同时记录oldLast的值为子字符串的结束的索引+1的值即4，即下次执行find时，就从4开始匹配
         * */
        while( matcher.find() ){
            //如果正则表达式有()，即分组
            //取出匹配的字符串规则如下
            //group[0]表示匹配到的子字符串
            //group[1]表示匹配到的子字符串的第一组字串
            //group[2]表示匹配到的子字符串的第2组字串
            //... 但是分组的数不能越界
            System.out.println("找到 " + matcher.group(0));
        }
    }
}
```

如果想要灵活的运用正则表达式，必须了解其中各种元字符的功能，元字符从功能上大致分为：

- 限定符
- 选择匹配符
- 分组组合和反向引用符
- 特殊字符
- 字符匹配符
- 定位符

# 正则表达式语法

## 元字符——转义号```\\```

```\\```符号：在我们使用正则表达式去检索某些特殊字符的时候，需要用到转义符号，否则检索不到结果，甚至会报错。

**提醒，在Java的正则表达式中，两个```\\```代表其他语言中的```\```**

## 元字符——字符匹配符
![image.png](https://pumpkn.xyz/upload/2021/09/image-d4e6b9a627e645b294a9e063c3944d48.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-75db9e2c4c144204a0f4a2da2981176d.png)


## Java正则表达式默认是区分字母大小写的，如何实现不区分大小写？

- ```(?i)abc```表示abc都不区分大小写
- ```a(?i)bc```表示bc不分区大小写
- ```Pattern compile = Pattern.compile(regStr, Pattern.CASE_INSENSITIVE);```

![image.png](https://pumpkn.xyz/upload/2021/09/image-72394db7563146b9be22cf5d9c3fd6d6.png)

## 元字符——选择匹配符

在匹配某个字符串的时候是选择性的，即：既可以匹配这个，又可以匹配那个。

![image.png](https://pumpkn.xyz/upload/2021/09/image-1ed5d0e3507e453a98d6ca3fc1a5ca98.png)

## 元字符——限定符
用于指定其前面的字符和组合项连续出现多少次

![image.png](https://pumpkn.xyz/upload/2021/09/image-eed8c02508f3404ab6e925e7ef34b207.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-8a9a5145f28f41c6ab5fd6edbc18035b.png)

<font color="red">Java匹配默认贪婪匹配，即尽可能多的匹配</font>，例如
```Java
String regStr = "a{3,4}" ; //表示匹配aaa或者aaaa，但是优先匹配aaaa
```

## 元字符——定位符
定位符，规定要匹配的字符串出现的位置，比如在字符串的开始还是在结束的位置
![image.png](https://pumpkn.xyz/upload/2021/09/image-680006fbac80430ea19c6d1205ddb28d.png)

# 分组
![image.png](https://pumpkn.xyz/upload/2021/09/image-6f70b5ccb3e94846b456f51ef95a5ec5.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-0b3b2ef5697d49389ca7dcbb46b216be.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-757c00bf61f64b78a69c59190fcf429a.png)

# 非贪婪匹配

```?```: 当此字符紧随任何其他限定符(*、+、?、{n}、{n,}、{n.m})之后时，匹配模式是“非贪婪的”

# 正则表达式三个常用类

## ```Pattern```类
```Pattern```对象是一个正则表达式对象。```Pattern```类没有公共构造方法。要创建一个```Pattern```对象，调用其公共静态方法，它返回一个```Pattern```对象。该方法接受一个正则表达式作为它的第一个参数，例如```Pattern r = Pattern.compile(pattern)```

## ```Matcher类```
```Matcher类```对象是对输入字符串进行解释和匹配的引擎。与Pattern一样，```Matcher类```没有公共构造方法。需要调用Pattetn对象的matcher方法来获得要给Matcher对象

## ```PatternSyntaxException```
是一个非强制异常类，他表示一个正则表达式模式中的语法错误

```Java
public class 正则表达式2 {
    public static void main(String[] args) {
        String content = "hello" ;
        String regStr = "\\w{6}" ;
        boolean matches = Pattern.matches(regStr, content);
        System.out.println(matches);
    }
}

```


# 分组、捕获、反向引用

## 分组
我们可以用圆括号组成一个比较复杂的匹配模式，那么一个圆括号的部分我们可以看作是一个子表达式/一个分组

## 捕获
把正则表达式中子表达式/分组部分的内容，保存到内存中以数字编号或显示命名的组里，方便后面引用。从左向右，以分组的括号为标志，第一个出现的分组的组号为1，第二个为2，依次类推。组0代表整个正则式

## 反向引用
圆括号的内容被捕获后，可以在这个括号后被使用，从而写出一个比较使用的匹配模式，这个称为反向引用，这种引用既可以是在正则表达式内部，也可以是在正则表达式外部，内部反向引用```\\分组号```，外部反向引用```$分组号```

## 案例

- 匹配两个连续的相同的数字: ```(\\d)\\1```
- 匹配五个连续的相同的数字: ```(\\d)\\1{4}```
- 匹配各位与千位相同，十位与百位相同的数如5225，1551: ```(\\d)(\\d)\\2\\1```