---
title: 【Java基础】进程与线程
date: 2021-09-19 15:47:21.323
updated: 2021-09-19 15:47:21.323
url: https://pumpkn.xyz/archives/java-ji-chu--jin-cheng-yu-xian-cheng
categories: 
tags: 学习 | Java
---

# 进程概念

- 进程是指运行中的程序，比如我们使用QQ，就启动了一个进程，操作系统就会为该进程分配内存空间。当我们使用迅雷，又启动了一个进程，操作系统将为迅雷分配新的内存空间。
- 进程是程序的一次执行过程，或是正在运行的一个程序。是动态过程: 有它自身的产生、存在和消亡的过程。

```Java

public class z1_CpuNums {
    public static void main(String[] args) {
        Runtime runtime = Runtime.getRuntime();
        //获取当前电脑cpu核心数
        int cpuNums = runtime.availableProcessors();
        System.out.println("cpu可用个数=" + cpuNums );
    }
}
```


# 什么是线程

- **线程由进程创建，是进程的一个实体**
- 一个进程可以拥有多个线程。

# 其他相关概念

- 单线程：同一个时刻，只允许执行一个线程
- 多线程：同一个时刻，可以执行多个线程，比如：一个QQ进程，可以同时打开多个聊天窗口，一个迅雷进程，可以同时下载多个文件
- **并发**：同一个时刻，多个任务交替执行，造成一种“貌似同时”的错觉，简单的说，单核cpu实现的多任务就是并发。

- **并行**：同一个时刻，多个任务同时执行，多核cpu可以实现并行

# 创建线程的两种方式

- 在Java中线程的创建有两种方法

	- 继承```Thread类```，重写```run```方法
	- 实现```Runnable```接口，重写```run```方法

- 当一个类继承了Thread类，该类就可以当作线程使用
- 我们会重写run方法，写上自己的业务代码
- Thread类实现了Runnable接口的run方法


# 线程基本使用

## 继承```Thread```类

开启一个线程，该线程每隔1秒，输出喵喵，当输出40次时，结束该线程

```Java
public class z1_Thread01 {
    public static void main(String[] args) {
        Cat cat = new Cat();
        //启动线程，最终执行cat的run方法
        cat.start(); //main线程开启一个子线程
        //main开启子线程后，并不会等待子线程执行完毕后再往下执行代码，而是开启后立即执行后面的代码
        //mian线程和子线程交替执行
        for (int i = 0; i < 100; i++) {
            System.out.println(i + " 线程= " + Thread.currentThread().getName());
        }


    }
}

class Cat extends Thread{
    int times = 0 ;
    @Override
    public void run(){
        while( true ){
            System.out.println("喵喵喵" + (++times) + " 线程= " + Thread.currentThread().getName());
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            if( times >= 40 ){
                break;
            }

        }

    }

}

```

**start()方法调用start0()方法后，该线程并不一定立马执行，只是将线程变成了可运行状态。具体什么时候执行，取决于CPU，由CPU同一调度**

