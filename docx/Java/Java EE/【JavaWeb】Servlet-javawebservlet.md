---
title: 【JavaWeb】Servlet
date: 2021-10-08 09:46:26.36
updated: 2021-10-08 16:39:26.848
url: https://pumpkn.xyz/archives/javawebservlet
categories: 
tags: 学习 | JavaWeb
---

# ```Servlet```

## 什么是```Servlet```

- ```Servlet```是JavaEE规范之一。规范就是接口
- ```Servlet```就是JavaWeb三大组件之一。三大组件分别是：```servlet程序```、```Filter过滤器```、```Listener监听器```
- ```Servlet```是运行在服务器上的一个java小程序，**它可以接受客户端发送过来的请求，并响应数据给客户端**

## ```Servlet```程序的实现

- 编写一个类去实现Servlet接口
- 实现service方法，处理请求，并响应数据
- 到```web.xml```中去配置servlet程序的访问地址


```Java
public class HelloServlet implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("HelloServlet 被访问");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```

**在 ```web.xml```中的配置**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!-- servlet标签给tomcat配置Servlet程序    -->
    <servlet>
        <!-- servlet_name标签给Servlet程序起一个别名（一般是类名）-->
        <servlet-name>helloservlet</servlet-name>
        <!-- servlet_class是Servlet程序的全类名-->
        <servlet-class>com.zf.first.HelloServlet</servlet-class>
    </servlet>

    <!-- servlet-mapping给servlet程序配置访问地址-->
    <servlet-mapping>
        <!-- servlet_name标签的作用是告诉服务器当前配置的地址给哪个Servlet程序用-->
        <servlet-name>helloservlet</servlet-name>
        <!-- url-pattern 标签配置访问地址
            / 斜杠在服务器解析的时候，表示地址为http://ip:port/工程路径
            /hello 表示地址为http://ip:port/工程路径/hello
        -->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>



</web-app>
```

### 常见的错误
```url-pattern```中配置的路径没有以斜杠打头

![image.png](https://pumpkn.xyz/upload/2021/10/image-98f12aa4af194bdc8c76fc071e4548f6.png)

```servlet-name```配置的值不存在
![image.png](https://pumpkn.xyz/upload/2021/10/image-3def26b8056c40fc91952e5e6326fc73.png)

## url地址到Servlet程序的访问
![image.png](https://pumpkn.xyz/upload/2021/10/image-f6df6cca2ad64f61864efa3eb58bc8dc.png)


## servlet的生命周期

- 执行```servlet```构造器方法
- 执行```init```初始化方法
> 第一、二步是在第一次访问的时候创建servlet程序会调用，只执行一次

- 执行```service```方法
> 每次访问都会被调用

- 执行```destory```销毁方法
> 在web工程停止的时候调用 

## ```get```和```post```请求的分发处理

```Java
public class HelloServlet implements Servlet {
    /*** service 方法是专门用来处理请求和响应的
     * @param servletRequest * @param servletResponse
     * @throws ServletException * @throws IOException
     * */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("3 service === Hello Servlet 被访问了");
        // 类型转换（因为它有 getMethod()方法）
         HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        // 获取请求的方式 
        String method = httpServletRequest.getMethod(); 
        if ("GET".equals(method)) {
            doGet(); 
        } 
        else if ("POST".equals(method)) { doPost(); 
        } 
    }
    /*** 做 get 请求的操作 */ 
    public void doGet(){ 
        System.out.println("get 请求"); 
        System.out.println("get 请求"); 
    }
    /*** 做 post 请求的操作 */
    public void doPost(){ 
        System.out.println("post 请求"); 
        System.out.println("post 请求"); 
    } 
}
```

## 通过继承```httpServlet```实现```servlet```程序

一般在实际项目开发中，都是使用继承HttpServlet类的方式实现servlet程序

- 编写一个类去继承```HttpServlet```类
- 根据业务需要重写doGet或doPost方法
- 到web.xml中配置servlet程序的访问地址

```Java
public class HelloServlet1 extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("doPost方法");
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        System.out.println("doGet方法");
    }
}
```

在web.xml中配置
```xml
    <servlet>
        <servlet-name>HelloServlet1</servlet-name>
        <servlet-class>com.zf.web.HelloServlet1</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>HelloServlet1</servlet-name>
        <url-pattern>/helloServlet1</url-pattern>
    </servlet-mapping>
```

## ```Servlet```类的继承体系
![image.png](https://pumpkn.xyz/upload/2021/10/image-09ce2c59c3d24009a7695caf50f572ef.png)

# ```ServletConfig```类

- servletConfig类从类名上来看，就知道是servlet程序的配置信息类
- servlet程序和servletConfig对象都是由Tomcat负责创建的，我们负责使用
- servlet程序默认是第一次访问的时候创建，servletConfig是每个servlet程序创建时，就创建一个对应的servletConfig对象

## ```ServletConfig```类的三大作用

- 可以获取servlet程序的别名servlet-name的值
- 获取初始化参数init-param
- 获取servletContext对象


web.xml中配置
```xml
    <servlet>
        <!--servlet-name 标签 Servlet 程序起一个别名（一般是类名） -->
        <servlet-name>HelloServlet</servlet-name>
        <!--servlet-class 是 Servlet 程序的全类名-->
        <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
        <!--init-param 是初始化参数-->
        <init-param>
            <!--是参数名-->
            <param-name>username</param-name>
            <!--是参数值-->
            <param-value>root</param-value>
        </init-param> <!--init-param 是初始化参数-->
        <init-param>
            <!--是参数名-->
            <param-name>url</param-name>
            <!--是参数值-->
            <param-value>jdbc:mysql://localhost:3306/test</param-value>
        </init-param>
    </servlet>
