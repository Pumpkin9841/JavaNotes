---
title: 【Java基础】文件
date: 2021-09-19 20:17:48.581
updated: 2021-09-19 20:18:51.701
url: https://pumpkn.xyz/archives/java基础文件
categories: 
tags: 学习 | Java
---

# 文件

## 文件流
文件在程序中是以流的形式来操作的
![image.png](https://pumpkn.xyz/upload/2021/09/image-e7990234e0894e96a5479eceed0806fc.png)

## 流
数据在数据源（文件）和程序（内存）之间经历的路径

- **输入流**：数据从数据源（文件）到程序（内存）的路径
- **输出流**：数据从程序（内存）到数据源（文件）的路径

# 常用的文件操作

## 创建文件对象相关构造器和方法

- ```new File(String path)``` 根据路径构建一个File对象
- ```new File(File parent , String child)``` 根据父目录文件+子路径构建
- ```new File(String parent , String child)``` 根据父目录+子路径构建
- ```createNewFile``` 创建新文件

![image.png](https://pumpkn.xyz/upload/2021/09/image-773b58cc85b2461480c5663f3119bfc1.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-1d52ffff042f43f69417d16bc6be8cc8.png)

## 获取文件的相关信息

- ```getName```
- ```getAbsolutePath```
- ```getParent```
- ```length```
- ```exists```
- ```isFile```
- ```isDirectory```

![image.png](https://pumpkn.xyz/upload/2021/09/image-0d096d6f8f774ab680a221af47d5b68a.png)


## 目录的操作和文件删除
- ```mkdir``` 创建一级目录
- ```mkdirs``` 创建多级目录
- ```delete``` 删除空目录或文件

![image.png](https://pumpkn.xyz/upload/2021/09/image-c0abc3dba1914cae9e0c9430001b1c85.png)

# IO流原理及流的分类

- ```I/O```是Input/Output的缩写，```I/O```技术是非常实用的技术，用于处理数据传输。如读/写文件，网络通讯等
- Java程序中，对于数据的输入/输出操作以"流(stream)"的方式进行。
- ```java.io```包下提供了各种"流"类和接口，用以获取不同种类的数据，并通过方法输入或输出数据
- 输入input: 读取外部数据（磁盘、光盘等存储设备的数据）到程序（内存）中。
- 输出output: 将程序（内存）数据输出到磁盘、光盘等存储设备中

# 流的分类

- 按操作数据单位不同分为

	- 字节流（8 bit）二进制文件
	- 字符流（按字符）文本文件

- 按数据流的流向不同分为

	- 输入流
	- 输出流

- 按流的角色不同分为
	- 节点流
	- 处理流/包装流

|抽象基类|字节流|字符流|
|-------|-------|-------|
|输入流|InputStream|Reader|
|输出流|OutputStream|Writer|

- Java的IO流共涉及40多个类，实际上都是从如上4个抽象基类派生的
- 由这4个类派生出来的子类名称都是以其父类名作为子类名后缀

![image.png](https://pumpkn.xyz/upload/2021/09/image-3230be943cab492ab2fd25378871d88c.png)

# ```InputStream```: 字节输入流

<font color="red"> **```InputStream```**抽象类是所有**字节输入流**的**超类**
</font>

## ```InputStream```常用的子类

- ```FileInputStream``` 文件输入流
- ```BufferedInputStream``` 缓冲字节输入流
- ```ObjectInputStream``` 对象字节输入流

## ```FileInputStream```的使用

```Java
public class z1_FileInputStem {
    java.lang.String filePath = "D:\\z1.txt" ;
    /**
     * fileInputStream.read()
     * 每次读取一个字节，若文件为空，则阻止该方法。
     * 若读到文件末尾，返回-1
     * */
    @Test
    public void readFile(){
        int readData ;
        try {
            FileInputStream fileInputStream = new FileInputStream(filePath);
            while( ( readData = fileInputStream.read() ) != -1 ){
                System.out.print((char)readData);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 定义一个byte数组
     * fileInputStream.read(bytes)
     * 每次最多读取byte数组长度的字节，
     * 返回读取的字节数
     * 文件末尾返回-1
     * */
    @Test
    public void readFileLength(){
        byte[] bytes = new byte[8] ;
        int readLen ;
        try {
            FileInputStream fileInputStream = new FileInputStream(filePath);
            while(  ( readLen =  fileInputStream.read(bytes) ) != -1 ){
                System.out.println(new  java.lang.String(bytes , 0 , readLen));
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}


```

## ```FileOutputStream```的使用

```Java
public class z2_FileOutputStem {

    @Test
    public void write(){
        String filePath = "D:\\a.txt" ;
        FileOutputStream fileOutputStream = null ;
        try {
            //fileOutputStream = new FileOutputStream(filePath) 覆盖文件原有内容
            //fileOutputStream = new FileOutputStream(filePath , true) 每次追加在文件内容后边
            fileOutputStream = new FileOutputStream(filePath , true) ;
            String str = "hello world" ;
            fileOutputStream.write(str.getBytes());
            //读取str的[0,3)范围的字节
            fileOutputStream.write(str.getBytes() , 0 , 3);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                fileOutputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

}

```

## 文件拷贝

```Java
public class z3_文件拷贝 {
    public static void main(String[] args) throws IOException {
        String srcFilePath = "D:\\zaizai.jpg" ;
        String destFilePath = "D:\\zhouzhou.jpg" ;
        FileInputStream fileInputStream = null ;
        FileOutputStream fileOutputStream = null ;

        try {
            fileInputStream = new FileInputStream(srcFilePath) ;
            fileOutputStream = new FileOutputStream(destFilePath) ;
            byte[] bytes = new byte[1024] ;
            int readLen  ;
            while( (readLen = fileInputStream.read(bytes)) != -1 ){
                fileOutputStream.write(bytes , 0 , readLen);
            }

        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            if( fileInputStream != null ){
                fileInputStream.close();
            }
            if( fileOutputStream != null ){
                fileOutputStream.close();
            }
        }

    }
}

```

# ```FileReader```和```FileWriter```类

```FileReader```和```FileWriter```是字符流，即按照字符来操作io

![image.png](https://pumpkn.xyz/upload/2021/09/image-d8f15f9e6f994716921ada1077cbf7c8.png)


## ```FileReader```相关方法

- ```new FileReader(File/String)```
- ```read``` 每次读取单个字符，返回该字符，如果到文件末尾返回-1
- ```read(char [])``` 批量读取多个字符到数组，返回读取到的字符数，如果到文件末尾返回-1

## 相关API
- ```new String(char[])``` 将char[]转换成String
- ```new String(char[] , off, len)``` 将char[]指定部分转换成String

```Java
public class z1_FileReader {

    /**
     * fileReader.read()
     * 每次读取一个字符
     * */
    @Test
    public void FileRead(){
        String filePath = "D:\\stroe.txt" ;
        FileReader fileReader = null ;
        try {
            fileReader = new FileReader(filePath);
            int data ;
            while( (data = fileReader.read()) != -1 ){
                System.out.print((char)data);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if( fileReader != null ){
                try {
                    fileReader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }

    @Test
    public void fileReadMore(){
        String filePath = "D:\\stroe.txt" ;
        FileReader fileReader = null ;

        try {
            fileReader = new FileReader(filePath) ;
            int readLen ;
            char[] chars = new char[1024] ;
            while( (readLen = fileReader.read(chars)) != -1 ){
                String s = new String(chars, 0, readLen);
                System.out.print(s);
            }

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        finally {
            if( fileReader != null ){
                try {
                    fileReader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

}
```


## ```FileWriter```相关方法
- ```new FileWriter(File/String)``` 覆盖模式，相当于流的指针在首端
- ```new FileWriter(File/String, true)``` 追加模式，相当于流的指针在尾端
- ```write(int)``` 写入单个字符
- ```write(char[])``` 写入指定数组
- ```write(char[] , off , len)``` 写入指定数组的指定部分
- ```write(String)``` 写入整个字符串
- ```write(String , off , len)``` 写入字符串的指定部分

## 相关API
- ```String类```: 
	
	- ```toCharArray```: 将String转换成char[]

## **注意：FileWriter使用后，必须要关闭(close)或者刷新(flush)，否则写入不到指定文件！**

# 节点流和处理流

## 基本介绍
- 节点流可以从一个特定的数据源读写数据，如FileReader、FileWriter
- 处理流（也叫包装流）是“连接”在已存在的流（节点流或处理流）之上，为程序提供更为强大的读写功能，如BufferedReader、BufferedWriter


# 节点流和处理流的区别和联系

- 节点流是底层流/低级流，直接跟数据源相接
- 处理流（包装流）包装节点流，既可以消除不同节点流的实现差异，也可以提供更方便的方法来完成输入输出。
- 处理流（也叫包装流）对节点流进行包装，使用了修饰器设计模式，不会直接与数据源相连

# 处理流的功能主要体现在

- 性能的提高：主要以增加**缓冲**的方式来提高输入输出的效率
- 操作的便捷：处理流提供了一系列便捷的方法来一次输入输出大批量的数据，使用更加灵活方便

# ```BufferedReader```和```BufferedWriter```——字符处理流

- ```BufferedReader```和```BufferedWriter```属于字符流，是按照字符来读取文件的
- 关闭处理流时，只需要关闭外层流即可

##  ```BufferedReader```的使用
```Java
public class z1_BufferdReader {
    public static void main(String[] args) throws IOException {
        String filePath = "D:\\stroe.txt" ;
        BufferedReader bufferedReader = new BufferedReader(new FileReader(filePath));
        String line ;
        while( (line = bufferedReader.readLine()) != null ){
            System.out.println(line);
        }
        bufferedReader.close();
    }
}

```

## ```BufferedWriter```
```Java
public class z2_BufferedWriter {
    public static void main(String[] args) throws IOException {
        String filePath = "D:\\bufferedWriter.txt" ;
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(filePath , true));
        bufferedWriter.write("hello1 , world !");
        bufferedWriter.newLine(); //写入一个与系统符合的换行符
        bufferedWriter.write("hello2 , world !");
        bufferedWriter.newLine();
        bufferedWriter.write("hello3 , world !");

        bufferedWriter.close();
    }
}
```


# ```BufferedInputStream```和```BufferedOutputStream```——字节处理流

- ```BufferedInputStream```是字节流，在创建```BufferedInputStream```时，会创建一个内部缓冲区数组。

- ```BufferedOutputStream```是字节流，实现缓冲的输出流，可以将多个字节写入底层输出流中，而不必对每次字节写入调用底层系统

# ```ObjectInputStream```和```ObjectOutputStream```——对象流

## 看一个需求
将int num = 100 这个int数据保存到文件中，注意不是100这个数字，而是int 100 ， 并且能够从文件从直接恢复int 100</br>
将```Dog dog = new Dog("小黄" , 3)```这个dog对象保存到文件中，并且能够从文件中恢复</br>
上面的要求，就是能够将基本数据类型或者对象进行**序列化**和**反序列化**操作

# 序列化和反序列化
- 序列化就是在保存数据时，保存数据的值和数据类型
- 反序列化就是在恢复数据时，恢复数据的值和数据类型
- 需要让某个对象支持序列化机制，则必须让其类时可序列化的，为了让某个类是可序列化的，该类必须实现如下两个接口之一
	
	- ```Serializable``` // 这是一个标记接口
	- ```Externalizable```


## ```ObjectOutputStream```的使用

```Java
public class z3_ObjectOutputStream {
    public static void main(String[] args) throws IOException {
        String filePath = "D:\\objectOutputStream.dat" ;
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filePath)) ;
        oos.writeInt(100);
        oos.writeBoolean(true);
        oos.writeUTF("周");
        oos.writeObject( new Dog("邦邦" , 3) );
        System.out.println("存储完毕");
        oos.close();

    }
}

class Dog implements Serializable {
    private String name ;
    private int age ;

    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

```

## 注意事项
- 读写顺序要一致
- 要求实现序列化或反序列化对象，需要实现```Serializable```
- 序列化的类中简直添加```SerialVersionUID```，为了提高版本的兼容性
- 序列化对象时，默认将里面所有属性都进行序列化，但除了static或transient修饰的成员
- 序列化对象时，要求里面属性的类型也需要实现序列化接口
- 序列化具备可继承性，也就是如果某类已经实现了序列化，则它的所有子类也已经默认实现了序列化

# ```InputStreamReader```和```OutputStreamWriter```——转换流

- ```InputStreamReader``` Reader的子类，可以将InputStream（字节流）包装成Reader（字符流）
- ```OutputStreamWriter``` Writer的子类，可以将OutputStream（字节流）包装成Writer（字符流）
- 当处理纯文本数据时，如果使用字符流效率更高，并且可以有效解决中文问题，所以建议将字节流转换成字符流
- 可以在使用时指定编码格式（比如utf-8，gbk，gb2312，ISO8859-1等）

![image.png](https://pumpkn.xyz/upload/2021/09/image-0608f8cec2c848dca8593d4c95b954b5.png)

# ```Properties```类

- 专门用于读写配置文件的集合类

	- 配置文件的格式： 键=值

- 注意：键值对不需要有空格，值不需要用引号一起来。默认类型是String

## 常用方法

- ```load``` 加载配置文件的键值对到Properties对象
- ```list``` 将数据显示到指定设备
- ```getProperty(key)``` 根据键获取值
- ```setProperty(key ,value)``` 设置键值对到Properties对象
- ```store``` 将Properties中的键值对存储到配置文件，在idea中，保存信息到配置文件，如果含有中文，会存储为unicode码

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
        //将k-v存储到文件
        properties.store( new FileOutputStream("src\\mysql.properties") , null);
    }
}

```