![image.png](https://pumpkn.xyz/upload/2021/09/image-607587735703438bb5cb6b68c04686cc.png)

## 实现```Runnable```接口

- Java是单继承的，在某些情况下一个类可能已经继承了某个父类，这时再用继承Thread类方法来创建线程显然不可能了
- 可以通过实现Runnable接口来创建线程

编写程序，改程序可以每隔1秒。在控制台输出"hi!"，当输出10次后，自动退出。请使用实现Runnable接口的方式实现

```Java
public class z2_Thread02 {
    public static void main(String[] args) {
        Dog dog = new Dog();
        //dog.statr()  这样是错的，因为Runable接口没有start方法
        //创建Thread对象，把dog对象（实现Runnable），放入Thread
        Thread thread = new Thread(dog);
        thread.start();
    }
}

class Dog implements Runnable{

    int count = 0 ;

    @Override
    public void run() {
        while( true ){
            System.out.println("汪汪汪" + (++count) + " " +Thread.currentThread().getName() );
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            if( count == 10 ){
                break ;
            }
        }

    }
}
```

# 继承```Thread``` vs 实现 ```Runnable```

- 从Java设计来看，通过继承Thread或者实现Runnable接口来创建线程本质上没有区别。从jdk文档可以看到Thread类本身就实现了Runnable接口
- 实现Runnable接口实现方式更加适合多个线程共享一个资源的情况，并且避免了单继承的限制。


## 售票系统，变成模拟三个售票窗口售票100。

```Java
public class z3_多线程售票 {
    public static void main(String[] args) {
        SaleTicks saleTicks = new SaleTicks();
        Thread thread = new Thread(saleTicks);
        Thread thread1 = new Thread(saleTicks);
        Thread thread2 = new Thread(saleTicks);
        thread.start();
        thread1.start();
        thread2.start();
    }
}

class SaleTicks implements Runnable{

    private static int count = 100 ;
    @Override
    public void run() {
        while( true ){

            if( count <= 0 ){
                break;
            }

            System.out.println(Thread.currentThread().getName() + "售票一张，还剩" + (--count));
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```

此代码没有实现线程同步，可能会导致超卖的现象。

# 线程终止

- 当线程完成任务后，会自动退出
- 还可以通过使用变量来控制run方法退出的方式停止线程，即通知方式。

# 线程常用方法

- ```setName```   设置线程名称。
- ```getName```   返回该线程的名称
- ```start```   使该线程开始执行；Java虚拟机底层调用该线程的start0方法
- ```run``` 调用线程对象run方法
- ```setPriority```   更改线程优先级
- ```getPriority``` 获取线程的优先级
- ```sleep``` 在指定的毫秒数内让当前正在执行的线程休眠(暂停执行)
- ```interrupt``` 中断线程
- ```yield``` 线程的礼让。让出cpu，让其他线程执行，但礼让的时间不确定，所以也不一定礼让成功
- ```join``` 线程的插队。插队的线程一旦插队成功，则肯定先执行完插入的线程所有的任务

# 注意细节

- start底层会创建新的线程，调用run，run就是一个简单的方法调用，不会启动新的线程
- 线程优先级的范围
![image.png](https://pumpkn.xyz/upload/2021/09/image-19aaf8b896784e27bb5845f52f82c619.png)

- interrupt，中断线程，但并没有真正的结束线程。所以一般用于中断正在休眠线程
- sleep : 线程的静态方法，使当前线程休眠

# 用户线程和守护线程

- 用户线程: 也叫工作线程，当线程的任务执行完或通知方式结束
- 守护线程： 一般是为工作现场服务的，当所有的用户线程结束，守护线程自动结束
- 常见的守护线程： 垃圾回收机制

```Java
public class z4_守护线程 {
    public static void main(String[] args) {
        T1 t1 = new T1();
        Thread thread = new Thread(t1);
        //将该线程设置为守护线程->main线程退出，守护线程就退出
        thread.setDaemon(true);
        thread.start();
        for (int i = 0; i < 10; i++) {
            System.out.println(Thread.currentThread().getName() + ".............");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class T1 implements Runnable{

    @Override
    public void run() {
        while( true ){
            System.out.println("守护线程.....");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```



## JDK中用```Thread.State```枚举表示了线程的集中状态

![image.png](https://pumpkn.xyz/upload/2021/09/image-466f1c60fb0245cf83983e13db5f0339.png)


# 线程同步机制

- 在多线程编程中，一些敏感数据不允许被多个线程同时访问，此时就使用同步访问技术，保证数据在任何时刻，最多有一个线程访问，以保证数据的完整性
- 也可以这样理解：线程同步，即当有一个线程在对内存进行操作时，其他线程都不可以对这个内存地址进行操作，直到该线程完成操作，其他线程才能对该内存地址进行操作。

# 同步方法——```sysnchronized```

- 同步代码块

```Java
   sysnchronized(对象){  //得到对象锁，才能操作同步代码
      //需要被同步的代码
   }
```

- sysnchronized还可以放在方法声明中，表示整个方法为同步方法

```Java
  public sysnchronized void m(String name){

   }
```

使用sysnchronized解决售票超卖问题

```Java
public class z5_使用同步机制售票 {
    public static void main(String[] args) {
        SaleTicks2 saleTicks = new SaleTicks2();
        Thread thread = new Thread(saleTicks);
        Thread thread1 = new Thread(saleTicks);
        Thread thread2 = new Thread(saleTicks);
        thread.start();
        thread1.start();
        thread2.start();
    }
}

class SaleTicks2 implements Runnable{

    private static int count = 100 ;

    public synchronized void sell(){
        while( count > 0 ){

            System.out.println(Thread.currentThread().getName() + "售票一张，还剩" + (--count));
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    @Override
    public void run() {
        sell();
    }
}


```

# 互斥锁

- Java中，引入了**对象互斥锁**的概念，来保证共享数据操作的完整性。
- 每个对象都对应于一个可称为**互斥锁**的标记，这个标记用来保证在任意时刻，只能有一个现场访问该对象
- 关键字```sysnchronized```来与对象的互斥锁联系。当某个对象用```sysnchronized```修饰时，表明该对象在任一时刻只能由一个线程访问
- 同步的局限性：导致程序执行效率降低
- 同步方法（非静态的）的锁可以是this,也可以是其他对象（要求是同一个对象）
- 同步方法（静态的）的锁为当前类本身

## 互斥锁注意细节

- 同步方法如果没有使用static：默认锁对象为this
- 如果方法使用static修饰，默认锁对象：当前类.class
- 实现的步骤

	- 需要先分析上锁的代码
	- 选择同步代码快或同步方法
	- 要求多个线程的所对象为同一个即可

# 线程的死锁

多个线程都占用了对象的锁资源，但不肯想让，导致了死锁，在编程是一定要避免死锁的发生。


# 下面操作不会释放锁

- 线程执行同步代码块或同步方法时，程序调用Thread.sleep()、Thread.yield()方法暂停当前线程的执行，不会释放锁

- 线程执行同步代码块时，其他线程调用了该线程的suspend()方法将该线程挂起，该线程不会释放锁。


## 练习
在main方法中启动两个线程，第一个线程循环随机打印100以内的整数，直到第2个线程从键盘读取"Q"命令

```Java
public class z6_homework1 {
    public static void main(String[] args) {
        A a = new A();
        B b = new B(a);
        Thread thread = new Thread(a);
        Thread thread1 = new Thread(b);
        thread.start();
        thread1.start();
    }
}

class A implements Runnable{

    private boolean loop = true ;

    @Override
    public void run() {
        while( loop ){
            System.out.println((int)(Math.random()*100 + 1));
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public boolean isLoop() {
        return loop;
    }

    public void setLoop(boolean loop) {
        this.loop = loop;
    }
}


class B implements Runnable{

    private A a ;

    public B(A a) {
        this.a = a;
    }

    private Scanner in = new Scanner(System.in) ;

    @Override
    public void run() {
        while( true ){
            String s = in.nextLine();
            if( "Q".equalsIgnoreCase(s) ){
                a.setLoop(false);
                break;
            }
        }

    }
}

```











