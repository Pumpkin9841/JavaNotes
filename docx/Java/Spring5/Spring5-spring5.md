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
<context:component-scan base-package="com.atguigu" use-defaultfilters="false">
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
	- ![image](https://user-images.githubusercontent.com/91939988/142454698-cf6ce6e8-d278-4a59-ac74-2b1eb4ec7e27.png)


- 没有接口的情况，使用CGLIB动态代理

    - 创建子类的代理对象，增强类的方法
    - ![image](https://user-images.githubusercontent.com/91939988/142454744-3baa0fe5-b505-4cf7-ae3f-db5afc4b7b7b.png)
    
 ### 使用JDK动态代理
 使用JDK动态代理，使用Proxy类里面的方法创建代理对象
 
 1. 调用newProxyInstance方法

```java
static Object newProxyInstance(ClassLoader loader , class<?>[] interfaces , InvocationHander h)
```
![image](https://user-images.githubusercontent.com/91939988/142454794-9c3cc077-4174-42ad-90fd-2664b9ef5d97.png)

  - 方法有三个参数

      - 类加载器
      - 增强方法所在的类，这个类实现的接口，支持多个接口
      - 实现这个接口InvocaionHander，创建代理对象，写增强的部分


2. 编写JDK动态代理代码

    (1) 创建接口，定义方法
    
```java
public interface UserDao {    
    public int add( int a , int b ) ;    
    public String update(String s) ;}

```
   
   (2) 创建接口实现类，实现方法
    
    
```java

public class UserDaoImpl implements UserDao {
    @Override
    public int add(int a, int b) {
        return a+b;
    }
    @Override
    public String update(String id) {
        return id;
    }
}

```
(3) 使用proxy类创建接口代理对象


```java
public class JDKProxy {  

public static void main(String[] args) {        
    UserDao o = (UserDao) Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), new Class[]{UserDao.class}, new InvocationHandler() {            
        @Override           
        public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {    
                UserDaoImpl userDao = new UserDaoImpl();               
                System.out.println( method.getName() + "执行前");               
                Object invoke = method.invoke(userDao, args);                
                System.out.println(invoke);                
                System.out.println(method.getName() + "执行后");               
                return invoke ;           
            }       
        });       
    o.add(1,2);    
    }
}


```


### APO术语

1. 连接点
    类里面哪些方法可以被增强，这些方法成为连接点
    
2. 切入点
    实际被真正增强的方法，成为切入点
3. 通知（增强）

    - 实际增强的逻辑部分成为通知（增强）
    - 通知有多种类型

        - 前置通知
        - 后置通知
        - 环绕通知
        - 异常通知
        - 最终通知
4. 切面
    是一个动作，把通知应用到i恶如点的过程

### AOP操作（准备工作）

1. Spring框架一般都是基于AspectJ实现AOP操作

    - Aspect不是Spring组成部分，独立AOP框架，一般把AspectJ和Spring框架一起使用，进行AOP操作


2. 基于AspectJ实现AOP操作
    
    - 基于xml配置文件实现
    - 基于注解方式实现（使用得多）


3. 在项目里引入AOP相关依赖

![image](https://user-images.githubusercontent.com/91939988/142454926-2d417f75-86a3-466a-b1b3-7d12d9f1b247.png)


4. 切入点表达式

    - 切入点表达式作用：知道对哪个类里面的哪个方法进行增强
    - 语法结构
    - ```execution([权限修饰符][返回类型][类全路径][方法名称][参数列表]) ```

示例1：

```java
//对 com.zf.dao.BookDao类里面的add进行增强
execution(* com.zf.dao.BookDao.add(..))
```

示例2

```java
//对com.zf.dao.BookDao类里面的所有方法进行增强
execution(* com.zf.dao.BookDao.*(..))
```

示例3

```java
//对com.zf.dao包里面的所有类，类里面的所有方法进行增强
execution(* com.zf.dao.*.*(..))
```

### AOP操作（AspectJ注解实现）

1.  创建类，在类里面定义方法
    
```java

public class User {
    public void add() {
        System.out.println("add.......");
    }
}

```
2.  创建增强类（编写增强逻辑）
   
   - 在增强类里面，创建方法，让不同方法代表不同通知类型

```java

//增强的类
public class UserProxy {
    public void before() {//前置通知
        System.out.println("before......");
    }
}

```

3. 进行通知配置
    (1) 在Spring配置文件中，开启注解扫描
    
```xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:aop="http://www.springframework.org/schema/aop"
xsi:schemaLocation="http://www.springframework.org/schema/beans
                            http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/context
                            http://www.springframework.org/schema/context/spring-context.xsd
                            http://www.springframework.org/schema/aop
                            http://www.springframework.org/schema/aop/spring-aop.xsd">


<!-- 开启注解扫描 -->
<context:component-scan base-package="com.zf.spring5.aopanno"></context:component-scan>

```

(2) 使用注解创建User和UserProxy对象
```java
//被增强的类
@Component
public class User{

}

// 增强类
@Component
public class UserProxy{

}

```

(3) 在增强类上面添加注解@Aspect

```java
//增强的类
@Component
@Aspect  //生成代理对象
public class UserProxy{

}

```

4. 配置不同类型的通知
    (1)在增强类的里面，在作为通知方法上面添加通知类型注解，使用切入点表达式配置
    
    
```java
//增强的类
@Component
@Aspect  //生成代理对象
public class UserProxy{
    //前置通知
    //@Before 注解表示作为前置通知
    @Before(value="execution(* com.zf.spring5.aopanno.User.add(..))")
    public void before(){
        System.out.println("before...........") ;
    }
    
    //后置通知（返回通知）
    @AfterReturning(value="execution(* com.zf.spring5.aopanno.User.add(..))")
    public void afterReturning(){
        System.out.println("afterReturning...........") ;
    }
    
    //最终通知
    @After(value="execution(* com.zf.spring5.aopanno.User.add(..))")
    public void after(){
        System.out.println("after...........") ;
    }
    
    //异常通知
    @AfterThrowing(value="execution(* com.zf.spring5.aopanno.User.add(..))")
    public void afterThrowing(){
        System.out.println("AfterThrowing...........") ;
    }
    
    //环绕通知
    @Around(value="execution(* com.zf.spring5.aopanno.User.add(..))")
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable{
    System.out.println("环绕之前...........") ;
    proceedingJoinPoint.proceed() ;
    System.out.println("环绕之后...........") ;
    }
}
```
测试
```java
@Testpublic void test1(){  
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("demo_5_2.xml");   
    UserBase userBase = applicationContext.getBean("userBase", UserBase.class); 
    userBase.add();}

```


5. 相同的切入点抽取

```java
//相同的切入点抽取
@Pointcut(value="execution(* com.zf.spring5.aopanno.User.add(..))")
public void pointdemo(){

}

//前置通知
@Before(value="pointdemo()")
public void before(){
    System.out.println("before...........") ;
}
```


6. 有多个增强类对同一个方法进行增强，设置增强类优先级
    (1) 在增强类上面增加注解@Order(数字类型值)，数字类型值越小优先级越高
    
    
```java
@Component
@Aspect
@Order(1)
public class PersonProxy{

}
```
    
7. 完全使用注解开发

  (1) 创建配置类，不需要创建xml配置文件
  
```java
@Configuration
@ComponentScan(basePackages={"com.zf"})
@EnableAspectJAutoProxy(proxyTargetClass=true)
public class ConfigAop{

}
```

### AOP操作（AspectJ配置文件实现）

1. 创建两个类，增强类和被增强类，创建方法
2.在Spring配置文件中创建两个类对象
```xml

<bean id="book" class="com.zf.spring5.aopxml.Book"></bean>
<bean id="bookProxy" class="com.zf.spring5.aopxml.BookProxy"></bean>

```

3. 在Spring配置文件中配置切入点
```xml
<!-- 配置aop增强 -->
<aop:config>
    <!-- 切入点 -->
    <aop:pointcut id="p" expression="execution(* com.zf.spring5.aopxml.Book.buy(..))" />
    <!-- 配置切面 -->
    <aop:aspect ref="bookProxy">
        <!-- 增强作用在具体的方法上 -->
        <aop:before method="before" pointcut-ref="p">
    </aop:aspect>

</aop:config>
```
 
 
 # <span id = "8">JdbcTemplate </span>
 
 ### 1 什么是JdbcTemplate
 
 (1) Spring框架对JDBC进行封装，使用JdbcTemplate方便实现对数据库操作
 
 ### 2 JdbcTemplate准备工作
 
 (1) 引入相关jar包
 
 ![image](https://user-images.githubusercontent.com/91939988/142455007-278113fa-5aaa-4c51-b577-223ae21b9ad7.png)
 
 (2) 在Spring配置文件配置数据库连接池
 
 
```xml
<!-- 数据库连接池 -->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
    <property name="url" value="jdbc:mysql://localhost:3306/jdbctemp?useUnicode=true&amp;characterEncoding=utf-8&amp;userSSL=false&amp;serverTimezone=GMT%2B8">
    <property name="username" value="root" />
    <property name="password" value="123456" />
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
    
</bean>
```

(3) 配置JdbcTemplate对象，注入DataSource

```xml
<!-- JdbcTemplate对象 -->
<bean id = "jdbcTemplate"  class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource"  ref="dataSource" />
</bean> 
```

(4) 创建service类，创建dao类，在dao注入jdbcTemplate对象

```xml
<!--开启组件扫描-->
<context:component-scan base-package="com.zf" ></context:component-scan>
```

Dao
```java
@Repository
public class BookDaoImple implements BookDao{
    
    //注入JdbcTemplate
    @Autowired
    private JdbcTemplate jdbcTemplate ;
}
```

Service
```java

public class BookServcie{
    //注入dao
    @Autowired
    private BookDao bookdao ;
}

```

### 3 JdbcTemplate操作数据库（添加）

1. 对应数据库创建实体类(pojo)
```java
public class Book {   
    private int id ;  
    private String name ;
    //get  set 方法
}
```

2. 编写service和dao
    (1) 在dao进行数据库添加操作
    (2) 调用JdbcTemplate对象里面update方法实现添加操作
![image](https://user-images.githubusercontent.com/91939988/142455069-8c88da90-bf63-4b9a-8abf-4c24bc30e7ce.png)

- 有两个参数

    - 第一个参数：sql 语句
    - 第二参数： 可变参数，设置sql语句的变量

```java

public class BookDaoImpl implements BookDao {
    //注入 JdbcTemplate
    @Autowired
    private JdbcTemplate jdbcTemplate;
    //添加的方法
    @Override
    public void add(Book book) {
        //1 创建 sql 语句
        String sql = "insert into t_book values(?,?,?)";
        //2 调用方法实现
        Object[] args = {book.getUserId(), book.getUsername(),
        book.getUstatus()};
        int update = jdbcTemplate.update(sql,args); 
        System.out.println(update);
    }
}

```

3. 测试类

```java
@Test
public void testJdbcTemplate() {
     ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
     BookService bookService = context.getBean("bookService", BookService.class);
     Book book = new Book();
     book.setUserId("1");
     book.setUsername("java");
     book.setUstatus("a");
     bookService.addBook(book);
}
```

### 4 JdbcTemplate操作数据库（修改和删除）

1. 修改

```java

@Override
public void updateBook(Book book) {
    String sql = "update t_book set username=?,ustatus=? where user_id=?";
    Object[] args = {book.getUsername(), book.getUstatus(),book.getUserId()};
    int update = jdbcTemplate.update(sql, args);
    System.out.println(update);
}

```

2. 删除
```java

@Override
public void delete(String id) {
    String sql = "delete from t_book where user_id=?";
    int update = jdbcTemplate.update(sql, id);
    System.out.println(update);
}

```

### 5 JdbcTemplate操作数据库（查询返回某个值）

1. 查询表里面有多少条记录，返回是某个值
![image](https://user-images.githubusercontent.com/91939988/142455147-72ffedd3-4fa9-454a-85ec-772cde7c37f5.png)

- 有两个参数

    - 第一个参数：sql语句
    - 第二个参数： 返回类型class
    
    
```java

@Override
public int selectCount() {
    String sql = "select count(*) from t_book";
    Integer count = jdbcTemplate.queryForObject(sql, Integer.class);
    return count;
}

```

### 6 JdbcTemplate操作数据库（查询返回对象）

1. 场景：查询图书详情
![image](https://user-images.githubusercontent.com/91939988/142455181-6b3121d5-cfc9-4249-a5ee-3bc89214abf0.png)

- 有三个参数

    - 第一个参数：sql语句
    - 第二个参数：RowMapper事接口，针对返回不同类型数据，使用这个接口里面实现类完成数据封装
    - 第三个参数：sql语句值


```java
//查询返回对象
@Override
public Book findBookInfo(String id) {
    String sql = "select * from t_book where user_id=?";
    //调用方法
    Book book = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<Book>(Book.class), id);
    return book;
}

```

### 7 JdbcTemplate操作数据库（查询返回集合）

1 场景：查询图书列表分页...

![image](https://user-images.githubusercontent.com/91939988/142455216-21a9bd51-b7e5-43c6-92c1-874fe6e7c7eb.png)

- 有三个参数

    - 第一个参数：sql语句
    - 第二个参数：RowMapper是接口，针对返回不同类型数据，使用这个接口实现类完成数据封装
    - 第三个参数：sql语句


```java

@Override
public List<Book> findAllBook() {
    String sql = "select * from t_book";
    //调用方法
    List<Book> bookList = jdbcTemplate.query(sql, new BeanPropertyRowMapper<Book>(Book.class));
    return bookList;
}

```

### JdbcTemplate操作数据库（批量操作）

1. 批量操作：同时操作表里面多条记录

![image](https://user-images.githubusercontent.com/91939988/142455243-2b284f8c-fae4-431e-b9b6-aad186dacef0.png)

- 有两个参数

    - 第一个参数：sql语句
    - 第二个参数：List集合，添加多条记录数据   


批量添加

```java

//批量添加
@Override
public void batchAddBook(List<Object[]> batchArgs) {
    String sql = "insert into t_book values(?,?,?)";
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
    System.out.println(Arrays.toString(ints));
}
//批量添加测试
List<Object[]> batchArgs = new ArrayList<>();
Object[] o1 = {"3","java","a"};
Object[] o2 = {"4","c++","b"};
Object[] o3 = {"5","MySQL","c"};
batchArgs.add(o1);
batchArgs.add(o2);
batchArgs.add(o3);
//调用批量添加
bookService.batchAdd(batchArgs);

```

批量修改

```java
//批量修改

@Override
public void batchUpdateBook(List<Object[]> batchArgs) {
    String sql = "update t_book set username=?,ustatus=? where user_id=?";
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
    System.out.println(Arrays.toString(ints));
}
//批量修改
List<Object[]> batchArgs = new ArrayList<>();
Object[] o1 = {"java0909","a3","3"};
Object[] o2 = {"c++1010","b4","4"};
Object[] o3 = {"MySQL1111","c5","5"};
batchArgs.add(o1);
batchArgs.add(o2);
batchArgs.add(o3);
//调用方法实现批量修改
bookService.batchUpdate(batchArgs);

```

批量删除

```java

//批量删除
@Override
public void batchDeleteBook(List<Object[]> batchArgs) {
    String sql = "delete from t_book where user_id=?";
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
    System.out.println(Arrays.toString(ints));
}
//批量删除
List<Object[]> batchArgs = new ArrayList<>();Object[] o1 = {"3"};
Object[] o2 = {"4"};
batchArgs.add(o1);
batchArgs.add(o2);
//调用方法实现批量删除
bookService.batchDelete(batchArgs);

```

# <span id = "9">事务操作（事务概念） </span>

1. 什么是事务
(1) 事务是数据库操作最基本单元，逻辑上一组操作，要么都成功，要么都失败。如果有一个失败，所有操作都失败。
(2) 典型场景：银行转账
    lucy 转账100元给mary
    lucy少100 mary多100
2. 事务四个特性（ACID）

- 原子性
- 一致性
- 隔离性
- 持久性

### 事物操作（搭建事物操作环境）

![image](https://user-images.githubusercontent.com/91939988/142455307-c2c58982-d357-4039-b551-f156d2d79202.png)

1. 创建数据库表，添加记录

![image](https://user-images.githubusercontent.com/91939988/142455325-984eff6b-daf7-45c9-b8a1-3dee3ed0a515.png)


2. 创建service，搭建dao，完成对象创建和注入关系

(1) service注入dao，在dao中注入jdbctemplate，在JdbcTemplate中注入DataSource
```java


@Service
public class UserService {
    //注入 dao
    @Autowired
    private UserDao userDao;
}


@Repository
public class UserDaoImpl implements UserDao {
    //注入jdbcTemplate
    @Autowired 
    private JdbcTemplate jdbcTemplate;
}
```

在配置文件中给JdbcTemplate注入DataSource

```xml
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">    
    <property name="url" value="jdbc:mysql://localhost:3306/jdbctempuseUnicode=true&amp;characterEncoding=utf8&amp;userSSL=false&amp;serverTimezone=GMT%2B8"></property>  
    <property name="username" value="root" ></property>    
    <property name="password" value="123456" ></property>    
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" ></property>
</bean>

<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">   
    <property name="dataSource" ref="dataSource" ></property>
</bean>


```

3. 在dao创建两个方法：多钱和少钱的方法，在service创建方法（转账的方法）

```java

@Repository
public class UserDaoImpl implements UserDao {
    @Autowired
    private JdbcTemplate jdbcTemplate;
    //lucy 转账 100 给 mary
    //少钱
    @Override
    public void reduceMoney() {
    String sql = "update t_account set money=money-? where username=?";
    jdbcTemplate.update(sql,100,"lucy");
    }
    //多钱
    @Override
    public void addMoney() {
    String sql = "update t_account set money=money+? where username=?";
    jdbcTemplate.update(sql,100,"mary");
    }
}



@Service
public class UserService {
    //注入 dao
    @Autowired
    private UserDao userDao;
    //转账的方法
    public void accountMoney() {
       //lucy 少 100
       userDao.reduceMoney();
       //mary 多 100
       userDao.addMoney();
   }
}

```

上面代码，如果正常执行没有问题，但是如果代码执行过程中出现异常，有问题。

是使用事务进行解决

### 事务操作过程

```java
@Service
public class UserService {
    //注入 dao
    @Autowired
    private UserDao userDao;
    //转账的方法
    public void accountMoney() {
        try{
            //第一步 开启事务
            //第二部 进行业务操作
            //lucy 少 100
           userDao.reduceMoney();
           //mary 多 100
           userDao.addMoney();
           // 第三步 没有发生异常，提交事务
        }
        catch( Exception e ){
            //第四步 出现异常，事务回滚
        }
   }
}
```

### 事务操作（Spring事务管理）

1. 事务添加到JavaEE三层结构里面Service层（业务逻辑层）
2. 在Spring进行事务管理操作
   - 有两种方式
        - 编程式事务管理
        - 声明式事务管理（推荐）
3. 声明式事务管理
    (1) 基于注解方式（推荐）
    (2) 基于xml配置文件方式

4. 在Spring进行声明式事务管理，底层使用AOP原理

5. Spring事务管理API
    (1) 提供一个接口，代表事务管理器，这个接口针对不同的框架提供不同的实现类
    ![image](https://user-images.githubusercontent.com/91939988/142455403-75265441-fcc1-4884-bf99-e7b81d688624.png)
    
### 事务操作（注解声明式事务管理）
1 在Spring配置文件配置事务管理器

```xml

<!--创建事务管理器-->
<bean id="transactionManager"class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <!--注入数据源-->
    <property name="dataSource" ref="dataSource"></property>
</bean>

```
    

2. 在Spring配置文件，开启事务注解
(1) 在Spring配置文件引入名称空间tx
```xml

<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:aop="http://www.springframework.org/schema/aop"
xmlns:tx="http://www.springframework.org/schema/tx"
xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/aop
            http://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/tx
            http://www.springframework.org/schema/tx/spring-tx.xsd">

```
(2) 开启事务注解
```xml

<!--开启事务注解-->
<tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>

```

3. 在service类上面（或者service类里面方法上面）添加事务注解
(1) @Transactional，这个注解添加到类上面，也可以添加方法上面
(2) 如果把这个注解添加到类上面，这个类里面所有的方法都添加事务
(3) 如果把这个注解添加方法上面，为这个方法添加事务

```java

@Service
@Transactional
public class UserService {

}
```

### 事务操作（声明式事务管理参数配置）

1. 在service类上面添加注解@Transactional，在这个注解里面可以配置事务相关参数

![image](https://user-images.githubusercontent.com/91939988/142455466-229f5483-dd30-4b02-8b75-9769b21795e0.png)


2. propagation: 事务传播行为
(1) 有事务方法直接调用另外一个的方法，这个过程中事务式如何进行管理的----------事务传播行为

事务方法：对数据库表数据进行变化的操作
![image](https://user-images.githubusercontent.com/91939988/142455493-c8ef8045-ed0f-4ca3-94cb-3345d5d83bc7.png)

Spring框架事务传播行为有7种
![image](https://user-images.githubusercontent.com/91939988/142455528-b37440db-5821-4f78-8f6c-744dee25bc78.png)

```java
@Service
@Transactional(propagation = Propagation.REUIRED)
public class UserService {

}
```

3. ioslation 事务隔离级别
(1) 事务有特性成为隔离性，多事务操作之间不会产生影响。不考虑隔离性产生很多问题
(2) 有三个读问题：脏读、不可重复度、幻读
- 脏读：一个未提交事务读取到另一个未提交事务的数据
- 不可重复读：一个未提交事务读取到另一提交事务的修改操作的数据
- 幻读：一个未提交事务读取到另一提交事务的添加操作的数据
(3) 解决：通过设置事务隔离级别，解决读问题
![image](https://user-images.githubusercontent.com/91939988/142455555-9efd3b12-422c-4b69-847f-bea5668c7087.png)

```java
@Service
@Transactional(propagation = Propagation.REUIRED,isolation=Isolation.REPEATABLE_READ)
public class UserService {

}
```

4. timeout 超时时间
(1) 事务需要在一定时间内进行提交，如果不提交进行回滚
(2) 默认值是-1，设置时间以秒为单位进行计算
5. readOnly 是否只读
(1) 读：查询操作，写：添加修改删除
(2) readOnly默认值 false,表示可以查询，可以添加修改删除
(3) 设置readOnly值是true，只能查询
6. rollbackFor 回滚
(1) 设置出现哪些异常进行事务回滚
7 noRollbackFor 不会滚
(1) 设置出现哪些异常不进行事务回滚


### 事务操作(xml声明式事务管理)
1. 在Spring配置文件中进行配置
- 配置事务管理器
- 配置通知
- 配置切入点和切面

```xml
<!-- 1创建事务管理器-->
<bean id="transactionManager"class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <!--注入数据源-->
    <property name="dataSource" ref="dataSource"></property>
</bean>
<!--2 配置通知-->
<tx:advice id="txadvice">
    <!--配置事务参数-->
    <tx:attributes>
        <!--指定哪种规则的方法上面添加事务-->
        <tx:method name="accountMoney" propagation="REQUIRED"/>
        <!--<tx:method name="account*"/>-->
    </tx:attributes>
</tx:advice>
<!--3 配置切入点和切面-->
<aop:config>
    <!--配置切入点-->
    <aop:pointcut id="pt" expression="execution(*com.atguigu.spring5.service.UserService.*(..))"/>
    <!--配置切面-->
    <aop:advisor advice-ref="txadvice" pointcut-ref="pt"/>
</aop:config>

```

### 事务操作（完全注解声明式事务管理）
```java

// 1、创建配置类，使用配置类替代 xml 配置文件
@Configuration //配置类
@ComponentScan(basePackages = "com.zf") //组件扫描
@EnableTransactionManagement //开启事务
public class TxConfig {
//创建数据库连接池
@Bean
public DruidDataSource getDruidDataSource() {
    DruidDataSource dataSource = new DruidDataSource();
    dataSource.setDriverClassName("com.mysql.jdbc.Driver");
    dataSource.setUrl("jdbc:mysql:///user_db");
    dataSource.setUsername("root");
    dataSource.setPassword("root");
    return dataSource;
}
//创建 JdbcTemplate 对象
@Bean
public JdbcTemplate getJdbcTemplate(DataSource dataSource) {
    //到 ioc 容器中根据类型找到 dataSource
    JdbcTemplate jdbcTemplate = new JdbcTemplate();
    //注入 dataSource
    jdbcTemplate.setDataSource(dataSource);
    return jdbcTemplate;
}
//创建事务管理器
@Bean
public DataSourceTransactionManager getDataSourceTransactionManager(DataSource dataSource) {
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource);
        return transactionManager;
    }
}

```
