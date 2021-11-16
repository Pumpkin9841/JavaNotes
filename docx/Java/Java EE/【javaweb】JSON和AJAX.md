# Filter过滤器

### 1 什么是过滤器

- Filter过滤器是JavaWeb三大组件之一。三大组件分别是Servlet程序、Listener监听器、Filter过滤器
- Filter过滤器它是JavaEE的规范，也就是接口
- Filter过滤器的作用是：拦截请求，过滤响应


拦截请求常见的引用场景有：
    - 权限检查
    - 日记操作
    - 事务管理
    ...等等
    
### 2 体验Filter

要求：在web工程下，有一个admin目录。这个admin目录下的所有资源（html页面、jpg图片、jsp文件、等等）都必须是用户登录之后才允许访问的。

![0eff8ecffbf56eed4a531768e596d41c.png](en-resource://database/2956:1)

实例代码

filter代码

```java
// 实现Filter接口
public class AdminFilter implements Filter {
/**
* doFilter 方法，专门用于拦截请求。可以做权限检查
*/
@Override
public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain
filterChain) throws IOException, ServletException {
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        HttpSession session = httpServletRequest.getSession();
        Object user = session.getAttribute("user");
        // 如果等于 null，说明还没有登录
        if (user == null) {
             servletRequest.getRequestDispatcher("/login.jsp").forward(servletRequest,servletResponse);
            return;
        } 
        else {
        // 让程序继续往下访问用户的目标资源
            filterChain.doFilter(servletRequest,servletResponse);
        }
    }
}

```

web.xml配置
```xml

<!--filter 标签用于配置一个 Filter 过滤器-->
<filter>
<!--给 filter 起一个别名-->
    <filter-name>AdminFilter</filter-name>
<!--配置 filter 的全类名-->
    <filter-class>com.atguigu.filter.AdminFilter</filter-class>
</filter>
<!--filter-mapping 配置 Filter 过滤器的拦截路径-->
<filter-mapping>
<!--filter-name 表示当前的拦截路径给哪个 filter 使用-->
    <filter-name>AdminFilter</filter-name>
<!--url-pattern 配置拦截路径
/ 表示请求地址为：http://ip:port/工程路径/ 映射到 IDEA 的 web 目录
/admin/* 表示请求地址为：http://ip:port/工程路径/admin/*
-->
    <url-pattern>/admin/*</url-pattern>
</filter-mapping

```

Filter过滤器的使用步骤

- 编写一个类去实现Filter接口
- 实现过滤方法doFilter()
- 到web.xml中去配置Filter的拦截路径


### 3 Filter的生命周期

Filter的生命周期包含几个方法

- 1. 构造方法
- 2. init初始化方法
>第1，2步在web工程启动的时候执行（Filter已经创建）

- 3.doFilter过滤方法
>第3步，每次拦截到请求，就会执行

- 4.destory销毁
>第4步，停止web工程的时候，就会执行（停止web工程，也会徐晓辉Filter过滤器）


### 4 FilterConfig类

FilterConfig类，他是Filter过滤器的配置文件类。
Tomcat每次创建Filter的时候，也会同时创建一个FilterConfig类，这里包含了Filter配置文件的配置信息。

- Filter类的作用是获取Filter过滤器的配置内容

    - 获取Filter的名称 filter-name的内容
    - 获取Filter中配置的init-param初始化参数
    - 获取ServletContext对象


```java
// 类实现Filter接口
@Override
public void init(FilterConfig filterConfig) throws ServletException {
    System.out.println("2.Filter 的 init(FilterConfig filterConfig)初始化");
    //1、获取 Filter 的名称 filter-name 的内容
    System.out.println("filter-name 的值是：" + filterConfig.getFilterName());
    //2、获取在 web.xml 中配置的 init-param 初始化参数
    System.out.println("初始化参数 username 的值是：" + filterConfig.getInitParameter("username"));
    System.out.println("初始化参数 url 的值是：" + filterConfig.getInitParameter("url"));
    //3、获取 ServletContext 对象
    System.out.println(filterConfig.getServletContext());
}

```

```xml

<!--filter 标签用于配置一个 Filter 过滤器-->
<filter>
<!--给 filter 起一个别名-->
    <filter-name>AdminFilter</filter-name>
<!--配置 filter 的全类名-->
<filter-class>com.atguigu.filter.AdminFilter</filter-class>
    <init-param>
        <param-name>username</param-name>
        <param-value>root</param-value>
    </init-param>
    <init-param>
        <param-name>url</param-name>
        <param-value>jdbc:mysql://localhost3306/test</param-value>
    </init-param>
</filter>

```

### 5 Filter过滤器链

Filter 过滤器
Chain  链
FilterChain就是过滤器链（多个过滤器如何一起共工作）

![39b41cbef0b889479116ce549b46a0f7.png](en-resource://database/2958:1)


### 6 Filter的拦截路径

- 精确匹配
    - <url-pattern>/target.jsp</url-pattern>

- 目录匹配
    - <url-pattern>/admin/*</url-pattern>
> 以上配置的路径，表示请求地址必须为：http://ip:port/工程路径/admin/*


- 后缀名匹配
    - <url-pattern>*.html</url-pattern>
>以上配置的路径，表示请求地址必须以.html 结尾才会拦截到



Filter 过滤器它只关心请求的地址是否匹配，不关心请求的资源是否存在！！！
