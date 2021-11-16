---
title: Spring5
date: 2021-10-10 17:03:57.39
updated: 2021-10-22 00:03:39.56
url: https://pumpkn.xyz/archives/spring5
categories: 
tags: 学习 | Java | Spring5
---

# Spring5框架介绍

- [Spring框架概述](#1)
- [IOC容器](#2)

	- [IOC底层原理](#3)
	- [IOC接口(BeanFactory)](#4)
	- [IOC操作Bean管理(基于xml)](#5)
	- [IOC操作Bean管理(基于注解)](#6)

- [AOP](#7)
- [JdbcTemplate](#8)
- [事务管理](#9)
- [Spring5新特性(TODO)](#10)

# <span id = "1">Spring5框架概述 </span>

- Spring是轻量级的开源的JavaEE框架
- Spring可以解决企业应用开发的复杂性
- Spring有两个核心部分： ```IOC```和```AOP```

	- ```IOC```: 控制反转，把创建对象过程交给Spring进行管理
	- ```AOP```: 面向切面，不修改源码进行功能增强

- Spring特点

	- 方便解耦，简化开发
	- AOP编程
	- 方便程序测试
	- 方便和其他框架进行整合
	- 方便进行事务操作
	- 降低API开发难度

## 快速体验Spring5
### [Spring包下载](https://repo.spring.io/ui/native/release/org/springframework/spring/)

下载解压
![image.png](https://pumpkn.xyz/upload/2021/10/image-23fd6ca1c67f4734a872d367282d91a5.png)

导入Spring5相关jar包
![image.png](https://pumpkn.xyz/upload/2021/10/image-26a1e4af2fb8429496b511e9dd6c52bd.png)

 ![image.png](https://pumpkn.xyz/upload/2021/10/image-4fa3f30840ec4ee99d257f1d56a7a1c3.png)

创建普通类，在这个类中创建普通方法

```Java
public class User {
 	public void add() {
 		System.out.println("add......");
 	} 
}
```

创建Spring配置文件，在配置文件中配置创建的对象</br>
Spring配置文件使用```xml```格式
![image.png](https://pumpkn.xyz/upload/2021/10/image-f16e622efb5e4bb88b61f7029c129243.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" 
 	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
 	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 	<!--配置 User 对象创建--> 
 	<bean id="user" class="com.atguigu.spring5.User"></bean>
</beans>
```

进行测试
```Java
@Test
public void testAdd() {
 	//1 加载 spring 配置文件
 	ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
 	//2 获取配置创建的对象
 	User user = context.getBean("user", User.class);
 	System.out.println(user);
 	user.add();
}
```

# <span id = "2">IOC容器 </span>

- **什么是IOC**

	- 控制反转，把对象创建和对象之间的调用过程，交给Spring进行管理
	- 使用IOC目的：为了降低耦合度
	- 上述快速体验Spring案例就是IOC的实现


- **IOC底层原理**

	- xml解析
	- 工厂模式
	- 反射

![image.png](https://pumpkn.xyz/upload/2021/10/image-c8c4fbb2a7414c1884cec61c4d19e74f.png)


## <span id = "3">IOC接口(BeanFactory) </span>

- IOC思想基于IOC容器完成，IOC容器底层就是对象工厂
- Spring提供IOC容器实现两种方式：(两个接口)

	- ```BeanFactory``` IOC容器基本实现，是Spring内部的使用接口，不提供开发人员进行使用。<font color="red">加载配置文件时候不会创建对象，在获取对象(使用)才去创建对象 </font>
	- ```ApplicationContext``` BeanFactory接口的子接口，提供更多更强大的功能，一般由开发人员进行使用。<font color="red">加载配置文件时候就会把配置文件对象进行创建 </font>


- ApplicationContext接口有实现类

![image.png](https://pumpkn.xyz/upload/2021/10/image-a9865b4110104d67a844e67e64338939.png)


# IOC操作Bean管理

- 什么是Bean管理

	- Bean管理指的是两个操作
	
		- Spring创建对象
		- Spring注入属性

- Bean管理操作有两种方式

	- 基于```xml```配置文件方式实现
	- 基于注解方式实现


## <span id = "5">IOC操作Bean管理(基于xml方式) </span>

###  基于xml方式创建对象

```xml
<bean id="userDaoImpl" class="com.zf.spring.dao.impl.UserDaoImpl" ></bean>
```
- 在Spring配置文件中，使用bean标签，标签里面添加对应属性，就可以实现对象创建
- 在Bean标签有很多属性
	
	- ```id``` 唯一标识
	- ```class``` 类全路径（包类路径）

- 创建对象时候，默认也是执行无参构造方法完成对象的创建

### 基于xml方式注入属性

- ```DI``` 依赖注入，就是注入属性

### 第一种注入方式：使用set方法进行注入

1. 创建类 ， 定义属性和对应的set方法

```Java
/**
* 演示使用 set 方法进行注入属性
*/
public class Book {
 	//创建属性
 	private String bname;
 	private String bauthor;
 	//创建属性对应的 set 方法
 	public void setBname(String bname) {
 		this.bname = bname;
 	}
 	public void setBauthor(String bauthor) {
 		this.bauthor = bauthor;
 	}
}
```


2. 在spring配置文件配置对象创建，配置属性注入

```xml
<!--2 set 方法注入属性--> 
<bean id="book" class="com.zf.spring5.Book">
 <!--使用 property 完成属性注入
 name：类里面属性名称
 value：向属性注入的值
 -->
 <property name="bname" value="易筋经"></property>
 <property name="bauthor" value="达摩老祖"></property>
</bean>
```

### 第二种注入方式：使用有参构造进行注入

1. 创建类，定义属性，创建属性对应的有参构造器

```Java
/**
* 使用有参数构造注入
*/
public class Orders {
 //属性
 private String oname;
 private String address;
 //有参数构造
 public Orders(String oname,String address) {
 	this.oname = oname;
 	this.address = address;
 }
```
2. 在Spring配置文件中进行配置

```xml
<!--3 有参数构造注入属性--> 
<bean id="orders" class="com.atguigu.spring5.Orders">
 	<constructor-arg name="oname" value="电脑"></constructor-arg>
 	<constructor-arg name="address" value="China"></constructor-arg>
</bean>
```

## IOC操作Bean管理(xml注入其他类型属性)

### 1. 字面量

- ```null```值

```xml
<property name="address">
 <null/>
</property>
```

- 属性值包含特殊符号

```xml
<!--属性值包含特殊符号
 1 把<>进行转义 &lt; &gt;
 2 把带特殊符号内容写到 CDATA
-->
<property name="address">
 	<value><![CDATA[<<南京>>]]></value>
</property>
```

### 2. 注入属性-外部bean

- 创建两个类```service```类和```dao```类
- 在```service```调用```dao```里面的方法
- 在Spring配置文件中进行配置

```Java
public class UserService {

    private UserDao userDao ;

    public UserDao getUserDao() {
        return userDao;
    }

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void add(){
        System.out.println("UserService add ......");
        userDao.update();
    }
}

```

```xml
    <bean id="userService" class="com.zf.spring.service.UserService" >
        <property name="userDao" ref="userDaoImpl"></property>
    </bean>

    <bean id="userDaoImpl" class="com.zf.spring.dao.impl.UserDaoImpl" ></bean>
```

### 3. 注入属性-内部bean

- 一对多关系：部门和员工。一个部门有多个员工，一个员工属于一个部门
- 在实体类之间表示一对多关系，员工表示所属部门，使用对象类型属性进行表示

```Java
//部门类
public class Dept {
 private String dname;
 public void setDname(String dname) {
 	this.dname = dname;
 } 
}
//员工类
public class Emp {
 private String ename;
 private String gender;
 //员工属于某一个部门，使用对象形式表示
 private Dept dept;
 public void setDept(Dept dept) {
 	this.dept = dept;
 }
 public void setEname(String ename) {
 	this.ename = ename;
 }
 public void setGender(String gender) {
 	this.gender = gender;
 } 
}
```

在spring配置文件中进行配置
```xml
<!--内部 bean--> 
<bean id="emp" class="com.atguigu.spring5.bean.Emp">
 <!--设置两个普通属性-->
 <property name="ename" value="lucy"></property>
 <property name="gender" value="女"></property>
 <!--设置对象类型属性-->
 <property name="dept">
 	<bean id="dept" class="com.atguigu.spring5.bean.Dept">
 		<property name="dname" value="安保部"></property>
 	</bean>
 </property>
</bean>
```

### 4. 注入属性-级联赋值

第一种写法

```xml
<!--级联赋值--> 
<bean id="emp" class="com.atguigu.spring5.bean.Emp">
 <!--设置两个普通属性-->
 <property name="ename" value="lucy"></property>
 <property name="gender" value="女"></property>
 <!--级联赋值-->
 <property name="dept" ref="dept"></property>
</bean> 
<bean id="dept" class="com.atguigu.spring5.bean.Dept">
 <property name="dname" value="财务部"></property>
</bean>
```

第二种写法

![image.png](https://pumpkn.xyz/upload/2021/10/image-f5ef11cff7f5452b90f7c8eb378af62c.png)

```xml

    <bean id="emp" class="com.zf.spring.pojo.Emp">
        <property name="name" value="zf"></property>
        <property name="dept" ref="dept"></property>
        <property name="dept.dname" value="研发部"></property>
    </bean>

    <bean id="dept" class="com.zf.spring.pojo.Dept">
    </bean>
```

### 5. ```xml```注入集合属性

- 注入数组类型属性
- 注入list集合类型属性
- 注入map集合类型属性

创建类、定义数组、list、map、set类型属性，生成对应get、set方法

```Java
public class Student {
    private String[] course ;
    private List<String> list ;
    private Map<String ,String> map ;
    private Set<String> set ;
    public Student(String[] course, List<String> list, Map<String, String> map) {
        this.course = course;
        this.list = list;
        this.map = map;
    }

    public Set<String> getSet() {
        return set;
    }

    public void setSet(Set<String> set) {
        this.set = set;
    }

    public Student() {
    }

    public String[] getCourse() {
        return course;
    }

    public void setCourse(String[] course) {
        this.course = course;
    }

    public List<String> getList() {
        return list;
    }

    public void setList(List<String> list) {
        this.list = list;
    }

    public Map<String, String> getMap() {
        return map;
    }

    public void setMap(Map<String, String> map) {
        this.map = map;
    }

    @Override
    public String toString() {
        return "Student{" +
                "course=" + Arrays.toString(course) +
                ", list=" + list +
                ", map=" + map +
                ", set=" + set +
                '}';
    }
}
```
 在Spring配置文件中进行配置
```xml
    <bean id="stu" class="zf.com.pojo.Student">
        <property name="course">
            <array>
                <value>数学</value>
                <value>英语</value>
                <value>物理</value>
                <value>化学</value>
            </array>
        </property>

        <property name="list">
            <list>
                <value>1</value>
                <value>2</value>
                <value>3</value>
                <value>4</value>
            </list>
        </property>
        <property name="map">
            <map>
                <entry key="1" value="1" ></entry>
                <entry key="2" value="2" ></entry>
                <entry key="3" value="3" ></entry>
            </map>
        </property>
        <property name="set">
            <set>
                <value>1</value>
                <value>2</value>
                <value>3</value>
            </set>
        </property>
    </bean>
```


### 4.在集合里面注入对象类型值

```xml
<!--创建多个 course 对象--> 
<bean id="course1" class="com.atguigu.spring5.collectiontype.Course">
 	<property name="cname" value="Spring5 框架"></property>
</bean> 
<bean id="course2" class="com.atguigu.spring5.collectiontype.Course">
 	<property name="cname" value="MyBatis 框架"></property>
</bean>
<!--注入 list 集合类型，值是对象--> 
<property name="courseList">
 <list>
 	<ref bean="course1"></ref>
 	<ref bean="course2"></ref>
 </list>
</property>
```

### IOC操作Bean管理(FactorBean)

Spring有两种类型bean，一种普通bean，另外一种工厂bean(FactorBean)

- 普通bean: 在配置文件中定义bean类型就是返回类型
- 工厂bean: 在配置文件定义bean类型可以和返回类型不一样

	- 第一步: 创建类，让这个类作为工厂bean，实现接口```FactoryBean```
	- 第二部: 实现接口里面的方法，在实现的方法中定义返回的bean类型

```Java
public class MyBean implements FactoryBean<Course> {
 //定义返回 bean
 @Override
 public Course getObject() throws Exception {
 Course course = new Course();
 course.setCname("abc");
 return course;
 }
 @Override
 public Class<?> getObjectType() {
 return null;
 }
 @Override
 public boolean isSingleton() {
 return false;
 } }
```

```xml
<bean id="myBean" class="com.atguigu.spring5.factorybean.MyBean">
</bean>
```

```Java
@Test
public void test3() {
 	ApplicationContext context = new ClassPathXmlApplicationContext("bean3.xml");
 	Course course = context.getBean("myBean", Course.class);
 	System.out.println(course);
}
```

### IOC操作Bean管理(Bean作用域)

在Spring里面，设置创建bean实例是单实例还是多实例，<font color="red">在Spring里面，默认情况下，bean是单实例对象</font>

![image.png](https://pumpkn.xyz/upload/2021/10/image-baf3eabf4be149868ec57a7c3de47386.png)

### 如何设置单实例还是多实例

- 在Spring配置文件bean标签里面有属性(scope)，用于设置单实例还是多实例
- ```scope```属性值

	- ```singleton``` 默认值，表示单实例对象
	- ```prototype``` 表示多实例对象

### ```singleton```和```prototype```区别

- ```singleton```单实例，```prototype```多实例
- 设置scope值是```singleton```时候，加载spring配置文件时候就会创建单实例对象
- 设置scope值是```prototype```时候，不是在加载spring配置文件创建对象，在调用```getBean```方法时候创建多实例对象


## Bean生命周期

- 生命周期

	- 从对象创建到对象销毁的过程

- bean生命周期

	- 通过构造器创建bean实例（无参构造器）
	- 为bean的属性设置值和其他bean引用（调用set方法）
	- 调用bean的初始化方法（需要进行配置初始化方法）
	- beam可以使用了（对象获取到了）
	- 当容器关闭时，调用bean的销毁方法（需要进行配置销毁的方法）


演示bean生命周期

```Java
public class Orders {
 //无参数构造
 public Orders() {
 	System.out.println("第一步 执行无参数构造创建 bean 实例");
 }
 private String oname;
 public void setOname(String oname) {
 	this.oname = oname;
 	System.out.println("第二步 调用 set 方法设置属性值");
 }
 //创建执行的初始化的方法
 public void initMethod() {
 	System.out.println("第三步 执行初始化的方法");
 }
 //创建执行的销毁的方法
 public void destroyMethod() {
 	System.out.println("第五步 执行销毁的方法");
 } 
}
```
配置文件
```xml
<bean id="orders" class="com.zf.spring5.bean.Orders" init-method="initMethod" destroy-method="destroyMethod">
 	<property name="oname" value="手机"></property>
</bean>
```

测试
```Java
 @Test
 public void testBean3() {
 	ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean4.xml");
 	Orders orders = context.getBean("orders", Orders.class);
 	System.out.println("第四步 获取创建 bean 实例对象");
 	System.out.println(orders);
 	//手动让 bean 实例销毁
 	context.close();
 }
```
![image.png](https://pumpkn.xyz/upload/2021/10/image-61d299646b374452b45ec79c3ea00ab5.png)

### bean的后置处理器，bean生命周期有七步

- 通过构造器创建 bean 实例（无参数构造）
- 为 bean 的属性设置值和对其他 bean 引用（调用 set 方法）
- 把 bean 实例传递 bean 后置处理器的方法 postProcessBeforeInitialization 
- 调用 bean 的初始化的方法（需要进行配置初始化的方法）
- 把 bean 实例传递 bean 后置处理器的方法postProcessAfterInitialization
- bean 可以使用了（对象获取到了）
- 当容器关闭时候，调用 bean 的销毁的方法（需要进行配置销毁的方法）


```Java

public class MyBeanPost implements BeanPostProcessor {
 @Override
 public Object postProcessBeforeInitialization(Object bean, String beanName) 
throws BeansException {
 System.out.println("在初始化之前执行的方法");
 return bean;
 }
 @Override
 public Object postProcessAfterInitialization(Object bean, String beanName) 
throws BeansException {
 System.out.println("在初始化之后执行的方法");
 return bean;
 } }
```


## xml自动装配

- 什么时自动装配

根据指定装配规则（属性名或者属性类型），Spring自动将匹配的属性值进行注入

- 演示自动装配

根据属性名称自动注入

```xml
（1）根据属性名称自动注入
<!--实现自动装配
 bean 标签属性 autowire，配置自动装配
 autowire 属性常用两个值：
 byName 根据属性名称注入 ，注入值 bean 的 id 值和类属性名称一样
 byType 根据属性类型注入
-->
<bean id="emp" class="com.atguigu.spring5.autowire.Emp" autowire="byName">
 <!--<property name="dept" ref="dept"></property>-->
</bean>
<bean id="dept" class="com.atguigu.spring5.autowire.Dept"></bean>
```

根据属性类型自动注入

```xml
<!--实现自动装配
 bean 标签属性 autowire，配置自动装配
 autowire 属性常用两个值：
 byName 根据属性名称注入 ，注入值 bean 的 id 值和类属性名称一样
 byType 根据属性类型注入
-->
<bean id="emp" class="com.atguigu.spring5.autowire.Emp" autowire="byType">
 <!--<property name="dept" ref="dept"></property>-->
</bean> 
<bean id="dept" class="com.atguigu.spring5.autowire.Dept"></bean>
```


## IOC操作bean管理（外部属性文件）
- 直接配置数据库信息

	- 配置德鲁伊连接池
	- 引入德鲁伊连接池依赖jar包

![image.png](https://pumpkn.xyz/upload/2021/10/image-190c371400cf424e99dd22626a4e2f13.png)
```xml
<!--直接配置连接池--> 
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
 <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
 <property name="url" value=""jdbc:mysql://localhost:3306/jdbctemp?useUnicode=true&amp;characterEncoding=utf-8&amp;userSSL=false&amp;serverTimezone=GMT%2B8""></property>
 <property name="username" value="root"></property>
 <property name="password" value="root"></property>
</bean>
```

- 引入外部属性文件配置数据库连接池

	- 创建外部属性文件，properties 格式文件，写数据库信息
![image.png](https://pumpkn.xyz/upload/2021/10/image-0046ce829ecf421ab58a96ec3bebe30f.png)
	- 把外部 properties 属性文件引入到 spring 配置文件，引入```context```命名空间
```xml
<beans xmlns="http://www.springframework.org/schema/beans" 
 	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
 	xmlns:p="http://www.springframework.org/schema/p" 
 	xmlns:util="http://www.springframework.org/schema/util" 
 	xmlns:context="http://www.springframework.org/schema/context" 
 	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd 
 			http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd 
 			http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
```

在spring配置文件使用标签引入外部属性文件

```xml
<!--引入外部属性文件--> 
<context:property-placeholder location="classpath:jdbc.properties"/>
<!--配置连接池--> 
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
 	<property name="driverClassName" value="${prop.driverClass}"></property>
 	<property name="url" value="${prop.url}"></property>
 	<property name="username" value="${prop.userName}"></property>
 	<property name="password" value="${prop.password}"></property>
</bean>
```

## <span id = "6">IOC操作Bean管理（基于注解方式） </span>

- 什么是注解

	- 注解是代码特殊标记，格式：```@注解名称(属性名称=属性值,属性名称=属性值...)```
	- 使用注解，注解作用在类上面，方法上面，属性上面
	- 使用注解的目的：简化xml配置


- Spring针对Bean管理中创建对象提供注解

	- ```@Component```
	- ```@Service```
	- ```@Controller```
	- ```@Repository```
> 上面四个注解功能是一样的，都可以用来创建bean实例

### 基于注解方式实现对象创建
**第一步 引入依赖**

![image.png](https://pumpkn.xyz/upload/2021/10/image-3cce24cf0b47464f81ce679a8da3cb97.png)

**第二步 开启组件扫描**
```xml
第二步 开启组件扫描
<!--开启组件扫描
 1 如果扫描多个包，多个包使用逗号隔开
 2 扫描包上层目录
-->
<context:component-scan base-package="com.zf"></context:component-scan>
```

**第三步 创建类，在类上面添加创建对象注解**

```java
//在注解里面 value 属性值可以省略不写，
//默认值是类名称，首字母小写
//UserService -- userService
@Component(value = "userService")   //相当于<bean id="userService" class=".."/>
public class UserService {
 	public void add() {
 		System.out.println("service add.......");
 	} 
}
```

**第四步 开启组件扫描细节配置**

```xml
<!--示例 1
 use-default-filters="false" 表示现在不使用默认 filter，自己配置 filter
 context:include-filter ，设置扫描哪些内容
-->
<context:component-scan base-package="com.atguigu" use-defaultfilters="false">
 	<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>

```

```xml
<!--示例 2
 下面配置扫描包所有内容
 context:exclude-filter： 设置哪些内容不进行扫描
-->
<context:component-scan base-package="com.atguigu">
 <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

**第五步 基于注解方式实现属性注入**

- ```@Autowired``` 根据属性类型进行自动装配

	- 把service和dao对象创建，在service和dao类添加创建对象注解
	- 在service注入dao对象，在service类添加dao类型属性，在属性上面使用注解


```java
@Service
public class UserService {
 	//定义 dao 类型属性
 	//不需要添加 set 方法
 	//添加注入属性注解
 	@Autowired 
 	private UserDao userDao;
 	public void add() {
 		System.out.println("service add.......");
 		userDao.add();
 	} 
}

```

- ```@Qualifier``` 根据名称进行注入
	
	- @Qualifier需要和@Autowired一起使用

```Java
//定义 dao 类型属性
//不需要添加 set 方法
//添加注入属性注解
	@Autowired //根据类型进行注入
	@Qualifier(value = "userDaoImpl1") //根据名称进行注入
	private UserDao userDao;
```

- ```@Resource``` 可以根据类型注入，可以根据名称注入

	- @Resource不属于spring包下的注解，而是javax包下的注解，所以一般不使用

```Java
//@Resource //根据类型进行注入
@Resource(name = "userDaoImpl1") //根据名称进行注入
private UserDao userDao;
```

- ```@Value``` 注入普通类型属性

```Java
@Value(value = "abc")
private String name;
```


## 完全注解开发
创建配置类替代xml配置文件

```Java
@Configuration //作为配置类，替代 xml 配置文件
@ComponentScan(basePackages = {"com.atguigu"})
public class SpringConfig {

}

```

测试类

```Java
@Test
public void testService2() {
 //加载配置类
 	ApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
 	UserService userService = context.getBean("userService", UserService.class);
 	System.out.println(userService);
 	userService.add();
}
```

# <span id = "7">AOP</span>

## 1. 什么是AOP

- 面向切面编程（方面），利用aop可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的**耦合度**降低，提高程序的可重用性，同时提高了开发的效率。
- 通俗描述：不通过修改源代码的方式、在主干功能里面添加新功能。


## aop登陆案例

![image.png](https://pumpkn.xyz/upload/2021/10/image-63c25684df804270984a486fd8026285.png)

## ```AOP```底层原理

有两种情况动态代理

- 有接口的情况，使用jdk动态代理

	- 创建接口实现类代理对象，增强类的方法
	- 