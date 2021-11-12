---
title: 【Java基础】反射
date: 2021-09-20 11:19:54.383
updated: 2021-09-20 12:54:18.177
url: https://pumpkn.xyz/archives/java基础反射
categories: 
tags: 学习 | Java
---

# 反射
通过外部文件配置，在不修改源码情况下，来控制程序，这样的需求在学习框架时特别多，也符合设计模式的ocp原则（开闭原则）

# 反射快速入门

- 根据配置文件```re.properties```指定信息，创建Cat对象并调用方法eat

## ```re.properties```文件
```properties
classfullpath=test.Cat
methodName=eat
```
## 代码
```Java
public class z1_反射入门 {
    public static void main(String[] args) throws IOException, ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException {
        Properties properties = new Properties();
        properties.load(new FileInputStream("src\\ref.properties"));
        String classfullpath = properties.getProperty("classfullpath");
        String methodName = properties.getProperty("methodName");
        //使用反射机制
        //加载类，返回Class类型的对象cls
        Class cls = Class.forName(classfullpath);
        //通过cls的newInstance()得到你加载的类Test.Cat的对象实例
        Object o = cls.newInstance();
        //通过cls得到你加载的类Test.Cat的名为methodName的方法对象
        //即：在反射种，可以把方法视为对象（万物皆对象）
        Method method = cls.getMethod(methodName);
        //通过方法对象method调用方法：即通过方法对象来调用方法
        method.invoke(o) ;//传统方法，对象.方法()，反射机制 方法.invoke(对象)

    }
}

```

# 反射机制
- 反射机制允许程序在执行期间借助于```Reflection API```取得任何类的内部信息（比如成员变量，构造器，成员方法等等），并能操作对象的属性及方法。反射在设计模式和框架底层都会用到
- 加载完类之后，在**堆**中就产生了一个```Class```类型的对象（**一个类只有一个Class对象**），这个对象包含了类的完整结构信息。通过这个对象得到类的结构。这个对象就像一面镜子，透过这个镜子看到类的结构，所以，形象的称之为：**反射**

