---
title: 【JavaWeb】Tomcat
date: 2021-09-28 09:56:19.772
updated: 2021-09-28 10:42:30.958
url: https://pumpkn.xyz/archives/javawebtomcat
categories: 
tags: 学习 | tomcat  | JavaWeb
---

# JavaWeb的概念

- **什么是Javaweb**
	- > javaweb是指，所有通过java语言编写可以通过浏览器访问的程序的总称，javaweb是基于请求和响应来开发的

- **什么是请求**
	- > 请求是客户端给服务器发送数据，叫请求```request```

- **什么是响应**
	- > 响应是指服务器给客户端回传数据，叫响应response

- **请求和响应的关系**
	- > 请求和响应是成对出现的，有请求就有响应。

![image.png](https://pumpkn.xyz/upload/2021/09/image-af2252e2ae974ae9bd17b4fc92368acb.png)



# web资源分类
web资源按实现的技术和呈现的效果的不同，又分为静态资源和动态资源两种

- 静态资源
	
	- html、css、js、txt、mp4、jpg
- 动态资源

	- jsp页面、servlet程序


# 常用的web服务器

- ```Tomcat``` 由 Apache 组织提供的一种 Web 服务器，提供对 jsp 和 Servlet 的支持。它是一种轻量级的 javaWeb 容器（服务 器），也是当前应用最广的 JavaWeb 服务器（免费）。

- ```Jboss```  是一个遵从 JavaEE 规范的、开放源代码的、纯 Java 的 EJB 服务器，它支持所有的 JavaEE 规范（免费）

- ```GlassFish``` 由 Oracle 公司开发的一款 JavaWeb 服务器，是一款强健的商业服务器，达到产品级质量（应用很少）。

- ```Resin``` 是 CAUCHO 公司的产品，是一个非常流行的服务器，对 servlet 和 JSP 提供了良好的支持， 性能也比较优良，resin 自身采用 JAVA 语言开发（收费，应用比较多）。

- ```WebLogic``` 是 Oracle 公司的产品，是目前应用最广泛的 Web 服务器，支持 JavaEE 规范， 而且不断的完善以适应新的开发要求，适合大型项目（收费，用的不多，适合大公司）。

# Tomcat与servle版本关系

![image.png](https://pumpkn.xyz/upload/2021/09/image-fb5a350e843a4384a1753797263bd41b.png)


Servlet 程序从 2.5 版本是现在世面使用最多的版本（xml 配置） 到了 Servlet3.0 之后。就是注解版本的 Servlet 使用


# Tomcat的使用

## 安装
找到需要的tomcat版本对应的zip，解压即可


## 目录介绍

- ```bin``` 专门用来存放tomcat服务器的可执行程序
- ```conf``` 专门用来存放tomcat服务器的配置文件
- ```lib``` 专门用来存放tomcat服务器的jar包
- ```logs``` 专门用来存放tomcat服务器运行时输出的日记信息
- ```temp``` 专门用来存放tomcat运行时产生的临时数据
- ```webapps``` 专门用来存放部署的web工程
- ```work``` 是tomcat工作时的目录，用来存放tomcat运行时jsp翻译为servlet的源码，和session钝化目录

## 如何启动tomcat服务器
找到tomcat目录下的bin目录下的```startup.dat```文件，双击就可以启动tomcat服务器。

### 如何测试tomcat服务器启动成功？
打开浏览器，输入```localhost:8080```，出现如下界面即成功

![image.png](https://pumpkn.xyz/upload/2021/09/image-b6d7fdf18ed74f1bb606d48d8914c441.png)


## 启动失败？
常见的启动失败的情况有，双击startup.bat文件，就会出现一个小黑窗一闪而过。这个时候，**失败的原因基本上是因为没有配置好```JAVA_HOME```环境变量**

- **配置```JAVA_HOME环境变量``` **

	- JAVA_HOME必须全大写
	- 只需要配置到jdk安装目录即可，不需带上bin目录


## 如何修改Tomcat端口号
**tomcat默认的端口号是8080**，找到tomcat目录下的```conf```目录，找到```server.xml```配置文件

![image.png](https://pumpkn.xyz/upload/2021/09/image-2139e32b459c4bb694489d7a10a29629.png)


# 如何部署web工程到tomcat中

## 方法一
**只需要把web工程的目录拷贝到tomcat的webapps目录下即可**</br>
访问时，只需要在浏览器中输入访问地址```http://ip:port/工程名/目录下/文件名```


## 方法二
找到tomcat下的conf目录\Catalina\localhost\下，创建配置文件```文件名.xml```</br>

![image.png](https://pumpkn.xyz/upload/2021/09/image-e7139d83ebea41d5be4c6d72cc7e0277.png)

```abc.xml```配置文件内容
```xml
<!-- 
Context 表示一个工程上下文 
path 表示工程的访问路径:/abc 
docBase 表示你的工程目录在哪里 
--> 
<Context path="/abc" docBase="E:\book" />
```
访问这个工程的路径为```http://ip:port/abc/``` ，就表示访问```E:\book```目录 


# 手托html到浏览器与地址访问的区别

- **手托html页面原理** 
![image.png](https://pumpkn.xyz/upload/2021/09/image-afb0c528832b4a68a9b16043c5cc222f.png)

- **输入访问地址原理**

![image.png](https://pumpkn.xyz/upload/2021/09/image-38e170c9781a4725b9d6162beaebdee2.png)

# root工程的访问与默认index.html页面

当在浏览器地址栏中输入访问地址```http://ip:port/```——>没有工程名的时候，默认访问的时root工程

</br>
当在浏览器地址栏中输入的访问地址```http://ip:port/工程名/——>没有资源名，默认访问index.html页面


# idea 整合Tomcat服务器

操作的菜单如下：File | Settings | Build, Execution, Deployment | Application Servers

![image.png](https://pumpkn.xyz/upload/2021/09/image-69a56ec6b09647d8a3916cb6a64d4819.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-3deb73869e8b4098aa057cd945305d5c.png)

就可以通过创建一个 Model 查看是不是配置成功！！！

![image.png](https://pumpkn.xyz/upload/2021/09/image-7694a615983b44eea63e9d36d6e5969b.png)

# idea中动态web工程的操作

- **idea中创建动态web工程**

	- ![image.png](https://pumpkn.xyz/upload/2021/09/image-2012b48f984f423fb396f6dbba6986b9.png)
	- ![image.png](https://pumpkn.xyz/upload/2021/09/image-54957c514b534b5db435735895a939db.png)


- **web工程目录介绍**
![image.png](https://pumpkn.xyz/upload/2021/09/image-f28619f451e34f96a02fa4d748d1d4a1.png)


- **如何给动态web工程添加额外jar包**

可以打开项目结构菜单操作界面，添加一个自己的类库：

![image.png](https://pumpkn.xyz/upload/2021/09/image-4c03f219bce74e54bf7d25f39ccae9e9.png)


![image.png](https://pumpkn.xyz/upload/2021/09/image-4c922e3d1f9b4754b30039cc3dd05d35.png)

选择你添加的类库，给哪个模块使用：
![image.png](https://pumpkn.xyz/upload/2021/09/image-44ee3883206c426e802545f7aecc1e1c.png)

选择 Artifacts 选项，将类库，添加到打包部署中：

![image.png](https://pumpkn.xyz/upload/2021/09/image-75c4f05d58fd4de5bccc75a8d99e94f6.png)

# 如何在idea中部署工程到tomcat上运行
建议修改 web 工程对应的 Tomcat 运行实例名称：
![image.png](https://pumpkn.xyz/upload/2021/09/image-478ba44e64844458b8c890ea89974b62.png)

确认你的 Tomcat 实例中有你要部署运行的 web 工程模块：
![image.png](https://pumpkn.xyz/upload/2021/09/image-697c98cf30214241b5654f4219ebafa3.png)

你还可以修改你的 Tomcat 实例启动后默认的访问地址

![image.png](https://pumpkn.xyz/upload/2021/09/image-4c9d7141cb764243b29855cd0e4e556e.png)

配置资源热部署

![image.png](https://pumpkn.xyz/upload/2021/09/image-7d7ea612e3574117a2b0dbc6e43701e9.png)