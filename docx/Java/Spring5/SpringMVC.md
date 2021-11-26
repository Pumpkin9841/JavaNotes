- [Spring映射- [SpringMVC](#1)
    - 什么是MVC
    - 什么是SpringMVC
    - SpringMVC特点

- [HelloWorld](#2)
    - 开发环境
    - 创建maven工程
    - 配置web.xml
    - 创建请求控制器
    - 创建SpringMVC的配置文件
    - 测试

- [@RequestMapping注解](#3)
    - @RequestMapping注解功能
    - @RequestMapping注解的位置
    - @RequestMapping注解的value属性
    - @RequestMapping注解的method属性
    - @RequestMapping注解的params属性
    - @RequestMapping注解的headers属性
    - SpringMVC支持ant风格的路径
    - SpringMVC支持路径中的占位符
- [SpringMVC获取请求参数](#4)
    - 通过ServletAPI获取
    - 通过控制器方法的形参获取请求参数
    - @RequestParam
    - @ReuestHeader
    - @CookieValue
    - 通过POJO获取请求参数
    - 解决获取请求参数的乱码问题
- [域对象共享数据](#5)
    - 使用ServletAPI向request域对象共享是苏剧
    - 使用ModelAndView向request域对象共享数据
    - 使用Model向request域对象共享数据
    - 使用map向request域对象共享数据
    - 使用ModelMap向reuest域对象共享数据
    - Model、ModelMap、Map的关系
    - 向session域共享数据
    - 向applicaiton域共享数据
- [SpringMVC的视图](#6)
    - ThymeleafView
    - 转发视图
    - 重定向视图
    - 视图控制器view-controller
- [RESTful](#7)
- [HttpMessageConverte](#8)
    - @RequestBody
    - RequestEntity
    - @ResponseBody
    - SpringMVC处理json
    - SpringMVC处理ajax
    - @RestController注解
    - ResponseEntity
- [文件上传和下载](#9)
    - 文件下载
    - 文件上传
- [拦截器](#10)
    - 拦截器的配置
    - 拦截器的三个抽象方法
    - 多个拦截器的执行顺序
- [异常处理器](#11)
    - 基于配置的异常处理
    - 基于注解的异常处理
- [注解配置SpringMVC](#12)
    - 创建初始化类，代替web.xml
    - 创建SpringConfig配置类，代替Spring的配置文件
    - 创建WebConfig配置类，代替SpringMVC的配置文件
- [SpringMVC执行流程](#13)


# <span id="1"> SpringMVC </span>

### 1. 什么是MVC

MVC是一种软件架构的思想，将软件按照模型、视图、控制器来划分
- M: Model，模型层，指工程中的JavaBean，作用是处理数据
    - JavaBean分为两类
        - 一类称为实体类Bean：专门存储业务数据的，如Student、User等
        - 一类称为业务处理Bean：指Service或Dao对象，专门用于处理业务逻辑和数据访问。
- V: View，视图层，指工程中的html或jsp等页面，作用是与用户进行交互，展示数据
- C: Controller，控制层，指工程中的Servlet，作用是接收请求和响应浏览器。

MVC的工作流程：用户通过视图层发送请求到服务器，在服务器中请求被Controller接收，Controller调用相应的Model层处理请求，处理完毕将结果返回到Controller，Controller再根据请求处理的结果找到相应的View视图，渲染数据后最终响应给浏览器。

### 2. 什么是SpringMVC
SpringMVC是Spring的一个后续产品，是Spring的一个子项目
SpringMVC是Spring为表述层开发提供的一整套完备的解决方案。在表述层框架历经strust、webwork、strust2等诸多产品的历代更迭之后，目前业界普遍选择了SpringMVC作为javaEE项目表述层开发的首选方案。
>注：三层架构分为表述层（或表示层）、业务逻辑层、数据访问层，表述层表示前台页面和后台servlet

### 3. SpringMVC特点

- Spring家族原生产品，与IOC容器等基础设施无缝对接
- 基于原生的servlet，通过了功能强大的前端控制器DispatcherServlet，对请求和响应进行统一处理
- 表述层各细分领域需要解决的问题全方位覆盖，提供全面解决方案
- 代码清新简洁，大幅度提升开发效率
- 内部组件化程度高。可插拔式组件即插即用，想要什么功能配置相应组件即可
- 性能卓著，尤其适合现代大型、超大型互联网项目要求


# <span id="2"> HelloWorld </span>
### 1. 开发环境

- IDE
- 构建工具： maven3.6
- 服务器: tomcat8
- Spring版本: 5.3.1


### 2. 创建maven工程

- 添加web模块
- 打包方式 war
- 引入依赖

```xml

<dependencies>
<!-- SpringMVC -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.1</version>
    </dependency>
<!-- 日志 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>
<!-- ServletAPI -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
<!-- Spring5和Thymeleaf整合包 -->
    <dependency>
        <groupId>org.thymeleaf</groupId>
        <artifactId>thymeleaf-spring5</artifactId>
        <version>3.0.12.RELEASE</version>
    </dependency>
</dependencies>

```

>注：由于Maven的传递性，我们不必将所有需要的包全部配置依赖，而是配置最顶端的依赖，其他靠传递性导入


### 3 配置web.xml
注册SpringMVC的前端控制器DispacherServlet

- 1. 默认配置方式
此配置作用下，SpringMVC的配置文件默认位于WEB-INF下，默认名称为<servlet-name>-servlet.xml，例如，以下配置所对应SpringMVC的配置文件位于WEB-INF下，文件名为```SpringMVC-servlet.xml```

```xml

<!-- 配置SpringMVC的前端控制器，对浏览器发送的请求统一进行处理 -->
<servlet>
    <servlet-name>springMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>springMVC</servlet-name>
    <!--
    设置springMVC的核心控制器所能处理的请求的请求路径
    / 所匹配的请求可以是/login或.html或.js或.css方式的请求路径
    但是 / 不能匹配.jsp请求路径的请求
    -->
    <url-pattern>/</url-pattern>
</servlet-mapping>

```

- 2. 扩展配置方式
可通过init-param标签设置SpringMVC配置文件的位置和名称，通过load-on-startup标签设置SpringMVC前端控制器DispatcherServlet的初始化时间
```xml

    <!-- 配置SpringMVC的前端控制器，对浏览器发送的请求统一进行处理 -->
    <servlet>
        <servlet-name>springMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 通过初始化参数指定SpringMVC配置文件的位置和名称 -->
        <init-param>
            <!-- contextConfigLocation为固定值 -->
            <param-name>contextConfigLocation</param-name>
            <!-- 使用classpath:表示从类路径查找配置文件，例如maven工程中的
            src/main/resources -->
            <param-value>classpath:springMVC.xml</param-value>
        </init-param>
        <!--
            作为框架的核心组件，在启动过程中有大量的初始化操作要做
            而这些操作放在第一次请求时才执行会严重影响访问速度
            因此需要通过此标签将启动控制DispatcherServlet的初始化时间提前到服务器启动时
        -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springMVC</servlet-name>
        <!--
            设置springMVC的核心控制器所能处理的请求的请求路径
            /所匹配的请求可以是/login或.html或.js或.css方式的请求路径
            但是/不能匹配.jsp请求路径的请求
        -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>

```
> 注：<url-pattern>标签中使用 / 和 /* 的区别：
> /   所匹配的请求可以是/login或.html或.js或.css方式的请求路径，但是/不能匹配.jsp请求路径和请求
>     因此就可以避免在访问jsp页面时，该请求被DispacherServlet处理，从而找不到相应的页面
> /*  则能够匹配所有请求，例如在使用过滤器时，若需要对所有请求进行过滤，就需要使用 /* 的写法


### 4. 创建请求控制器
由于前端控制器对浏览器发送的请求进行了同意的处理，但是具体的请求有不同的处理过程，因此需要创建处理具体请求的类，即请求控制器。
请求控制器中每一个处理请求的方法称为控制器方法
因为SpringMVC的控制器由一个POJO（普通的java类）担任，因此需要通过@Controller注解将其标识为一个控制层组件，交给Spring的IOC容器管理，此时SpringMVC才能够识别控制器的存在。

```java

@Controller
public class HelloController {
}

```

### 5. 创建SpringMVC的配置文件
```xml

<!-- 自动扫描包 -->
<context:component-scan base-package="com.zf.mvc.controller"/>


<!-- 配置Thymeleaf视图解析器 -->
<bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
    <property name="order" value="1"/>
    <property name="characterEncoding" value="UTF-8"/>
    <property name="templateEngine">
        <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
            <property name="templateResolver">
                <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                    <!-- 视图前缀 -->
                    <property name="prefix" value="/WEB-INF/templates/"/>
                    <!-- 视图后缀 -->
                    <property name="suffix" value=".html"/>
                    <property name="templateMode" value="HTML5"/>
                    <property name="characterEncoding" value="UTF-8" />
                </bean>
            </property>
        </bean>
    </property>
</bean>



<!--
    处理静态资源，例如html、js、css、jpg
    若只设置该标签，则只能访问静态资源，其他请求则无法访问
    此时必须设置<mvc:annotation-driven/>解决问题
-->
<mvc:default-servlet-handler/>
<!-- 开启mvc注解驱动 -->
<mvc:annotation-driven>
<mvc:message-converters>
<!-- 处理响应中文内容乱码 -->
<bean class="org.springframework.http.converter.StringHttpMessageConverter">
    <property name="defaultCharset" value="UTF-8" />
    <property name="supportedMediaTypes">
        <list>
            <value>text/html</value>
            <value>application/json</value>
        </list>
    </property>
</bean>
</mvc:message-converters>
</mvc:annotation-driven>


```

### 6. 测试Hello world

- 实现对首页的访问
在请求控制器中创建处理请求的方法


```java

// @RequestMapping注解：处理请求和控制器方法之间的映射关系
// @RequestMapping注解的value属性可以通过请求地址匹配请求，/表示的当前工程的上下文路径
// localhost:8080/springMVC/
@RequestMapping("/")
public String index() {
    //设置视图名称
    return "index";
}

```

- 通过超链接跳转到指定页面
在主页index.html中设置超链接
```html

<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
    <h1>首页</h1>
    <a th:href="@{/hello}">HelloWorld</a><br/>
</body>
</html>

```

在请求控制器中创建处理请求的方法

```java

@RequestMapping("/hello")
    public String HelloWorld() {
    return "target";
}

```

### 7. 总结
浏览器发送请求，若请求地址符合前端控制器url-pattern，该请求就会被前端控制器DispatcherServlet处理。前端控制器会读取SpringMVC的核心配置文件，通过扫描组件找到控制器，将请求地址和控制器中@RequestMapping注解的value属性值进行匹配，若匹配成功，该注解所标识的控制器方法就是处理请求的方法。处理请求的方法需要返回一个字符串类型的视图名称，该视图名称会被视图解析器解析，加上前缀和后缀组成视图的路径，通过Thymeleaf对视图进行渲染，最终转发到视图所对应页面。

# <span id="3"> @RequestMapping注解 </span>

### 1. @RequestMapping注解的功能
从注解名称上我们可以看到，@RequestMapping注解的作用就是将请求和处理请求的控制器方法关联起来，建立映射关系。
SpringMVC接收到指定的请求，就会来找到在映射关系中对应的控制器方法来处理这个请求。

### 2. @RequestMapping注解的位置

@RequestMapping标识一个类：设置映射请求的请求路径的初始信息

@RequestMapping标识一个方法：设置映射请求请求路径的具体信息

```java

@Controller
//标识一个类
@RequestMapping("/test")
public class RequestMappingController {
    //标识一个方法
    //此时请求映射所映射的请求的请求路径为：/test/testRequestMapping
    @RequestMapping("/testRequestMapping")
    public String testRequestMapping() {
        return "success";
    }
}

```

### 3. @RequestMapping注解的value属性

@RequestMapping注解的value属性通过请求的请求地址匹配请求映射
@RequestMapping注解的value属性是一个字符串类型的数组，表示该请求映射能够匹配多个请求地址所对应的请求
@RequestMapping注解的value属性必须设置，至少通过请求地址匹配请求映射
```html

<a th:href="@{/testRequestMapping}">测试@RequestMapping的value属性-->/testRequestMapping</a><br>
<a th:href="@{/test}">测试@RequestMapping的value属性-->/test</a><br>

```

```java

@RequestMapping(
    value = {"/testRequestMapping", "/test"}
)
public String testRequestMapping(){
    return "success";
}

```

### 4. @RequestMapping注解的method属性
@RequestMapping注解的method属性通过请求的请求方式（get或post）匹配请求映射
@RequestMapping注解的method属性是一个ReuetsMethod类型的数组，表示该请求映射能够匹配多种请求方式的请求

若当前请求的请求地址满足请求映射的value属性，但是请求方式不满足method属性，则浏览器报错405：Request method 'POST' not supported

```html

<a th:href="@{/test}">测试@RequestMapping的value属性-->/test</a><br>
<form th:action="@{/test}" method="post">
    <input type="submit">
</form>

```

```java

@RequestMapping(
    value = {"/testRequestMapping", "/test"},
    method = {RequestMethod.GET, RequestMethod.POST}
)
public String testRequestMapping(){
    return "success";
}

```

>注：
>1. 对于处理指定请求方式的控制器方法，SpringMVC中提供了@ReuestMapping的派生注解
>处理get请求的映射->@GetMapping
>处理post请求的映射->@PostMapping
>处理put请求的映射->@PutMapping
>处理delete请求的映射->@DeleteMapping
>2. 常用的请求方式有get，post，put，delete
>但是目前浏览器只支持get和post，若在form表单提交时，为method设置了其他请求方式的字符串（put或delete），则按照默认的请求方式get处理
>若要发送put和delete请求，则需要通过spring提供的过滤器HiddenHttpMethodFilter。


### 5. @RequestMapping注解的Params属性
@RequestMapping注解的params属性通过请求的请求参数匹配请求映射
@ReqeustMapping注解的params属性是一个字符串类型的数组，可以通过四种表达式设置请求参数和请求映射的匹配关系

- "param" : 要求请求映射所匹配的请求必须携带param请求参数
- "!param"：要求请求映射所匹配的请求不能携带param请求参数
- "param=value"：要求请求映射所匹配的请求必须携带param请求参数且param=value
- "param!=value"：要求请求映射所匹配的请求必须携带param请求参数但是param!=value

```html

<a th:href="@{/test(username='admin',password=123456)">测试@RequestMapping的params属性-->/test</a><br>

```

```java

@RequestMapping(
    value = {"/testRequestMapping", "/test"}
    ,method = {RequestMethod.GET, RequestMethod.POST}
    ,params = {"username","password!=123456"}
)
public String testRequestMapping(){
    return "success";
}

```

>注：
>若当前请求满足@RequestMapping注解的value和method属性，但是不满足params属性，此时页面会报错400: Parameter conditions "username, password!=123456" not met for actual request parameters: username={admin}, password={123456}


### 6. @RequestMapping注解的headers属性

@RequestMapping注解的headers属性通过请求的请求头信息匹配请求映射

基本和param属性用法相同

>注：
>若当前请求满足@RequestMapping注解的value和method属性，但是不满足headers属性，此时页面显示404错误，即资源未找到


### 7. SpringMVC支持ant风格的路径

- ? : 表示任意的单个字符
- * : 表示任意的0个或多个字符
- ** : 表示任意的一层或多层目录

> 在使用 ** 时，只能使用/** /xxx的方式

### 8. SpringMVC支持路径中的占位符（重点）
原始方式： /deleteUser?id=1
rest方式： /deleteUser/1

SpringMVC路径中的占位符常用RESTful风格中，当请求路径中将某些数据通过路径的方式传输到服务器中，就可以在相应的@ReuestMapping注解的value属性中通过占位符{xxx}表示传输的数据，在通过@ParamVariable注解，将占位符所表示的数据赋值给控制器方法的形参

```html

<a th:href="@{/testRest/1/admin}">测试路径中的占位符-->/testRest</a><br>

```

```java

@RequestMapping("/testRest/{id}/{username}")
public String testRest(@PathVariable("id") String id, @PathVariable("username") String username){
    System.out.println("id:"+id+",username:"+username);
    return "success";
}
//最终输出的内容为-->id:1,username:admin

```

# <span id="4">SpringMVC获取请求参数 </span>

### 1. 通过ServletAPI获取
将HttpServletRequest作为控制器方法的形参，此时HttpServletRequest类型的参数表示封装了当前请求的请求报文的对象。

```java
@RequestMapping("/testParam") 
public String testParam(HttpServletRequest request){ 
	String username = request.getParameter("username"); 
	String password = request.getParameter("password"); 
	System.out.println("username:"+username+",password:"+password); return "success"; 
}
```

### 2. 通过控制器方法的形参获取请求参数
在控制器方法的形参位置，设置和请求参数同名的形参，当浏览器发送请求，匹配到请求映射时，在DispatcherServlet中就会将请求参数赋值给相应的形参

```html
<a th:href="@{/testParam(username='admin',password=123456)}">测试获取请求参数-- >/testParam</a><br>
```

```java

@RequestMapping("/testParam")
public String testParam(String username, String password){
	System.out.println("username:"+username+",password:"+password);
	return "success";
}
```

> 注：
> 若请求所传输的请求参数中有多个同名的请求参数，此时可以在控制器方法的形参中设置字符串数组或者字符串类型的形参接收此请求参数
> 若使用字符串数组类型的形参，此参数的数组中包含了每一个数据
> 若使用字符串类型的形参，此参数的值为每个数据中间使用逗号拼接的结果

### 3. @RequestParam

@RequestParam 是将请求参数和控制器方法的形参创建映射关系
@RequestParam注解一共有三个属性
	- value : 指定为形参赋值的请求参数的参数名
	- required : 设置是否必须传输此参数，默认值为true
		- 若设置为true时，则当前请求必须传输value所指定的请求参数，若没有传输此参数，且没有设置defaultValue属性，则页面报错400：Required String parameter 'xxx' is not present；
		- 若设置为false，则当前请求不是必须传输value所指定的请求参数，若没有传输，则注解所表示的形参的值为null
	- defaultValue: 不管required属性值为true还是false，当value所指定的请求参数没有传输或传输的值为""时，则使用默认值为形参赋值

### 4. @RequestHeader
@RequestHeader是将请求头信息的控制器方法的形参创建映射关系
@RequestHeader注解一共有三个属性：value、required、defaultValue，用法同@ReqeustParam

### 5. @CookieValue
@RequestHeader是将Cookie信息的控制器方法的形参创建映射关系
@RequestHeader注解一共有三个属性：value、required、defaultValue，用法同@ReqeustParam

### 6. 通过pojo获取请求参数
可以在控制器方法的形参位置设置一个实体类类型的形参，若此时浏览器传输的请求参数的参数名和实体类中的属性名一致，那么请求参数就会为此属性赋值。


```html
<form th:action="@{/testpojo}" method="post"> 
    用户名：<input type="text" name="username"><br> 
    密码：<input type="password" name="password"><br> 
    性别：<input type="radio" name="sex" value="男">男     <input type="radio" name="sex" value="女">女<br> 
    年龄：<input type="text" name="age"><br> 
    邮箱：<input type="text" name="email"><br>
    <input type="submit">
</form>
```

```java
@RequestMapping("/testpojo")
public String testPOJO(User user){
	System.out.println(user);
	return "success";
}
//最终结果-->User{id=null, username='张三', password='123', age=23, sex='男', email='123@qq.com'}

```

### 7. 解决获取请求参数的乱码问题
解决获取请求参数的乱码问题，可以使用SpringMVC提供的编码过滤器CharacterEncodingFilter，但是必须在web.xml中进行注册

```xml
<!--配置springMVC的编码过滤器-->
<filter>
	<filter-name>CharacterEncodingFilter</filter-name>
	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
	<init-param>
	<param-name>encoding</param-name>
	<param-value>UTF-8</param-value>
	</init-param>
	<init-param>
	<param-name>forceResponseEncoding</param-name>
	<param-value>true</param-value>
	</init-param>
</filter>
<filter-mapping>
	<filter-name>CharacterEncodingFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
```
> 注
> SpringMVC中处理编码的过滤器一定要配置到其他过滤器之前，否则无效

# <span id="5">域对象共享数据 </span>

### 1. 使用ServletAPI向request域对象共享数据

```java
@RequestMapping("/testServletAPI")
public String testServletAPI(HttpServletRequest request){
    request.setAttribute("testScope", "hello,servletAPI");
    return "success";
}
```

### 2. 使用ModelAndView向request域对象共享数据

```java
@RequestMapping("/testModelAndView")
public ModelAndView testModelAndView(){
	/**
	* ModelAndView有Model和View的功能
	* Model主要用于向请求域共享数据
	* View主要用于设置视图，实现页面跳转
	*/
	ModelAndView mav = new ModelAndView();
	//向请求域共享数据
	mav.addObject("testScope", "hello,ModelAndView");
	//设置视图，实现页面跳转
	mav.setViewName("success");
	return mav;
}
```
### 3. 使用Model向request域对象共享数据

```java
@RequestMapping("/testModel")
public String testModel(Model model){
	model.addAttribute("testScope", "hello,Model");
	return "success";
}
```

### 4. 使用map向request域对象共享数据

```java
@RequestMapping("/testMap")
public String testMap(Map<String, Object> map){
	map.put("testScope", "hello,Map");
	return "success";
}

```

### 5. 使用ModelMap向request域对象共享数据

```java
@RequestMapping("/testModelMap")
public String testModelMap(ModelMap modelMap){
	modelMap.addAttribute("testScope", "hello,ModelMap");
	return "success";
}

```

### 6. Model、ModelMap、Map的关系
Model、ModelMap、Map类型的参数其实本质上都是```BindingAwareModelMap```类型

```java
public interface Model{}
public class ModelMap extends LinkedHashMap<String, Object> {}
public class ExtendedModelMap extends ModelMap implements Model {}
public class BindingAwareModelMap extends ExtendedModelMap {}
```

### 7. 向Session域共享数据

```java

@RequestMapping("/testSession")
public String testSession(HttpSession session){
	session.setAttribute("testSessionScope", "hello,session");
	return "success";
}

```

### 8. 向application域共享数据

```java
@RequestMapping("/testApplication")
public String testApplication(HttpSession session){
	ServletContext application = session.getServletContext();
	application.setAttribute("testApplicationScope", "hello,application");
	return "success";
}
```

# <span id="6"> SpringMVC的视图</span>
SpringMVC中的视图是View接口，视图的作用是渲染数据，将模型Model中的数据展示给用户
SpringMVC视图的种类很多，默认有转发视图和重定向视图
当工程引入```jstl```的依赖，转发视图会自动转换为```jstlView```
若使用的视图技术为```Thymeleaf```，在SpringMVC的配置i文件中配置了```Thymeleaf```的视图解析器，由此视图解析器解析之后i所得到的是```ThymeleafView```

### 1. ```ThymeleafView```
当控制器方法中所设置的视图名称没有任何前缀时，此时的视图名称会被SpringMVC配置文件中所配置的视图解析器解析，视图名称拼接视图前缀和视图后缀所得到的最终路径，会通过**转发的方式**实现跳转

```java

@RequestMapping("/testHello")
	public String testHello(){
	return "hello";
}
```
![SpringMVC视图解析](https://img-blog.csdnimg.cn/cf791f19a09a4efa9de2460bdb5a0347.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)

### 2. 转发视图
SpringMVC中默认的转发视图是```InternalResourceView```
SpringMVC中创建转发是图情况：
当控制器方法中所设置的视图名称以```forward```为前缀时，创建```InternalResourceView```视图，此时的视图名称不会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀```forward```去掉，剩余部分作为最终路径通过转发的方式实现跳转

例如```forward:/```，```forward:/employee```

```java

@RequestMapping("/testForward")
public String testForward(){
	return "forward:/testHello";
}
```
![SpringMVC转发视图](https://img-blog.csdnimg.cn/d96f6f494ba249afa40fa08c258d6a5d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
### 3. 重定向视图
SpringMVC中默认的重定向视图是```RedirectView```
当控制器方法中所设置的视图名称以```redirect```为前缀时，创建RedirectView视图，此时的视图名称不会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀```redirect```去掉，剩余部分作为最终路径通过重定向的方式实现跳转

例如```redirect:/```，```redirect:/employee```

```java

@RequestMapping("/testRedirect")
public String testRedirect(){
	return "redirect:/testHello";
}

```

![SpringMVC重定向](https://img-blog.csdnimg.cn/712da68b04b5481b851fafee024a82af.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)


> 注
> 重定向视图在解析时，会先将redirect:前缀去掉，然后会判断剩余部分是否以/开头，若是，则会自动拼接上下文路径

### 4. 视图控制器 ```view-controller```
当控制器方法中，仅仅用来实现页面按跳转，即只需要设置视图名称时，可以将处理器方法替换为 在SpringMVC配置文件中使用```view-controller```标签进行表示

```xml
<!--
	path：设置处理的请求地址
	view-name：设置请求地址所对应的视图名称
-->
<mvc:view-controller path="/testView" view-name="success"></mvc:view-controller>
```

> 注
> 当SpringMVC中设置任何一个view-controller时，其他控制器中的请求映射将全部失效，此时需要在SpringMVC的核心配置文件中设置开启mvc注解驱动的标签：
> ```<mvc:annotation-driven />```


# <span id="7"> RESTful</span>
### 1. RESTful简介
REST:Representational Satte Transfer，表现层资源状态转移

- **资源**
	- 资源是一种看待服务器的方式，即，将服务器看作是由很多离散的资源组成。每个资源是服务器上一个可命名的抽象概念。因为资源是一个抽象的概念，所以它不仅仅能代表服务器文件系统中的一个文件、数据库中的一张表等等具体的东西，可以将资源设计的要多抽象有多抽象，只要想象力允许而且客户端应用开发者能够理解。与面向对像想设计类似，资源是以名词为核心来组织的，首先关注的是名词。一个资源可以由一个或多个URI来标识。URI即是资源的名称，也是资源在web上的地址。对某个资源感兴趣的客户端应用，可以通过资源的URI与其进行交互。

- **资源的表述**
	- 资源的表述是一段对于资源在某个特定时刻的状态描述。可以在客户端-服务端之间转移（交换）。资源的表述可以由多种格式，例如HTML/XML/JSON/纯文本/图片/视频/音频等等。资源的表述格式可以通过协商机制来去欸的那个。请求-响应方法的表述通常使用不同的格式

- **状态转移**
	- 状态转移说的是：在客户端和服务器段之间转移代表资源状态的表述。通过转移和操作资源的表述，来简介实现操作资源的目的。

### 2.RESTful的实现
具体说，就是HTTP协议里面，四个标识操作方式的动词：GET、POST、PUT、DELETE。
他们分别对应四种基本操作：GET用来获取资源，POST用来新建资源、PUT用来更新资源，DELETE用来删除资源
REST风格提倡URL地址使用同意的风格设计，从前到后个个单词使用斜杠分开，不适用问号键值对方式携带请求参数，而是将要发送给服务器的数据作为URL地址的一部分，以保证整体风格的一致性。

| 操作 | 传统方式 | REST风格 | 
|--|--|--|
| 查询操作  | getUserById?id=1 | user/1->get | 
| 保存操作 | saveUser | user->post | 
| 删除操作 | deleteUser?id=1 | user/1->delete | 
| 更新操作 | updateUser?id=1 |user/1->put  | 

###  3. ```HiddenHttpMethodFilter```
由于浏览器只支持发送get和post方式的请求，那么该如何发送put和delete请求呢？
SpringMVC提供了```HiddenHttpMethodFilter```帮助我们**将POST请求转换为DELETE或PUT请求**
```HiddenHttpMethodFilter```处理put和delete请求的条件：

- 当前请求的请求方式必须为```post```
- 当前请求必须传输请求参数```_method```

满足以上条件，```HiddenHttpMethodFilter```过滤i就会将当前请求的请求方式转换为请求参数```_method```的值，因此请求参数```_method```的值才是最终的请求方式

在web.xml中注册```HiddenHttpMethodFilter```

```xml
<filter>
	<filter-name>HiddenHttpMethodFilter</filter-name>
	<filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
	<filter-name>HiddenHttpMethodFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>

```

> 注
> 目前为止，SpringMVC中提供了两个过滤器：```HiddenHttpMethodFilter```和```CharacterEncodingFilter```
> 在web.xml中注册时，必须先注册```CharacterEncodingFilter```，再注册```HiddenHttpMethodFilter```
> 因为
> - 在```CharacterEncodingFilter```中通过request.setCharacterEncoding(encoding)方法设置字符集
> - request.setCharacterEncoding(encoding)方法要求前面不能有任何获取请求参数的操作
> - 而```HiddenHttpMethodFilter```恰恰有一个获取请求方式的操作
> `String paramValue = request.getParameter(this.methodParam);`


# <span id="8"> ```HttpMessageConverter```</span>
```HttpMessageConverter```，报文信息转换器，将请求报文转换为java对象，或将java对象转换为响应报文

```HttpMessageConverter```提供了两个注解和两个类型：

- ```@RequestBody```
- ```@ResponseBody```
- ```ReqeustEntity```
- ```ResponseEntity```

### 1. @RequestBody
@RequestBody可以获取请求体，需要在控制器方法设置一个形参，使用@ReqeustBody进行标识，当前请求的请求体就会为当前注解所表示的形参赋值

```html
<form th:action="@{/testRequestBody}" method="post">
	用户名：<input type="text" name="username"><br>  <!-- 输入admin -->
	密码：<input type="password" name="password"><br>  <!-- 输入123456 -->
	<input type="submit">
</form>
```

```java

@RequestMapping("/testRequestBody")
public String testRequestBody(@RequestBody String requestBody){
	System.out.println("requestBody:"+requestBody);
	return "success";
}
//输出： requestBody:username=admin&password=123456
```

### 2. RequestEntity
RequestEntity封装请求报文的一种类型，需要在控制器方法的形参中设置该类型的形参，当前请求的请求报文就会赋值给该形参，可以通过getHeaders()获取请求头信息，通过getBody()后去请求提信息

```java

@RequestMapping("/testRequestEntity")
public String testRequestEntity(RequestEntity<String> requestEntity){
	System.out.println("requestHeader:"+requestEntity.getHeaders());
	System.out.println("requestBody:"+requestEntity.getBody());
	return "success";
}
```
### 3. @ResponseBody
@ResponseBody用于标识一个控制器方法，可以将该方法的返回值直接作为响应报文的响应体响应到浏览器

```java
@RequestMapping("/testResponseBody")
@ResponseBody
public String testResponseBody(){
	return "success";
}
//结果：浏览器页面显示success
```
### 4. SpringMVC处理json
@Response处理json的步骤

- 导入jackson依赖

```xml

<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-databind</artifactId>
	<version>2.12.1</version>
</dependency>

```

- 在SpringMVC的核心配置文件中开启mvc的注解驱动，此时在HandlerAdaptor中会自动装配一个消息转换器: ```MappingJackson2HttpMessageConverter```，可以将响应到浏览器的java对象转换为json格式的字符串

```xml
<mvc:annotation-driven />
```

- 在处理器方法上使用@ResponseBody注解进行标识
- 将java对象直接作为控制器方法的返回值返回，就会自动转换为json格式的字符串

```java

@RequestMapping("/testResponseUser")
@ResponseBody
public User testResponseUser(){
	return new User(1001,"admin","123456",23,"男");
}

// 浏览器的页面中展示的结果：{"id":1001,"username":"admin","password":"123456","age":23,"sex":"男"}
```

### 5. SpringMVC处理ajax

- 请求超链接

```html
<div id="app">
    <a th:href="@{/testAjax}" @click="testAjax">testAjax</a><br>
</div>
```

- 通过vue和axios处理点击事件

```html
<script type="text/javascript" th:src="@{/static/js/vue.js}"></script>
<script type="text/javascript" th:src="@{/static/js/axios.min.js}"></script>
<script type="text/javascript">
var vue = new Vue({
    el:"#app",
    methods:{
        testAjax:function (event) {
        axios({
            method:"post",
            url:event.target.href,
            params:{
                username:"admin",
                password:"123456"
        }
        }).then(function (response) {
            alert(response.data);
        });
        event.preventDefault();
        }
    }
});
</script>
```

- 控制器方法

```java
@RequestMapping("/testAjax")
@ResponseBody
public String testAjax(String username, String password){
	System.out.println("username:"+username+",password:"+password);
	return "hello,ajax";
}

```

### 6. @RestController注解
@RestController注解时SpringMVC提供的一个复合注解，标识在控制器的类上，就相当于为类添加了@Controller注解，并且为其中每个方法添加了@ResponseBody注解

### 7. ResponseEntity
ResponseEntity用于控制器方法的返回值类型，该控制器方法的返回值就是响应到浏览器的响应报文

# <span id="9"> 文件上传和下载 </span>

### 1. 文件下载
使用ResponseEntity实现下载问价你的功能

```java
@RequestMapping("/testDown")
public ResponseEntity<byte[]> testResponseEntity(HttpSession session) throws IOException {
	//获取ServletContext对象
	ServletContext servletContext = session.getServletContext();
	//获取服务器中文件的真实路径
	String realPath = servletContext.getRealPath("/static/img/1.jpg");
	//创建输入流
	InputStream is = new FileInputStream(realPath);
	//创建字节数组
	byte[] bytes = new byte[is.available()];
	//将流读到字节数组中
	is.read(bytes);
	//创建HttpHeaders对象设置响应头信息
	MultiValueMap<String, String> headers = new HttpHeaders();
	//设置要下载方式以及下载文件的名字
	headers.add("Content-Disposition", "attachment;filename=1.jpg");
	//设置响应状态码
	HttpStatus statusCode = HttpStatus.OK;
	//创建ResponseEntity对象
	ResponseEntity<byte[]> responseEntity = new ResponseEntity<>(bytes, headers, statusCode);
	//关闭输入流
	is.close();
	return responseEntity;
}

```

### 2. 文件上传
文件上传要求form表单的请求方式必须为post，并且添加属性enctype="multipart/form-data"
SpringMVC中将上传的文件封装到MultipartFile对象中，通过此对象可以获取文件相关信息
上传步骤

- 添加依赖

```xml
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
	<groupId>commons-fileupload</groupId>
	<artifactId>commons-fileupload</artifactId>
	<version>1.3.1</version>
</dependency>
```

- 在SpringMVC的配置文件中添加配置：

```xml
<!--必须通过文件解析器的解析才能将文件转换为MultipartFile对象-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
</bean>

```

- 控制器方法

```java
@RequestMapping("/testUp")
public String testUp(MultipartFile photo, HttpSession session) throws IOException {
	//获取上传的文件的文件名
	String fileName = photo.getOriginalFilename();
	//处理文件重名问题
	String hzName = fileName.substring(fileName.lastIndexOf("."));
	
	fileName = UUID.randomUUID().toString() + hzName;
	//获取服务器中photo目录的路径
	ServletContext servletContext = session.getServletContext();
	String photoPath = servletContext.getRealPath("photo");
	File file = new File(photoPath);
	if(!file.exists()){
		file.mkdir();
	}
	String finalPath = photoPath + File.separator + fileName;
	//实现上传功能
	photo.transferTo(new File(finalPath));
	return "success";
}
```

# <span id="10"> 拦截器 </span>

### 1. 拦截器的配置
SpringMVC中的拦截器用于拦截控制器方法的执行
SpringMVC中的拦截器需要实现```HandlerInterceptor```
SpringMVC的拦截器必须在SpringMVC的配置文件中进行配置

```xml

<bean class="com.zf.interceptor.FirstInterceptor"></bean>
<ref bean="firstInterceptor"></ref>
<!-- 以上两种配置方式都是对DispatcherServlet所处理的所有的请求进行拦截 -->
<mvc:interceptor>
    <mvc:mapping path="/**"/>
    <mvc:exclude-mapping path="/testRequestEntity"/>
    <ref bean="firstInterceptor"></ref>
</mvc:interceptor>
<!--
    以上配置方式可以通过ref或bean标签设置拦截器，通过mvc:mapping设置需要拦截的请求，通过
    mvc:exclude-mapping设置需要排除的请求，即不需要拦截的请求
-->

```

### 2. 拦截器的三个抽象方法
SpringMVC中的拦截器有三个抽象方法：
- ```preHandle```： 控制器方法执行preHandle()，其中boolean类型的返回值表示是否拦截或放行，返回true为放行，即调用控制器方法；返回false表示拦截。
- ```postHandle```：控制器方法执行之后执行postHandle()
- ```afterComplation```：处理完视图和模型数据，渲染视图完毕之后执行afterComplation()

### 3. 多个拦截器的执行顺序
1.  若每个拦截器的preHandle()都返回true
	 此时多个拦截器的执行顺序和拦截器的在SpringMVC的配置文件的配置顺序有关
	 preHandle()会按照配置的顺序执行，而postHandler()和afterCompaltion会按照配置的反序执行
2. 若某个拦截器的preHandle()返回了false
	preHandler()返回false和它之前的拦截器的preHandle()都会执行，postHandle()都不执行，返回false之前的拦截器的afterComplation()会执行


# <span id="11"> 异常处理器</span>

### 1. 基于配置的异常处理
SpringMVC提供了一个处理控制器方法执行过程中所出现的异常的接口：```HandlerExceptionResolver```
```HandlerExceptionResolver```接口的实现类有：
	- ```DefaultHandlerExceptionResolver```
	- ```SimpleMappingExceptionResolver```

SpringMVC提供了自定义的异常处理器```SimpleMappingExceptionResolver```，使用方式：

```xml
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    <property name="exceptionMappings">
        <props>
            <!--
                properties的键表示处理器方法执行过程中出现的异常
                properties的值表示若出现指定异常时，设置一个新的视图名称，跳转到指定页面
            -->
            <prop key="java.lang.ArithmeticException">error</prop>
        </props>
    </property>
    <!--
        exceptionAttribute属性设置一个属性名，将出现的异常信息在请求域中进行共享
    -->
    <property name="exceptionAttribute" value="ex"></property>
</bean>

```

### 2.基于注解的异常处理

```java

//@ControllerAdvice将当前类标识为异常处理的组件
@ControllerAdvice
public class ExceptionController {
    //@ExceptionHandler用于设置所标识方法处理的异常
    @ExceptionHandler(ArithmeticException.class)
    //ex表示当前请求处理中出现的异常对象
    public String handleArithmeticException(Exception ex, Model model){
        model.addAttribute("ex", ex);
        return "error";
    }
}

```

# <span id="12"> 注解配置SpringMVC </span>
使用配置类和注解代替web.xml和SpringMVC配置文件

### 1. 创建初始化类，代替web.xml
在Servlet3.0环境中，容器会在类路径中查找实现```javax.servlet.ServletContainerInitializer```接口的类，如果找到的话就用他来配置Servlet容器。Spring提供了这个接口的实现，名为```SpringServletContainerInitializer```，这个类反过来又会查找实现```WebApplicationInitializer```的类并将配置的任务交给他们来完成。Spring3.2引入了一个遍历的```WebApplicationInitializer```基础实现，名为```AbstractAnnotationConfigDispatcherServletInitializer```，当我们的类扩展了```AbstractAnnotationConfigDispatcherServletInitializer```并将其部署到servlet3.0容器的时候，容器会自动发现它，并用它来配置servlet上下文。

```java
public class WebInit extends AbstractAnnotationConfigDispatcherServletInitializer {
	/**
	* 指定spring的配置类
	* @return
	*/
	@Override
	protected Class<?>[] getRootConfigClasses() {
		return new Class[]{SpringConfig.class};
	}
	/**
	* 指定SpringMVC的配置类
	* @return
	*/
	@Override
	protected Class<?>[] getServletConfigClasses() {
		return new Class[]{WebConfig.class};
	}
	/**
	* 指定DispatcherServlet的映射规则，即url-pattern
	* @return
	*/
	@Override
	protected String[] getServletMappings() {
		return new String[]{"/"};
	}
	/**
	* 添加过滤器
	* @return
	*/
	@Override
	protected Filter[] getServletFilters() {
		CharacterEncodingFilter encodingFilter = new CharacterEncodingFilter();
		encodingFilter.setEncoding("UTF-8");
		encodingFilter.setForceRequestEncoding(true);
		HiddenHttpMethodFilter hiddenHttpMethodFilter = new HiddenHttpMethodFilter();
		return new Filter[]{encodingFilter, hiddenHttpMethodFilter};
	}
}

```

### 2. 创建SpringConfig配置类，代替Spring的配置文件

```java
@Configuration public class SpringConfig { 
	//ssm整合之后，spring的配置信息写在此类中 
}
```

### 3. 创建WebConfig配置类，代替SpringMVC的配置文件

```java
@Configuration
//扫描组件
@ComponentScan("com.atguigu.mvc.controller")
//开启MVC注解驱动
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {
	//使用默认的servlet处理静态资源
	@Override
	public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
		configurer.enable();
	}
	//配置文件上传解析器
	@Bean
	public CommonsMultipartResolver multipartResolver(){
		return new CommonsMultipartResolver();
	}
	//配置拦截器
	@Override
	public void addInterceptors(InterceptorRegistry registry) {
		FirstInterceptor firstInterceptor = new FirstInterceptor();
		registry.addInterceptor(firstInterceptor).addPathPatterns("/**");
	}
	//配置视图控制
	/*@Override
	public void addViewControllers(ViewControllerRegistry registry) {
	registry.addViewController("/").setViewName("index");
	}*/
	//配置异常映射
	/*@Override
	public void
	configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
	SimpleMappingExceptionResolver exceptionResolver = new
	SimpleMappingExceptionResolver();
	Properties prop = new Properties();
	prop.setProperty("java.lang.ArithmeticException", "error");
	//设置异常映射
	exceptionResolver.setExceptionMappings(prop);
	//设置共享异常信息的键
	exceptionResolver.setExceptionAttribute("ex");
	resolvers.add(exceptionResolver);
	}*/
	
	//配置生成模板解析器
	@Bean
	public ITemplateResolver templateResolver() {
		WebApplicationContext webApplicationContext = ContextLoader.getCurrentWebApplicationContext();
		// ServletContextTemplateResolver需要一个ServletContext作为构造参数，可通过WebApplicationContext 的方法获得
		ServletContextTemplateResolver templateResolver = new ServletContextTemplateResolver(
		webApplicationContext.getServletContext());
		templateResolver.setPrefix("/WEB-INF/templates/");
		templateResolver.setSuffix(".html");
		templateResolver.setCharacterEncoding("UTF-8");
		templateResolver.setTemplateMode(TemplateMode.HTML);
		return templateResolver;
	}
	//生成模板引擎并为模板引擎注入模板解析器
	@Bean
	public SpringTemplateEngine templateEngine(ITemplateResolver templateResolver) {
		SpringTemplateEngine templateEngine = new SpringTemplateEngine();
		templateEngine.setTemplateResolver(templateResolver);
		return templateEngine;
	}
	//生成视图解析器并未解析器注入模板引擎
	@Bean
	public ViewResolver viewResolver(SpringTemplateEngine templateEngine) {
		ThymeleafViewResolver viewResolver = new ThymeleafViewResolver();
		viewResolver.setCharacterEncoding("UTF-8");
		viewResolver.setTemplateEngine(templateEngine);
		return viewResolver;
	}
}
```
