# Cookie

### 1 什么是Cookie？

- Cookie翻译过来是饼干的意思
- Cookie是服务器通知客户端保存键值对的一种技术
- 客户端有了Cookie后，每次请求都发送给服务器
- 每个Cookie的大小不能超过4kb


### 2 如何创建Cookie

![image](https://user-images.githubusercontent.com/91939988/141827726-ff91fa90-1633-4037-9e81-7bd62889a1a8.png)

```java

protected void createCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException,IOException {
    //1 创建 Cookie 对象
    Cookie cookie = new Cookie("key4", "value4");
    //2 通知客户端保存 Cookie
    resp.addCookie(cookie);
    //1 创建 Cookie 对象
    Cookie cookie1 = new Cookie("key5", "value5");
    //2 通知客户端保存 Cookie
    resp.addCookie(cookie1);
    resp.getWriter().write("Cookie 创建成功");
}

```

### 服务器如何获取Cookie

服务器获取客户端的Cookie只需要一行代码
```java
request.getCookies() : Cookie[]
```

![image](https://user-images.githubusercontent.com/91939988/141827769-9dcc7078-68f8-4b6f-aa9d-c5196a9780f6.png)

```java

public class CookieUtils {
/**
* 查找指定名称的 Cookie 对象
* @param name
* @param cookies
* @return
*/
public static Cookie findCookie(String name , Cookie[] cookies){
       if (name == null || cookies == null || cookies.length == 0) {
            return null;
        }
      for (Cookie cookie : cookies) {
            if (name.equals(cookie.getName())) {
                 return cookie;
            }
        }
        return null;
    }
}

```
servlet代码

```java

protected void getCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
    Cookie[] cookies = req.getCookies();
    for (Cookie cookie : cookies) {
    // getName 方法返回 Cookie 的 key（名）
    // getValue 方法返回 Cookie 的 value 值
    resp.getWriter().write("Cookie[" + cookie.getName() + "=" + cookie.getValue() + "] <br/>");
    }
    Cookie iWantCookie = CookieUtils.findCookie("key1", cookies);

    // 如果不等于 null，说明赋过值，也就是找到了需要的 Cookie
    if (iWantCookie != null) {
    resp.getWriter().write("找到了需要的 Cookie");
    }
}

```

### Cookie值的修改

方案一: 
- 先创建一个要修改的同名（指的就是key）的cookie对象
- 在构造器，同时赋予新的Cookie值
- 调用response.addCookie(cookie)
```java

Cookie cookie = new Cookie("key1","newValue1");
resp.addCookie(cookie);

```

方案二：
- 先查找到需要修改的Cookie对象
- 调用setValue()方法赋予新的Cookie值
- 调用response.addCookie()通知客户端保存修改

```java


// 1、先查找到需要修改的 Cookie 对象
Cookie cookie = CookieUtils.findCookie("key2", req.getCookies());
if (cookie != null) {
// 2、调用 setValue()方法赋于新的 Cookie 值。
  cookie.setValue("newValue2");
// 3、调用 response.addCookie()通知客户端保存修改

 resp.addCookie(cookie);
}

```


### 3 Cookie生命控制

Cookie的生命控制指的是如何管理Cookie什么时候被销毁

**setMaxAge()**
  - 正数，表示在指定的秒数后过期
  - 负数，表示浏览器一关，Cookie就会被删除（默认值是-1）
  - 零，表示马上删除Cookie
  
  
  
```java

/**
* 设置存活 1 个小时的 Cooie
* @param req
* @param resp
* @throws ServletException
* @throws IOException
*/
protected void life3600(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
    Cookie cookie = new Cookie("life3600", "life3600");
    cookie.setMaxAge(60 * 60); // 设置 Cookie 一小时之后被删除。无效
    resp.addCookie(cookie);
    resp.getWriter().write("已经创建了一个存活一小时的 Cookie");
}



/**
* 马上删除一个 Cookie
* @param req
* @param resp
* @throws ServletException
* @throws IOException
*/
protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
    // 先找到你要删除的 Cookie 对象
    Cookie cookie = CookieUtils.findCookie("key4", req.getCookies());
    if (cookie != null) {
        // 调用 setMaxAge(0);
        cookie.setMaxAge(0); // 表示马上删除，都不需要等待浏览器关闭
        // 调用 response.addCookie(cookie);
        resp.addCookie(cookie);
        resp.getWriter().write("key4 的 Cookie 已经被删除");
    }
}


/**
* 默认的会话级别的 Cookie
* @param req
* @param resp
* @throws ServletException
* @throws IOException
*/
protected void defaultLife(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
    Cookie cookie = new Cookie("defalutLife","defaultLife");
    cookie.setMaxAge(-1);//设置存活时间
    resp.addCookie(cookie);
}


```

### 4 Cookie有效路径path的设置

Cookie的path属性可以有效的过滤哪些Cookie可以发送给服务器，哪些不发。 

path属性是通过请求的地址来进行有效过滤的。

- CookieA     path=/工程路径
- CookieB path=/工程路径/abc


请求地址如下：

- http://ip:port/工程路径/a.html

    - CookieA 发送
    - CookieB 不发送

- http://ip:port/工程路径/abc/a.html

    - CookieA 发送
    - CookieB 发送

```java

protected void testPath(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
    Cookie cookie = new Cookie("path1", "path1");
    // getContextPath() ===>>>> 得到工程路径
    cookie.setPath( req.getContextPath() + "/abc" ); // ===>>>> /工程路径/abc
    resp.addCookie(cookie);
    resp.getWriter().write("创建了一个带有 Path 路径的 Cookie");
}

```

# Session 会话

### 1 什么是Session会话

- Session就是一个接口(HttpSession)
- Session就是会话。它是用来维护一个客户端和服务器之间关联的一种技术
- 每个客户端都有自己的一个Session会话。
- Session会话中，我们经常用来保存用户登陆之后的信息


### 2 如何创建Session和获取(id号，是否为新)

如何创建和获取Session，他们的API是一样的。

- request.getSession()

    - 第一次调用是 创建Session会话
    - 之后调用都是：获取前面创建好的Session会话对象


- isNew() 

    - 判断Session是不是刚创建出来的
    - true 表示刚创建
    - false 表示获取之间创建


每个会话都有一个身份证号。也就是ID值。而且这个ID是唯一的。
getId() 得到Session的会话ID值。


### 3 Session域数据的存取

```java

/**
* 往 Session 中保存数据
* @param req* @param resp
* @throws ServletException
* @throws IOException
*/
protected void setAttribute(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
    req.getSession().setAttribute("key1", "value1");
    resp.getWriter().write("已经往 Session 中保存了数据");
}
/**
* 获取 Session 域中的数据
* @param req
* @param resp
* @throws ServletException
* @throws IOException
*/
protected void getAttribute(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
    Object attribute = req.getSession().getAttribute("key1");
    resp.getWriter().write("从 Session 中获取出 key1 的数据是：" + attribute);
}

```

### 4 Session生命周期控制
```java
// 设置Session的超时时间（单位：秒），超过指定的时长，Session就会被销毁
public void setMaxInactiveInterval(int interval)
```

- interval

    -  为正数时，为设定Session的超时时长
    -  负数表示永不超时（极少使用）



```java
//获取Session的超时时间
public int getMaxInactiveInterval()


// 让当前Session会话马上超时无效
public void invalidate()
```

Session默认的超时时间为**30分钟**

因为在Tomcat服务器的配置文件web.xml中默认有一下的配置，他表示配置了当前Tomcat服务器下的所有Session超时默认时长为30分钟

```xml

<!--表示当前 web 工程。创建出来 的所有 Session 默认是 20 分钟 超时时长-->
<session-config>
    <session-timeout>20</session-timeout>
</session-config>

```

Session超时的概念
![image](https://user-images.githubusercontent.com/91939988/141827831-028b289d-0627-4c33-8b3e-048eceaaa934.png)


```java

protected void life3(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
    // 先获取 Session 对象
    HttpSession session = req.getSession();
    // 设置当前 Session3 秒后超时
    session.setMaxInactiveInterval(3);
    resp.getWriter().write("当前 Session 已经设置为 3 秒后超时");
}

```

Session马上被销毁实例

```java

protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
    // 先获取 Session 对象
    HttpSession session = req.getSession();
    // 让 Session 会话马上超时
    session.invalidate();
    resp.getWriter().write("Session 已经设置为超时（无效）");
}

```

### 5 浏览器和Session之间关联的技术内幕

![image](https://user-images.githubusercontent.com/91939988/141827862-05dd9768-11e0-4f5b-b3a6-892aec4d1454.png)
