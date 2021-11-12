---
title: 【Java基础】集合
date: 2021-09-16 16:09:57.47
updated: 2021-09-19 10:47:18.537
url: https://pumpkn.xyz/archives/java基础集合
categories: 
tags: 
---

# 集合
Java的集合类很多，主要分为两大类
![image.png](https://pumpkn.xyz/upload/2021/09/image-412f09813b34411598cf1a015cd0f092.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-7d445749090046a2b5054b8514597bf8.png)

- 集合主要是两组（单列集合，双列集合）
- ```Collection```接口有两个重要的子接口```List```和```Set```，他们的实现子类都是单列集合
- ```Map```接口的实现子类是双列集合，存放K-V

## 集合整体结构
![image.png](https://pumpkn.xyz/upload/2021/09/image-da5d612c7e814ed2a8ac2f9550fb9221.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-86b24465e4c34366b4cc1e565036261d.png)

# ```Collection```接口和常用方法

## ```Collection```接口实现类的特点

- ```Collection```实现子类可以存放多个元素，每个元素可以是```Object```
- 有些```Collection```的实现类，可以存放重复的元素，有些不可以
- ```Collection```的实现类，有些是有序的(List),有些是无序的(Set)
- ```Collection```接口没有直接的实现子类，是通过它的子接口```Set```和```List```来实现的。

## ```Collection```常用方法

- ```add```： 添加单个元素
- ```remove```： 删除指定元素
- ```contains```： 查找元素是否存在，返回boolean值
- ```size```: 获取元素个数
- ```isEmpty```: 判断是否为空
- ```clear```: 清空
- ```addAll```: 添加多个元素
- ```containsAll```: 查找多个元素是否都存在
- ```removeAll```: 删除多个元素

以实现子类```ArrayList```来演示

```Java
@SuppressWarnings({"all"})
public class Test {
    public static void main(String[] args) {
        Collection col = new ArrayList<>();
        col.add("周") ;
        col.add("刘") ;
        col.add(100) ;
        col.add(new Integer(3)) ;

        System.out.println(col); //[周, 刘, 100, 3]

        col.remove(3) ;
        System.out.println(col); //[周, 刘, 100]
        boolean b = col.contains("周");
        System.out.println(b);  //true

        System.out.println(col.size());  //3
        System.out.println(col.isEmpty()); //false
        col.clear();
        System.out.println(col.size());  // 0

        Collection c = new ArrayList() ;
        c.add("AA");
        c.add("CC");
        c.add("BB");
        col.addAll(c) ;
        System.out.println(col);

        Collection c2 = new ArrayList() ;
        c2.add("CC");
        c2.add("AA");
        boolean b1 = col.containsAll(c2);
        System.out.println(b1); //true

        col.removeAll(c2) ;
        System.out.println(col);
    }
}

```

## ```Collection```接口遍历元素方式1——```Iterator```迭代器

- ```Iterator```对象称为迭代器，主要用于遍历Collection集合中的元素。
- 所有实现了```Collection```接口的集合类都有一个```iterator()```方法，用以返回一个实现了Iterator接口的对象，即可以返回一个迭代器
- ```Iterator```仅用于遍历集合，```Iterator```本身并不存放对象。


### 迭代器执行原理

得到一个集合的迭代器
```Java
Iterator iterator = col.iterator() ;
```
- ```iterator.hasNext()```: 判断是否还有下一个元素
- ```iterator.next()```: 作用

	- 下移
	- 将下移以后集合位置上的元素返回


![image.png](https://pumpkn.xyz/upload/2021/09/image-7e23551a181b43d598883c1852a35e0c.png)


### 提示
在调用```iterator.next()```方法之前必须要调用```iterator.hasNext()```进行检测。若不调用，且下一条记录无效，直接调用it.next()会抛出```NoSuchElementException```异常


```Java
@SuppressWarnings({"all"})
public class 迭代器 {
    public static void main(String[] args) {
        Collection col = new ArrayList() ;
        col.add("周") ;
        col.add("刘") ;
        col.add("起") ;
        col.add("在") ;

        //要遍历col，先得到对应的迭代器
        Iterator iterator = col.iterator();
        //快捷键 快速生成 itit
        //显示所有快捷操作的快捷键 ctrl + j
        while (iterator.hasNext()) {
            String next = (String)iterator.next();
            System.out.println(next);
        }

        System.out.println("=========================");
        //如果希望再次遍历，需要重置迭代器
        iterator = col.iterator() ;
        while (iterator.hasNext()) {
            Object next = iterator.next();
            System.out.println(next);
        }
    }
}
```

## ```Collection```接口遍历对象方式2——增强for循环

增强for循环，可以替代iterator迭代器，特点：增强for就是简化版的iteartor，本质一样。只能用于遍历集合或数组

### 基本语法

```Java
   for( 元素类型 元素名 : 集合名或数组名 ){
         访问元素
   }
```


# ```List```接口和常用方法

## ```List```接口基本介绍
List接口是Collection接口的子接口

- List集合类中元素有序（即添加顺序的取出顺序一致）、切可重复
- List集合中的每个元素都有其对应的顺序索引，即支持索引
- List容器中的元素都对应一个整数型的序号记载其在容器中的位置，可以根据序号存取容器中的元素
- JDK API中List接口的实现类常用的有:

	- ArrayList
	- LinkedList
	- Vector 

```Java
@SuppressWarnings({"all"})
public class List常用方法 {
    public static void main(String[] args) {
        List list = new ArrayList() ;
        list.add("jack") ;
        list.add("tom") ;
        list.add("mary") ;
        //List接口中的每个元素都有其对应的顺序索引，即List接口支持索引
        //索引从0开始
        System.out.println(list.get(1));

    }
}
```

## ```List```接口的常用方法

List集合里添加了一些根据索引来操作集合元素的方法

- ```void add(int index , Object ele) ```

	- 在index位置插入ele元素

- ```boolean addAll(int index , Collection eles)```

	- 从index位置开始将eles中的所有元素添加进来

- ```Object get(int index)```

	- 获取制定index位置的元素

- ```int indexOf(Object obj)```

	- 返回obj在当前集合中首次出现的位置

- ```int lastIndexOf(Object obj)```

	- 返回obj在当前集合中最后出现的位置

- ```Object remove(int index)```

	- 移除指定index位置的元素，并返回此元素（默认按索引删除，按指定元素删除时，需要传入对象才行）

- ```Object set(int index , Object ele)```

	- 设置指定index位置的元素为ele，相当于是替换

- ```List subList(int fromIndex , int toIndex)```

	- 返回从fromIndex到toIndex位置的子集合 

## List接口练习

使用List的实现类添加几本书，并遍历。要求按价格排序，从低到高（使用冒泡），并且使用ArrayList、LinkedList、Vector三种集合实现

```Java
@SuppressWarnings({"all"})
public class z1_test {
    public static void main(String[] args) {
        List list = new ArrayList();
        list.add(new Book("福尔" , 10 , "柯南道尔")) ;
        list.add(new Book("摩斯" , 80 , "柯南道尔")) ;
        list.add(new Book("华生" , 20 , "柯南道尔")) ;
        list.add(new Book("琼斯" , 15 , "柯南道尔")) ;


        sort(list) ;
        for (Object o : list) {
            System.out.println(o);
        }
    }
    //冒泡排序
   public static List sort(List list){
       int size = list.size();
       for( int i = 0 ; i < size-1 ; i ++ ){
           for( int j = 0 ; j < size-1-i ; j++ ){
               Book book = (Book) list.get(j);
               Book book1 = (Book) list.get(j + 1);
               if( book.getPrice() > book1.getPrice() ){
                    list.set(j , book1) ;
                    list.set(j+1 , book) ;
               }
           }
       }
       return list ;
   }
}

class Book{
    private String name ;
    private double price ;
    private String author ;

    public Book(String name, double price, String author) {
        this.name = name;
        this.price = price;
        this.author = author;
    }

    @Override
    public String toString() {
        return "名称: " + name +"\t\t价格: " + price + "\t\t作者: " + author ;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }
}
```

# ```ArrayList```底层结构

- ```ArrayList```可以加入```null```，并且可以多个
- ```ArrayList```是由**数组**来实现数据存储的
- ```ArrayList```基本等同于```Vector```，除了```ArrayList```是线程不安全（执行效率高），在多线程情况下，不建议使用```ArrayList```


# ```ArrayList```源码分析

- ```ArrayList```中维护了一个Object类型的数组elementData

	- transient Object[] elementData ; //transient表示瞬间的，短暂的，表示该属性不能被序列化


- 当创建ArrayList对象时，如果使用的是无参构造器，则初始elementData容量为0，第1次添加，则扩容elementData为10，如需要再次扩容，则扩容elementData为1.5倍
- 如果使用的是指定大小的构造器，则初始elementData容量为指定大小，如果需要扩容，则直接扩容elemetData为1.5倍

# ```Vector```底层结构

- vector底层也是一个对象数组，protected Object[] elementData ;
- Vector是线程同步的，即**线程安全的**，Vector类的操作方法带有```synchronized```关键字
- 在开发中，需要线程同步安全时，考虑使用Vector


# ```Vector```和```ArrayList```的比较

||底层结构|版本|线程安全，效率|扩容倍数|
|-------|-------|-------|-------|-------|
|ArrayList|可变数组|jdk1.2|不安全，效率高|如果有参构造 1.5倍。如果是无参，第一次10，从第二次开始按1.5倍扩容|
|Vector|可变数组|jdk1.0|安全，效率不高|如果是无参，默认10，满后，就按2倍扩容。如果指定大小，则每次直接按2倍扩容|



# ```LinkedList```底层结构

## ```LinkedList```全面说明

- ```LinkedList```底层实现了**双向链表**和**双端队列**的特点
- 可以添加任意元素(元素可以重复)，包括null
- **线程不安全，没有实现同步**

## ```LinkedList```的底层操作机制

- ```LinkedList```底层维护了一个**双向链表**
- ```LinkedList```中维护了两个属性```first```和```last```分别指向首节点和尾节点
- 每个节点(Node对象)，里面又维护了prev、next、item三个属性，其中通过prev指向前一个，通过next指向后一个节点。最终实现双向链表。
- 所以LinkedList元素的**添加和删除**，不是通过数组完成的，相对来说效率较高。

 ![image.png](https://pumpkn.xyz/upload/2021/09/image-19327f5aad0244a99f89faa4f0b117af.png)
 
 
## ```ArrayList```和```LinkedList```比较

||底层结构|增删的效率|改查的效率|
|-------|-------|-------|-------|
|ArrayList|可变数组|较低，数组扩容|较高|
|LinkedList|双向链表|较高，通过链表追加|较低|

## 如何选择```ArrayList```和```LinkedList```

- 如果我们改查的操作多，选择```ArrayList```
- 如果我们增删的操作多，选择```LinkedList```
- 一般来说，在程序中，80%-90%都是查询，因此大部分情况下会选择```ArrayList```
- 在一个项目中，根据业务灵活选择，也可能这样，一个模块使用的是```ArrayList```，另外一个模块是```LinkedList```

# Set接口和常用方法
 
## ```Set```接口基本介绍

 - 无序（添加和取出的顺序不一致），**没有索引**
 - 取出的顺序虽然与添加的顺序不同，但是他是固定的
 - 不允许重复元素，所以最多包含一个```null```
 - JDK API中Set接口的实现类有：
 	- ```HashSet```
 	- ```TreeSet```

## ```Set```接口的常用方法
和List接口一样，Set接口也是Collection的子接口，因此，常用方法和Collection接口一样。

## ```Set```接口的遍历方式
同Collection的遍历方式一样，因为Set接口是Collection接口的子接口

- 可以使用迭代器
- 增强for
- **不能使用索引的方式来获取**


## ```HashSet```——```Set```接口实现类

- ```HashSet```实现了```Set```接口
- ```HashSet```实际上是```HashMap```，看源码
		
	- ![image.png](https://pumpkn.xyz/upload/2021/09/image-1501c3342a0c45089b5ab52d666af244.png)

- 可以存放```null```值，但是只能有一个```null```
- ```HashSet```不保证元素是有序的，取决于hash后，再确定索引的结果
- 不能有重复元素/对象。


```Java
@SuppressWarnings({"all"})
public class 提出问题 {
    public static void main(String[] args) {
        Set hashSet = new HashSet<>();
        hashSet.add(new Dog("邦邦")) ; //能够添加
        hashSet.add(new Dog("邦邦")) ; //能够添加
        System.out.println(hashSet); //输出 [Dog{name='邦邦'}, Dog{name='邦邦'}]

        hashSet.clear();
        hashSet.add(new String("zf"));  //添加
        hashSet.add(new String("zf")); //没有添加
        //这是为什么？去看一下源码，调用add()到底发生了什么？
        System.out.println(hashSet); //输出 [zf]
    }
}

class Dog{
    String name ;

    public Dog(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                '}';
    }
}
```


## ```HashSet```底层机制说明

```HashSet```的底层就是```HashMap```，```HashMap```底层是（数组+链表+红黑树）。</br>
```HashSet```底层将数据存储在```table```数组里
![image.png](https://pumpkn.xyz/upload/2021/09/image-dae0e47e4da346d49926a93448661622.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-d9fe786c35b1491086c24aafe66d1562.png)

## <font color="red">```HashSet```扩容与红黑树化机制 </font>

- ```HashSet```底层是```HashMap```
- 添加一个元素时，先得到其hash值，然后将hash值转成索引值
- 找到存储数据表table，看这个索引位置是否已经存放了元素

	- 如果没有存放，直接加入
	- 如果已经存放了元素，条用```equals```比较，如果相同，则放弃添加。如果不相同，则添加到该索引连接的链表最后。

- ```HashSet```底层是```HashMap```，第一次添加时，```table```数组扩容到16，临界值(```threshold```)是16*加载因子(```loadFactor```)是0.75 = 12
- 如果```table```数组里存储的元素（包括链表上的元素）个数到了临界值12或者单个链表上挂载了超过```TREEIFY_YHRESHOLD```(默认是8)个元素，就会扩容到```16*2=32```，新的临界值就是```32*0.75=24```，依次类推
- 在Java8中，如果一条链表的元素个数到达```TREEIFY_YHRESHOLD```(默认是8)，并且table的大小(包括链表上的元素) >= ```MIN_TREEIFY_CAPACITY```(默认64)，就会进行树化（红黑树），否则仍然采用数组扩容机制。
- 红黑树化后，仍然遵守扩容机制，即若table中的元素个数(包括链表上的元素) > 临界值，table就会按2倍扩容


![image.png](https://pumpkn.xyz/upload/2021/09/image-56bc3912e5e24b409b453f1ef9cfe1fc.png)


## ```LinkedHashSet```——```Set```接口实现类

- ```LinkedHashSet```是```HashSet```的子类
- ```LinkedHashSet```是有序的，即添加顺序和输出顺序一致。
- ```LinkedHashSet```底层是一个```LinkedHashMap```，底层维护了一个数组+双向链表
- ```LinkedHashSet```根据元素的```HashCode```值来决定元素的存储位置，同时使用链表来维护元素的次序，这使得元素看起来是以插入顺序保存的。
- ```LinkedHashSet```不允许添加重复的元素

![image.png](https://pumpkn.xyz/upload/2021/09/image-aad2c888414245c3be384f55a47bf0d5.png)


## ```TreeSet```

- 当我们使用无参构造器，创建TreeSet时，默认是升序的
- 当希望添加的元素，按照特点的规则来排序时，```TreeSet```提供了一个构造器，可以传入一个比较器
- ```TreeSet```也不存储重复元素

```Java
@SuppressWarnings({"all"})
public class TreeSet_ {
    public static void main(String[] args) {
        TreeSet<String> treeSet = new TreeSet<>(new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o2.compareTo(o1); //字符串降序
            }
        });
        treeSet.add("zf");
        treeSet.add("lx");
        treeSet.add("lf");
        treeSet.add("zx");
        System.out.println(treeSet);
    }
}
```

# ```Map```接口和常用方法

## ```Map```接口实现类的特点

*注意：这里讲的是jdk8的Map接口特点*

- ```Map```与```Collection```并列存在。用于保存具有映射关系的数据:```Key-Value```
- ```Map```中的key和value可以是**任何引用类型**的数据，会封装到```HashMap$Node```对象中
- ```Map```中的key不允许重复，**原因和```HashSet```一样**
- ```Map```中的value可以重复
- ```Map```的key可以为null,value也可以为null，注意key为null，只能有一个。value为null，可以有多个
- 常用```String```类作为```Map```的key
- key和value之间存在单向的一对一关系，即通过指定的key总能找到对应的value
- ```Map```存放数据的```key-value```示意图，一堆```k-v```是放在一个```Node```中的，又因为```Node```实现了```Entry```接口，有些书上说一对```k-v```就是一个```Entry```


![image.png](https://pumpkn.xyz/upload/2021/09/image-686a915134c94b69835e7841d78f6d95.png)


## 重要细节

- k-v为了方便遍历，还会创建```Entry```集合，该集合存放的元素类型为```Entry```，而一个```Entry```对象就有k-v, 即```Set<Map.Entry<K,V>> entrySet``` 
- ```entrySet```中，定义的类型是```Map.Entry```，但是实际上还是指向```HashMap$Node```
- 把```HashMap$Node```对象存放到```entrySet```就方便遍历，因为```Map.Entry```提供了重要的方法 

	- ```K getKey()```
	- ```V getValue()```


![image.png](https://pumpkn.xyz/upload/2021/09/image-24ad02527a104dd2ba50d83866a3fd77.png)

## ```Map```接口的常用方法

- ```put```: 添加
- ```remove``` : 根据键删除映射关系
- ```get```： 根据键获取值
- ```size```: 获取元素个数
- ```isEmpty```: 判断个数是否为0
- ```clear```: 清空
- ```containsKet```: 查找键是否存在
- ```KeySet```: 获取所有键的集合
- ```values```: 获取所有的值
- ```entrySet```: 获取所有的关系k-v

![image.png](https://pumpkn.xyz/upload/2021/09/image-a6471b685b814c26918c523c7b300fc0.png)


## ```HashMap```——```Map```接口实现类

- ```Map```接口的常用实现类:```HashMap```、```Hashtable```和```Properties```
- ```HashMap```是```Map```接口使用频率最高的实现类
- ```HashMap```是以键值对的方式来存储数据(```HashMap$Node```类型)
- ```key```不能重复，但是值可以重复，允许使用null键和null值
- 如果添加相同的key,则会覆盖原来的k-v，等同于修改（key不会替换，val会替换）
- 与```HashSet```一样，不保证映射的顺序，因为底层是以```hash表```的方式来存储的。（JDK8的```HashMap```底层 数组+链表+红黑树）
- ```HashMap```没有实现同步，因此是**线程不安全**的，方法没有做同步互斥的操作，没有```syschronized```

```HashMap```底层示意图：

![image.png](https://pumpkn.xyz/upload/2021/09/image-446a7d568ce04e0f81e0ad82c15a49df.png)

## ```HashMap```底层解析
扩容机制与```HashSet```相同

- ```HashMap```底层维护了Node类型的数组table，默认为null
- 当创建对象时，将加载因子(loadFactor)初始化为0.75
- 当添加```key-value```时，通过```key```的**哈希值**得到在table的索引。然后判断该索引处是否已存储元素，如果没有存储元素则直接添加。如果该索引已经存有元素，继续判断已存元素的key和准备加入的key是否相等，如果相等，则直接替换val；如果不相等需要判断是树结构还是链表结构，做出相应的处理。如果添加时发现容量不够，则需要扩容
- **第一次添加，则需要扩容table容量为16，临界值为(```threshold```)为12(16*0.75)**
- 以后再扩容，则需要扩容table容量为原来的2倍，零界值为原来的2倍，依次类推
- 在Java8中，如果一条链表的元素个数超过```TREEIFY_THRESHOLD```（默认时8），并且table的大小 >= ```MIN_TREEIFY_CAPACITY```(默认64)，就会进行树化（红黑树）

## ```HashMap```源码解读

```Java
public class HashMap源码解读 {
    public static void main(String[] args) {
        HashMap<Object, Object> map = new HashMap<>();
        for (int i = 0; i < 10000; i++) {
            map.put(new Cat() , null) ;
        }
    }
}

class Cat{
    @Override
    public int hashCode() {
        return 100 ;
    }
}
```

## 底层执行解读
```Java
/*
	1.执行构造器new HashMap()
	  初始化加载因子loadfactor = 0.75
          HashMap$Node[] table = null
        2. 执行put调用hash方法，计算key的hash值(h=key.hashCode())^(h >>> 16)
*/
        public V put(K key, V value) {
            return putVal(hash(key), key, value, false, true);
    }

//2.1 putVal(中的hash(key)源代码),根据key的哈希值计算索引
//>>>表示无符号右移，也叫逻辑右移，即若该数为正，则高位补0，而若该数为负数，则右移后高位同样补0
   static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }

//3. 执行putVal方法，下面为putVal源码跟我的理解
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i; //辅助变量
	//如果底层的table数组为null，或者length=0，就扩容到16
        if ((tab = table) == null || (n = tab.length) == 0)
	    //resize()方法为扩容方法
            n = (tab = resize()).length;
        //取出hash值对应的table的索引位置Node，如果为null，则直接加入k-v
        if ((p = tab[i = (n - 1) & hash]) == null)
            //创建一个新Node，加入该位置即可
	    tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            //如果table的索引位置的key的hash值和待加入的key的hash值相同
	    //并且满足(table现有的节点的key和待加入的key是同一个对象||equals返回真)
            //就认为不能加入新的k-v
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
           //如果当前的table的已有的Node是红黑树，就按照红黑树的方式添加
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                //如果找到的节点，后面是链表，就循环比较
                for (int binCount = 0; ; ++binCount) {  //死循环
                    //如果整个链表，没有和待加入的key相同的，就加入到该链表的最后
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
		 	//加入后，判断当前链表的个数，是否已经到8个，到8个后，就调用treeifyBin方法进行红黑树的转换
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
  		//如果在循环比较过程中，发现有相同，就break，就只是替换value
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
	//没增加一个Node，就size++
        ++modCount;
	//如size > 临界值，就扩容
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```


## ```Hashtable```——```Map```接口实现类

- 存放的元素时键值对: 即```key-value```
- ```hashtable```的**键和值都不能为null**
- ```hashtable```使用方法基本上和```HashMap```一样
- ```hashtable```是**线程安全**的，```hashMap```是**线程不安全的**

## ```hashtable```底层机制

- 底层有数组```Hashtable$Entry[]```，初始化大小为11
- 临界值```threshold``` 8 =  11 * 0.75
- 扩容：按照自己的扩容机制来进行即可
- 执行方法 ```addEntry(hash , key , value ,index) ; 添加```k-v```封装到```Entry```
- 当```if(count >= threshold)```满足时，就进行扩容
- 按照```int newCapacity = (oldCapacity << 1) + 1的规则扩容，即2倍+1。


## ```Hashtable```和```HashMap```对比

|column1|版本|线程安全(同步)|效率|允许null键null值|
|-------|-------|-------|-------|-------|
|HashMap|1.2|不安全|高|可以|
|Hashtable|1.0|安全|低|不可以|


## ```Properties```——```Map```接口实现类

- ```Properties```类继承自```Hashtable```类并且实现了```Map```接口，也是使用一种键值对的形式来保存数据
- 他的使用特点和```Hashtable```类似
- ```Properties```还可以用于从```xxx.properties```文件中，加载数据到```Properties```类对象，并进行读取和修改
- 说明：工作后```xxx.properties```文件通常作为配置文件。


### 基本使用
```Java
public class z1 {
    public static void main(String[] args) throws IOException {
        Properties properties = new Properties();
        //加载properties文件
        properties.load(new FileReader("src\\mysql.properties"));
        //获取K-V
        String user = properties.getProperty("user");
        String pwd = properties.getProperty("pwd");
        System.out.println(user + "   " + pwd);

        //修改K-V，若K不存在，则等于添加
        properties.setProperty("user" , "xiaomi") ;
        properties.setProperty("max" , "200") ;
        System.out.println(properties.getProperty("user")  + "  " + properties.getProperty("max"));
    }
}
```

# 总结——开发中如何选择集合实现类
在开发中，选择什么集合实现类，主要取决于业务操作特点，然后根据集合实现类特性进行选择

- 先判断存储的类型（一组对象[单列]|一组键值对[双列]）
- 一组对象[单列]: ```Collection```

	- 允许重复: ```List```
	
		- 增删多:  ```linkedList```[底层双向链表]
		- 改查多： ```ArrayList```[底层为Object类型可变数组]

	- 不允许重复: ```Set```
	
		- 无序: ```HashSet```[底层是HashMap，维护一个哈希表(即数组+链表+红黑树)]
		- 排序: ```TreeSet```
		- 插入和取出顺序一致: ```LinkedHashSe```

- 一组键值对[双列] : ```Map```

	- 键无序: ```HashMap```[底层:哈希表jdk7为数组+链表，jdk8为数组+链表+红黑树]
	- 键排序:```TreeMap```
	- 键插入和取出顺序一致:```LinkedHashMap```
	- 读取文件: ```Properties``` 


# ```Collections```工具类

- ```Collections```是一个操作Set、List和Map等集合的工具类
- ```Collections```中提供了一系列静态方法对集合元素进行排序、查询和修改等操作

## 排序操作（均为static方法）

- ```reverse(List)```: 反转List中元素的顺序
- ```shuffle(List)```: 对List集合元素进行随机排序
- ```sort(List)```: 根据元素的自然顺序对指定List集合元素按升序排序
- ```sort(List,Comparator)```: 根据指定的Comparator产生的顺序对List集合元素进行排序，通常使用匿名内部类
- ```swap(List,int,int)```: 将指定list集合中的i处元素和j处元素进行交换


## 查找，替换

- ```Object max(Collection)```: 根据元素的自然顺序，返回给定集合中的最大元素
- ```Object max(Collection ,Comparator)```: 根据comparator指定的顺序，返回给定集合中的最大元素
- ```Object min(Collection)```
- ```Object min(Collection , Comparator)```
- ```int frequency(Colleciton , Object)```: 返回指定集合中指定元素的出现次数
- ```void copy(List dest , List src)```: 将src中的内容复制到dest中
- ```boolean replaceAll(List list , Object oldVal , Object newVal)```: 使用新值替换List对象的所有旧值




