---
title: 【JavaWeb】xml
date: 2021-09-28 09:53:20.768
updated: 2021-09-28 09:53:20.768
url: https://pumpkn.xyz/archives/javawebxml
categories: 
tags: 学习 | Java | JavaWeb
---

# xml简介

## 什么是xml
xml是可扩展的标记性语言

## xml的作用

- 用来保存数据，而且这些数据具有自我描述性
- 它还可以作为项目或者模块的配置文件
- 他可以作为网络传输数据的格式（现在JSON为主）

## xml语法

- 文档声明
- 元素（标签）
- xml属性
- xml注释
- 文本区域（CDATA区）

## 文档声明
创建xml文件
![image.png](https://pumpkn.xyz/upload/2021/09/image-cf92da24fe03478485138b46c95e75bd.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-90039e184c8b4b73831a5efb8d861b3a.png)

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!--
    <?xml version="1.0" encoding="utf-8" ?>
    以上内容是xml文件的声明
    version="1.0"     version表示xml版本
    encoding="utf-8"   encoding表示xml文件本身的编码
-->
<books>
    <book sn="sn1234456789">
        <name>福尔摩斯探案集</name>
        <author>柯南·道尔</author>
    </book>
    <book sn="sn213546878">
        <name>云边有个小卖部</name>
        <author>张嘉佳</author>
    </book>
</books>
```

## 元素（标签）

### 什么是xml元素
xml元素指的是从（包括）开始标签到（包括）结束标签的部分。</br>
元素可包含其他元素、文本或者两者的混合物。元素也可以拥有属性</br>
**元素可以简单的理解为是标签,element**

### xml命名规则
xml 元素必须遵循一下命名规则：

- 名称可以含字母、数字以及其他的字符
- 名称不能以数字或者标点符号开始
- 名称不能包含空格

### xml中的元素（标签）也分成单标签和双标签

- 单标签

	- ```<标签名 属性="值"... />```
- 双标签

	- ```<标签名 属性="值"...>文本或子标签 </标签名>```

## xml属性
xml的标签属性和html的标签属性非常类似，**属性可以提供元素的额外信息**</br>
在标签上可以书写属性

- 一个标签上可以写多个属性
- **每个属性的值必须使用引号引起来**

![image.png](https://pumpkn.xyz/upload/2021/09/image-b8dad4ebb03b423da2e522c84a302d95.png)


## 语法规则

- 所有xml都需要闭合
- xml标签对大小写敏感
- xml必须正确的嵌套
- **xml文档必须有根元素**

	- 根元素就是顶级元素
	- 没有父标签的元素，叫顶级元素
	- 根元素是没有父标签的顶级元素，而且是唯一一个才行

![image.png](https://pumpkn.xyz/upload/2021/09/image-2eb18da79a5448e0943a90665437d148.png)


# xml解析技术介绍
xml是可扩展语言，不管是html还是xml他们都是标记型文档，都可以使用w3c组织指定的dom技术来解析。
![image.png](https://pumpkn.xyz/upload/2021/09/image-c7033128e91b41d6b17956023067b5f7.png)

早期 JDK 为我们提供了两种 xml 解析技术 DOM 和 Sax 简介（已经过时，但我们需要知道这两种技术）</br>
dom 解析技术是 W3C 组织制定的，而所有的编程语言都对这个解析技术使用了自己语言的特点进行实现。</br>
 Java 对 dom 技术解析标记也做了实现。 sun 公司在 JDK5 版本对 dom 解析技术进行升级：SAX（ Simple API for XML ） </br>
SAX 解析，它跟 W3C 制定的解析不太一样。它是以类似事件机制通过回调告诉用户当前正在解析的内容。 它是一行一行的读取 xml 文件进行解析的。不会创建大量的 dom 对象。</br>
 所以它在解析 xml 的时候，在内存的使用和性能上，都优于 Dom 解析。

</br>
第三方解析：
- jdom在dom基础上进行了封装
- dom4j又对jdom进行了封装
- pull主要在android手机开发，是跟sal非常类似，都是事件机制解析xml文件

# dom4j解析技术
加载dom4j类库

```java
public class Dom4jTest {
    @Test
    public void test() throws DocumentException {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read("src/book.xml");
        Element rootElement = document.getRootElement();
        System.out.println(rootElement);
        List<Element> books = rootElement.elements();
        for (Element book : books) {
            String sn = book.attributeValue("sn");
            String name = book.elementText("name");
            String author = book.elementText("author");
            System.out.println(new Books(sn , name , author));
        }

    }
}

```

```xml
<books>
    <book sn="sn1234456789">
        <name>福尔摩斯探案集</name>
        <author>柯南·道尔</author>
    </book>
    <book sn="sn213546878">
        <name>云边有个小卖部</name>
        <author>张嘉佳</author>
    </book>
</books>
```

![image.png](https://pumpkn.xyz/upload/2021/09/image-cdc1206894374a268c758b1b38ef3ad0.png)