```

servlet程序
```Java
    public void init(ServletConfig servletConfig) throws ServletException { 
        System.out.println("2 init 初始化方法"); 
        // 1、可以获取 Servlet 程序的别名 servlet-name 的值 
        System.out.println("HelloServlet 程序的别名是:" + servletConfig.getServletName()); 
        // 2、获取初始化参数 init-param 
        System.out.println("初始化参数 username 的值是;" + servletConfig.getInitParameter("username")); 
        System.out.println("初始化参数 url 的值是;" + servletConfig.getInitParameter("url")); 
        // 3、获取 ServletContext 对象 
        System.out.println(servletConfig.getServletContext()); 
    }
```

![image.png](https://pumpkn.xyz/upload/2021/10/image-9011fb07fc4d467aa8618ccb37fe1fe0.png)

# ```ServletContext```类

## 什么是```ServletContext```

- ```ServletContext```是一个接口，它表示Servlet上下文对象
- 一个web工程，只有一个```ServletContext```对象示例
- ```ServletContext```对象是一个域对象
- ```ServletContext```是在web工程部署启动的时候创建的。在web工程停止的时候销毁。

### 什么是域对象？
域对象，是可以像map一样存取数据的对象，叫域对象。这里的域指的是存取数据的操作范围，整个web工程。

||存数据|取数据|删除数据|
|-------|-------|-------|-------|
|map|put()|get()|remove()|
|域对象|setAttribute()|getAttribute()|removeAttribute()|

## ```ServletContext```类的四个作用

- 获取web.xml中配置的上下文参数context-param
- 获取当前的工作路径，格式 :/工程路径
- 获取工程部署后在服务器硬盘上的绝对路径
- 像map一样存取数据

```Java
public class ContextServlet1 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取web.xml中配置的上下文参数context-param
        ServletContext context = getServletConfig().getServletContext();
        String username = context.getInitParameter("username");
        String password = context.getInitParameter("password");
        System.out.println(username + password);

        //获取当前路径名
        System.out.println(context.getContextPath());
        //获取工程部署后在服务器硬盘上的绝对路径
        /*
        *   / 斜杠被服务器解析为http://ip:port/工程名/ 映射到idea代码的web目录
        * */
        System.out.println("工程部署的路径是" + context.getRealPath("/"));
    
    }
}
```

web.xml代码
```xml
   <context-param>
       <param-name>username</param-name>
       <param-value>zf</param-value>
   </context-param>
    <context-param>
        <param-name>password</param-name>
        <param-value>lx</param-value>
    </context-param>
```


# Http协议

## 什么是http协议
协议是指双方，或多方，相互约定好，大家都需要遵守的规则，叫协议。</br>
所谓Http协议，就是指，客户端和服务器之间通信时，发送的数据，需要遵守的规则，叫http协议。</br>
**http协议中的数据又叫报文**

## 请求的http协议格式
客户端给服务器发送数据叫请求，服务器给客户端回传数据叫响应。</br>
请求又分为get请求和post请求

### get请求

- 请求行

	- 请求的方式  get
	- 请求的资源路径[+?+请求参数]
	- 请求的协议版本号  http/1.1

- 请求头

	- ```key:value```组成
	- 不同的键值对，表示不同的含义


![image.png](6)


## post请求

- 请求行

	- 请求的方式  post
	- 请求的资源路径[+?+请求参数]
	- 请求的协议版本号   http/1.1

- 请求头

	- ```key:value```

- <font color ="red">空行 </font>

- 请求体 ===>> 就是发送给服务器的数据

![image.png](7)


## 常用请求头的说明

- ```accept``` 表示客户端可以接受的数据类型
- ```accept-language``` 表示客户端可以接受的语言信息
- ```User-Agent``` 表示客户端浏览器的信息
- ```host``` 表示请求时的服务器ip和端口

## 哪些时get请求，那些时post请求

- get请求有

	- from标签 methood = get
	- a标签
	- link标签引入css
	- script标签引入js文件
	- img标签引入图片
	- iframe引入html页面
	- 在浏览器地址栏中输入地址后敲回车

- post请求

	- form标签 method=post

## 响应的htttp协议格式

- 响应行

	- 响应的协议和版本号
	- 响应状态码
	- 响应状态描述

- 响应头

	- ```key:vale```

- <font color ="red">空行 </font>
- 响应体 ===>> 就是回传给客户端的数据
 
![image.png](https://pumpkn.xyz/upload/2021/10/image-c6f2e9f428324cb9b7328696a3ce481d.png)


## 常用的响应码说明

- 200  请求成功
- 302 请求重定向
- 404 服务器请求已收到，但是你要的数据不存在
- 500 服务器请求已收到，但是服务器内部错误（代码错误）

## MIME类型说明

MIME是http协议中数据类型。MIME类型的格式是“大类型/小类型”，并与某一种文件的扩展名相对应。

![image.png](https://pumpkn.xyz/upload/2021/10/image-e549f67544fe48ff8dcd23f0744dfaae.png)

![image.png](https://pumpkn.xyz/upload/2021/10/image-333280ada0f44962b1991016396a64aa.png)

浏览器查看http协议

![image.png](https://pumpkn.xyz/upload/2021/10/image-615e781fc1654f0597329e0223aae7d3.png)


# HttpServletRequest类

- ```HttpServletRequest```类的作用

	- 每次只要有请求进入Tomcat服务器，Tomcat服务器就会把请求过来的HTTP协议信息解析好封装到Request对象中。然后传递到service方法（doGet和doPost方法）中给我们使用。我们可以通过HttpServletReqeust对象，获取所有请求的信息






