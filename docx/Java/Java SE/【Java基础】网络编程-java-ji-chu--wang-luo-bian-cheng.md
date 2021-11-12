---
title: 【Java基础】网络编程
date: 2021-09-19 23:21:47.948
updated: 2021-09-20 10:03:43.591
url: https://pumpkn.xyz/archives/java-ji-chu--wang-luo-bian-cheng
categories: 
tags: 学习 | Java
---

# 网络相关概念

## TCP和UDP

### TCP协议：传输控制协议

- 使用TCP协议前，须先建立TCP连接，形成传输数据通道
- 传输前，采用“三次握手”方式，是**可靠**的
- TCP协议进行通信的两个应用进程：客户端、服务端
- 在连接中可进行大数据量的传输
- 传输完毕，需释放已建立的连接，**效率低**

### UDP协议

- 将数据、源、目的封装成数据包，不需要建立连接
- 每个数据报的大小限制在**64K**内
- 因无需连接，故是**不可靠的**
- 发送数据结束时无序释放资源（因为不是面向连接的），速度快


## ```InetAddress```类
- ```getLocalHost``` 获取本机InetAddress对象
-  ```getByName``` 根据指定主机名/域名获取ip地址对象
- ```getHostName``` 获取InetAddress对象的主机名
- ```getHostAddress``` 获取InetAddress对象的地址
![image.png](https://pumpkn.xyz/upload/2021/09/image-d83918cb514f4d41bfb244224a670b1d.png)

# ```Socket```
## 基本介绍

- 套接字(Socket)开发网络应用程序被广泛采用，以至于成为事实上的标准。
- **通信的两端都要有Socket**,是两台机器间通信的端点
- 网络通信其实就是Socket间的通信
- Socket允许程序把网络连接当成一个流，数据在两个Socket间通过IO传输
- 一般主动发起通信的应用程序属于客户端，等待通信请求的为服务端

![image.png](https://pumpkn.xyz/upload/2021/09/image-358f6ffa8d4c4091a383c173dbefba7d.png)


# TCP网络通信编程

## 基本介绍

- 基于客户端——服务端的网络通信
- 底层使用的是TCP/IP协议
- 应用场景： 客户端发送数据，服务端接收并显示
- 基于```Socket```的TCP编程

	- ![image.png](https://pumpkn.xyz/upload/2021/09/image-6554e8678a2e4266aabe5ee33556cb4a.png)

## TCP应用案例1

- 编写一个服务器端和一个客户端
- 服务器端在9999端口监听
- 客户端连接到服务器端，发送"hello,server"，然后推出
- 服务器端收到客户端发送的信息，回应客户端并退出


![image.png](https://pumpkn.xyz/upload/2021/09/image-dd48d8f996784466b61c006ab4e125b2.png)

## 字节流实现

### 客户端代码
```Java
public class z1_TCP客户端 {
    public static void main(String[] args) throws IOException {

        // 连接服务端(ip,端口)
        //连接本机的9999端口，如果连接成功，返回socket对象
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);

        //连接上后，生成Socket,通过socket.getOutputStream()得到和socket对象关联的输出流对象
        OutputStream outputStream = socket.getOutputStream();

        //通过输出流，写入数据到数据通道
        outputStream.write("hello , socker!".getBytes());

        //设置写入结束标记
        socket.shutdownOutput();


        InputStream inputStream = socket.getInputStream();
        byte[] bytes = new byte[1024] ;
        int readLen ;
        while( (readLen = inputStream.read(bytes)) != -1 ){
            System.out.print(new String(bytes , 0 , readLen));
        }

        System.out.println("");

        //关闭流对象和socket
        inputStream.close();
        outputStream.close();
        socket.close();
        System.out.println("客户端退出!!!");
    }
}

```

### 服务端代码

```Java
public class z1_服务端 {
    public static void main(String[] args) throws IOException {
        //在本机监听9999端口，等待连接
        //细节： 1.要求在本机没有其他服务器在监听9999
        //      2.这个ServerSocket可以通过accept()返回多个Socket[多个客户端连接服务器的并发]
        ServerSocket serverSocket = new ServerSocket(9999);
        System.out.println("等待客户端连接~~~");

        //当没有客户端连接9999端口是，程序会阻塞，等待连接
        //如果有客户端连接，则返回Socket对象，程序继续
        Socket accept = serverSocket.accept();

        //通过socket.getInputStream()读取客户端写入到数据通道的数据
        InputStream inputStream = accept.getInputStream();

        //读取
        byte[] bytes = new byte[1024] ;
        int readLen ;
        while( (readLen = inputStream.read(bytes)) != -1 ){
            System.out.print(new String(bytes , 0 , readLen));
        }


        OutputStream outputStream = accept.getOutputStream();
        outputStream.write("hello , client".getBytes());

        //设置写入结束标记
        accept.shutdownOutput();


        System.out.println("服务器退出!!!");
        //关闭资源
        outputStream.close();
        inputStream.close();
        accept.close();
        serverSocket.close();
    }
}

```

## 字符流实现上述需求

### 客户端代码
```Java
public class z2_z1的字符流实现_客户端 {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);

        OutputStream outputStream = socket.getOutputStream();
        BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(outputStream));
        bufferedWriter.write("hello , server");
        bufferedWriter.flush();
        socket.shutdownOutput();

        InputStream inputStream = socket.getInputStream();
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
        String readLine ;
        while( (readLine = bufferedReader.readLine() ) != null ){
            System.out.println(readLine);
        }

        //关闭资源
        bufferedReader.close();
        bufferedWriter.close();
        socket.close();
    }
}

```

### 服务端代码
```Java
public class z2_z1的字符流实现_服务端 {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(9999);
        //等待连接
        Socket socket = serverSocket.accept() ;

        //通过字符流获取客户端发来的信息
        InputStream inputStream = socket.getInputStream();
        //将字节流转换成字符流
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
        String readLine ;
        while( (readLine = bufferedReader.readLine()) != null ){
            System.out.println(readLine);
        }

        OutputStream outputStream = socket.getOutputStream();
        BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(outputStream));
        bufferedWriter.write("hello , client!");
        bufferedWriter.flush(); //使用字符流需要手动刷新，否则数据不会写入数据通道

        socket.shutdownOutput(); //写入结束标记

        //关闭资源
        bufferedReader.close();
        bufferedWriter.close();
        socket.close();
        serverSocket.close();

    }
}

```

## TCP应用案例2——文件上传
- 编写一个服务端，和一个客户端
- 服务端在8888端口监听
- 客户端连接到服务端，发送一张图片```"D:\\zaizai.jpg"```
- 服务端接收到客户端发送的图片，保存到src下，发送“收到图片”再退出
- 客户端接收到服务端发送的“收到图片”再退出
- 二进制文件用字节流

### 客户端代码
```Java
public class z1_客户端 {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket(InetAddress.getLocalHost(), 8888);
        String srcFilePath = "D:\\zaizai.jpg" ;

        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(srcFilePath));

        BufferedOutputStream bos = new BufferedOutputStream(socket.getOutputStream());

        byte[] bytes = new byte[1024] ;
        int readLen ;
        while( (readLen = bis.read(bytes)) != -1 ){
            bos.write(bytes , 0 , readLen);
        }
        socket.shutdownOutput();

        BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
        String readLine ;
        while( (readLine = br.readLine()) != null ){
            System.out.println(readLine);
        }

        //关闭资源
        br.close();
        bis.close();
    //    bos.close();
        socket.close();

    }
}

```

### 服务端代码

```Java
public class z2_服务端 {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(8888);
        String destFilePath = "src\\1.jpg" ;
        System.out.println("等待客户端连接~~~~");
        //获取客户端连接
        Socket socket = serverSocket.accept();

        System.out.println("客户端连接成功~~~~");

        //获取socket相关输入流
        InputStream inputStream = socket.getInputStream();
        //包装输入流为处理字节流
        BufferedInputStream bis = new BufferedInputStream(inputStream);
        //实例字节输出流
        FileOutputStream fos = new FileOutputStream(destFilePath);
        //包装输出流为处理字节流
        BufferedOutputStream bos = new BufferedOutputStream(fos);

        byte[] bytes = new byte[1024] ;
        int readLine ;
        while( (readLine = bis.read(bytes)) != -1 ){
            bos.write(bytes , 0 , readLine);
        }

        System.out.println("文件上传完成!");

        OutputStream outputStream = socket.getOutputStream();
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(outputStream));
        bw.write("服务端通知：文件上传完成");
        bw.flush();

        socket.shutdownOutput();

        //关闭资源
        bw.close();
        bos.close();
        bis.close();
        socket.close();
        serverSocket.close();


    }
}

```

## TCP应用案例3——文件下载

### 客户端代码
```Java
public class TCPClient {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);

        OutputStream outputStream = socket.getOutputStream();
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(outputStream));
        Scanner scanner = new Scanner(System.in);
        String s = scanner.nextLine();
        bw.write(s);
        bw.flush();


        socket.shutdownOutput();


        InputStream is = socket.getInputStream();
        BufferedInputStream bis = new BufferedInputStream(is);
        String filePath = "D:\\2.jpg" ;
        FileOutputStream fos = new FileOutputStream(filePath);
        BufferedOutputStream bos = new BufferedOutputStream(fos);

        byte[] bytes = new byte[1024] ;
        int leng ;
        while( (leng = bis.read(bytes)) != -1 ){
            bos.write(bytes , 0 , leng);
        }

        System.out.println("文件下载成功");
        bw.close();
        bis.close();
        bos.close();
        socket.close();

    }
}

```

### 服务端代码
```Java
public class TCPServer {
    public static void main(String[] args) throws IOException {
        //服务端监听9999端口
        ServerSocket serverSocket = new ServerSocket(9999);

        System.out.println("等待客户端连接~~~~");
        //登台客户端连接
        Socket socket = serverSocket.accept();

        //获取socket相关的字节输出流
        InputStream is = socket.getInputStream();
        //包装成字符处理流
        BufferedReader br = new BufferedReader(new InputStreamReader(is));
        String readLine ;
        String fileName = null ;
        while( (readLine = br.readLine())  != null ){
            fileName = readLine ;
        }

        String filePath = "src\\" + fileName ;

        FileInputStream fis = new FileInputStream(filePath);
        BufferedInputStream bis = new BufferedInputStream(fis);

        OutputStream outputStream = socket.getOutputStream();
        BufferedOutputStream bos = new BufferedOutputStream(outputStream);

        byte[] bytes = new byte[1024] ;
        int leng ;
        while( (leng = bis.read(bytes)) != -1 ){
            bos.write(bytes , 0 , leng) ;
        }

        //执行shutdownOutput()方法，会将输出流bos关闭
        socket.shutdownOutput();

        br.close();
        bis.close();
     //   bos.close();
        socket.close();
        serverSocket.close();



    }
}

```


# TCP网络编程细节

## ```netstat```指令
- ```netstat -an``` 可以查看当前主机网络情况，包括端口监听情况和网络连接情况
- ```netstat -an|more``` 可以分页显示
- 要求再dos控制台下执行

## 客户端细节

- 当客户端连接到服务端后，实际上客户端也是通过一个端口和服务端进行通讯的，这个端口是TCP/IP来分配的，是不确定的，是随机的

![image.png](https://pumpkn.xyz/upload/2021/09/image-85e232fc43f5405ca1bca322f3127c1b.png)


# UDP网络编程【了解即可】

## 基本介绍

- 类```DatagramSocket```和```DatagramPacket```[数据包/数据报]实现了基于UDP协议网络程序
- UDP数据报通过数据报套接字```DatagramSocket```发送和接收，系统不保证UDP数据报一定能够安全送到目的地，也不能确定什么时候可以抵达
- ```DatagramSocket```对象封装了UDP数据报，在数据报中包含了发送端和IP地址和端口号以及接收端的ip地址和端口号
- UDP协议中每个数据报都给出了完整的地址信息，因此无需建立发送方和接收方的连接

## 操作步骤

- 核心的两个类/对象```DatagramSocket```和```DatagramPacket```
- 建立发送端，接收端
- 建立数据包
- 调用```DatagramSocket```的发送、接收方法
- 关闭```DatagramSocket```

![image.png](https://pumpkn.xyz/upload/2021/09/image-1a810037a62e46269598850e1003aa5a.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-3b9bf7095751481fbb4f930a4590ded2.png)

## UDP应用案例

### 接收端
```Java
public class z1_UDP接收端A {
    public static void main(String[] args) throws IOException {
        //创建一个DatagramSocket对象，准备在9999接收数据
        DatagramSocket ds = new DatagramSocket(9999);

        //构建一个DatagramPacket对象，准备就收数据
        byte[] bytes = new byte[1024] ;
        DatagramPacket dp = new DatagramPacket(bytes , bytes.length);

        System.out.println("准备接收数据~~~~~~");
        //调用接收方法，将通过网络传输到DatagramPacket对象
        //如果接收到，就会收到数据，否则会阻塞等待
        ds.receive(dp);

        //收到数据报，进行拆包，取出数据
        byte[] data = dp.getData();
        int length = dp.getLength();
        String s = new String(data, 0, length);
        System.out.println("A端接收到数据 " + s) ;


        byte[] bytes1 = "get your message from port B".getBytes() ;
        DatagramPacket dp1 = new DatagramPacket(bytes1, bytes1.length, InetAddress.getLocalHost(), 9998);

        ds.send(dp1);


        ds.close();
    }
}

```

### 发送端
```Java
public class z1_UDP发送端B {
    public static void main(String[] args) throws IOException {
        //创建DatagramSocket对象，准备在9998端口接收数据
        DatagramSocket ds = new DatagramSocket(9998);
        System.out.println("发送数据~~~~");
        //将需要发送的数据，封装到DatagramPacket对象
        byte[] message = "hello udp to port A".getBytes() ;
        DatagramPacket dp = new DatagramPacket(message, message.length, InetAddress.getLocalHost(), 9999);
        ds.send(dp);

        System.out.println("准备接收数据~~~~");
        byte[] bytes = new byte[1024] ;
        DatagramPacket packet = new DatagramPacket(bytes, bytes.length);
        ds.receive(packet) ;

        byte[] data = packet.getData();
        int length = packet.getLength() ;

        String s = new String(data, 0 , length);
        System.out.println("接收到数据A端数据:" + s );


        ds.close();
    }
}
```