![image.png](https://pumpkn.xyz/upload/2021/09/image-02faa1e6b36c4fcd845f990743b5c48e.png)


# Java反射机制可以完成

- 在运行时判断任意一个对象所属的类
- 在运行时构造任意一个类对象
- 在运行时得到任意一个类所具有的成员变量和方法
- 在运行时调用任意一个对象的成员变量和方法
- 生成动态代理

# 反射相关的主要类

- ```java.lang.Class``` 代表一个类，Class对象表示某个类加载后在堆中的对象。

- ```java.lange.reflect.Method``` 代表类的方法
- ```java.lang.reflect.Field``` 代表类的成员变量
- ```java.lang.reflect.Constructor``` 代表类的构造方法

# 反射优点和缺点
- 优点：可以动态的创建和使用对象（也是框架底层核心），使用灵活，没有反射机制，框架技术就失去底层支撑
- 缺点：使用反射基本是**解释执行**，对执行速度有影响

# 反射调用优化——关闭访问检查

- Method和Field、Constructor对象都有```setAccessible()```方法
- ```setAccessible()```作用是启动和禁用访问安全检查开关
- 参数为true表示反射的对象在使用时取消访问检查，提高反射效率。参数为false则表示反射的对象执行访问检查。
- 使用```setAccessible()```可以访问private修饰的成员


## 案例1
```Java
public class z2_反射爆破创建实例 {
    public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException {
        //得到Class类对象
        Class<?> cls = Class.forName("HspJava.day15.反射.Person");
        System.out.println(cls);

        //通过public无参构造器创建实例
        Person o = (Person)cls.newInstance();
        System.out.println(o);

        //通过public有参构造器创建实例
        //1.通过getConstructor()方法获得构造器对象，再通过构造器对象调用newInstance()方法创建实例
        Constructor<?> constructor = cls.getConstructor(String.class);
        Object o1 = constructor.newInstance("刘");
        System.out.println(o1);

        //通过private有参构造器创建实例
        Constructor<?> declaredConstructor = cls.getDeclaredConstructor(String.class, int.class);
        //爆破，使用反射可以访问private构造器
        declaredConstructor.setAccessible(true);
        Object lz = declaredConstructor.newInstance("lz", 21);
        System.out.println(lz);
    }
}
class Person{
    public String name = "周";

    private int age = 22 ;

    public Person() {
    }

    public Person(String name) {
        this.name = name;
    }

    private Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

```

## 案例2
```Java
public class z3_反射练习 {
    public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchFieldException, NoSuchMethodException, InvocationTargetException {
        Class<?> cls = Class.forName("HspJava.day15.反射.PrivateTest");
        //创建实例
        Object o = cls.newInstance();
        //得到name属性对象
        Field name = cls.getDeclaredField("name");
        //爆破name
        name.setAccessible(true);
        System.out.println(name.get(o));
        name.set(o , "周");
        //得到getName方法对象
        Method getName = cls.getMethod("getName");
        Object invoke = getName.invoke(o);
        System.out.println(invoke);
    }
}

class PrivateTest{
    private String name = "hellokitty" ;

    public String getName() {
        return name;
    }
}

```

# ```Class```类
- ```Class```也是类，因此继承了Object类
- ```Class```类对象不是new出来的，而是系统创建的
- 对于某个类的```Class```类对象，在内存中只有一份，因为类只加载一次
- 每个类的实例都会记得自己是由哪个```Class```实例所生成的
- 通过```Class```可以完整地得到一个类的完整结构
- ```Class```对象是存放在堆中的
- 类的字节码二进制数据，是放在**方法区**的，有的地方称为类的元数据

# ```Class```的常用方法

![image.png](https://pumpkn.xyz/upload/2021/09/image-71abd464f80e428386574dd197abba90.png)

## 案例

```Java
public class 反射练习3 {
    public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchFieldException {
        Class<?> cls = Class.forName("HspJava.day15.反射.Car");
        //输出cls，表示cls是哪个类的类对象
        System.out.println(cls); //输出class HspJava.day15.反射.Car
        //cls的运行类型
        System.out.println(cls.getClass());  //输出class java.lang.Class
        //得到报名
        System.out.println(cls.getPackage().getName()); //HspJava.day15.反射
        //得到全类名
        System.out.println(cls.getName()); //HspJava.day15.反射.Car
        //通过cls创建Car实例
        Car car = (Car) cls.newInstance();
        System.out.println(car);

        //通过反射获取属性brand
        Field brand = cls.getField("brand");
        System.out.println(brand.get(car)); //宝马
        //通过反射给属性赋值
        brand.set(car , "特斯拉");
        System.out.println(brand.get(car));  //特斯拉

        //获取所有pulic属性
        Field[] fields = cls.getFields();
        for (Field field : fields) {
            System.out.println(field.getName());
        }
        
        //获取所有属性
        Field[] declaredFields = cls.getDeclaredFields();
        for (Field declaredField : declaredFields) {
            System.out.println(declaredField.getName());
        }
    }
}

class Car{
    public String brand = "宝马";
    protected String brand2 = "宝马";
    private double price = 360000.0 ;

    public void buy(){
        System.out.println("买车了");
    }

    public void buyOne(int n){
        System.out.println("买了" + n + "辆车");
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", price=" + price +
                '}';
    }
}

```

# 获取```Class```类对象
- 前提：已知一个类的全类名，且该类在类路径下，可以通过Class类的静态方法```forName()```获取，可能抛出```ClassNotFoundException```

	- 多应用于配置文件，读取类全路径，加载类

- 前提：若已知具体类，通过类的class获取，该方式最为安全可靠，程序性能最高，例如 ```Class cls2 = Cat.class;```

	- 多用于参数传递，比如通过反射得到对应构造器对象。

- 前提： 已知某个类的实例，调用该实例的```getClass()```方法获取Class对象，实例：```Class cls3 = 对象.getClass() ```

	- 应用场景：通过创建好的对象，后去Class对象

- 其他方式

	- ClassLoader cl = 对象.getClass().getClassLoader() ;
	- Class cl2 = cl.loadClass("全类名") ;

**注意：通过各种方式得到了Class对象，其实是同一个对象**


- 基本数据(int , char , boolean , float , double ,byte , long ,short)按如下方式得到Class类对象

	- ```Class cls = 基本数据类型.class```

- 基本数据类型对应的包装类，可以通过```.type```得到Class类对象
	- ```Class cls = 包装类.TYPE```


# 哪些类有Class对象

- 外部类， 成员内部类，静态内部类，局部内部类，匿名内部类
- interface: 接口
- 数组
- enum: 枚举
- annotation : 注解
- 基本数据类型
- void 

# 类加载
反射机制是Java实现动态语言的关键，也就是通过反射实现类动态加载

- 静态加载： 编译时加载相关的类，如果没有则报错，依赖性太强
- 动态加载：运行时加载需要的类，如果运行时不用该类，则不报错，降低了依赖性

# 类加载时机

- 当创建对象是(new)
- 当子类被加载时
- 调用类中的静态成员时
- 通过反射

# 类加载过程

![image.png](https://pumpkn.xyz/upload/2021/09/image-2f6f4793915948bd9fe1b4bea20dc5bd.png)
 
# 类加载各阶段完成任务

![image.png](https://pumpkn.xyz/upload/2021/09/image-7e3ca0e1215e487385273931d08bd496.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-7f592d9e82324a539e13724278168f4f.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-3f55f306c1954a73aa56956267758309.png)

# 通过反射获取类的信息

## 第一组```java.lang.Class```类

- ```getName``` 获取全类名
- ```getSimpleName``` 获取简单类名
- ```getFields``` 获取所有public修饰的属性，包含本类以及父类的
- ```getDeclaredFields``` 获取本类中的所有属性
- ```getMethods``` 获取所有public修饰的方法，包含本类以及父类的
- ```getDeclaredMethods``` 获取本类中所有方法
- ```getConstructors``` 获取所有Public修饰的构造器，包含本类以及父类的
- ```getDeclaredConstructors``` 获取本类中所有构造器
- ```getPackage``` 以Package形式返回包信息
- ```getSuperClass``` 以Class形式返回父类信息
- ```getInterfaces``` 以Class[]形式返回接口信息
- ```getAnnotations``` 以Annotation[]形式返回注解信息

## 第二组```java.lang.reflect.Field```类

- ```getModifiers``` 以int形式返回修饰符
	
	- 默认修饰符是0，public 1 , private 2 ,protected 4 , static 8 , final 16 

- ```getType``` 以Class形式返回类型
- ```getName``` 返回属性名


## 第三组```java.lang.reflect.Constructor```类

- ```getModifiers``` 以int形式返回修饰符
- ```getName``` 返回构造器名（全类名）
- ```getParameterTypes``` 以Class[]返回参数类型数组

# 通过反射创建对象

- 方式一： 调用类中的public修饰的无参构造器
- 方式二： 调用类中的指定构造器
- ```Class```类相关方法

	- ```newInstance```: 调用类中的无参构造器，获取对应类的对象
	- ```getConstructor(Class...clazz)``` 根据参数列表，获取对应的Public构造器对象
	- ```getDecalaredConstructor(Class...clazz)``` 根据参数列表，获取对应的所有构造器

- ```Constructor```类相关方法

	- ```setAccessible``` 爆破
	- ```newInstance(Object...obj)``` 调用构造器


# 通过反射访问类中的成员

## 访问属性
- 根据属性名获取Field对象
	- Field f = clazz对象.getDeclaredField(属性名) ;

- 爆破: f.setAccessible(true); //f是Field
- 访问

	- f.set(o,值);//设置值，o表示对象
	- f.get(0) ; //获取值，o表示对象
 
**注意：如果是静态属性，则set和get中的参数o，可以写成null**

## 访问方法
- 根据方法名和参数列表获取Method方法对象

	- ```Method m = clazz.getDeclaredMethod(方法名 , XX.class);```//得到本类的所有方法

- 获取对象 : ```Object o = clazz.newInstance()```;
- 爆破: m.setAccessible(true) ;
- 访问: Object returnValue = m.invoke(o ,实参列表)

**注意：如果是静态方法，则invoke的参数o，可以写成null**

