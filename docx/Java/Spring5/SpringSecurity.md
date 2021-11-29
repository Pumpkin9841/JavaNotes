# SpringSecurity

### 1 SpringSecurity框架

Spring 是非常流行和成功的 Java 应用开发框架，Spring Security 正是 Spring 家族中的成员。Spring Security 基于 Spring 框架，提供了一套 Web 应用安全性的完整解决方案。


正如你可能知道的关于安全方面的两个主要区域是“认证”和“授权”（或者访问控制），一般来说，Web 应用的安全性包括用户认证（Authentication）和用户授权（Authorization）两个部分，这两点也是 Spring Security 重要核心功能。

1. 用户认证指的是：验证某个用户是否为系统中的合法主体，也就是说用户能否访问该系统。用户认证一般要求用户提供用户名和密码。系统通过校验用户名和密码来完成认证过程。通俗点说就是系统认为用户是否能登录。
2. 用户授权指的是验证某个用户是否有权限执行某个操作。在一个系统中，不同用户所具有的权限是不同的。比如对一个文件来说，有的用户只能进行读取，而有的用户可以进行修改。一般来说，系统会为不同的用户分配不同的角色，而每个角色则对应一系列的权限。通俗点讲就是系统判断用户是否有权限去做某些事情。


### 2 同款产品对比

#### ```Spring Security```
Spring技术栈的组成部分。
Spring Security特点

- 和Spring无缝整合
- 全面的权限控制
- 专门为web开发而设计
- 重量级（需要引入很多依赖）

#### ```Shiro```
Apache旗下的轻量级权限控制框架

特点

- 轻量级。Shiro主张的理念是把复杂的事情变简单。针对对性能有更高要求的互联网应用有更好表现
- 通用性
	- 好处：不局限于web环境，可以脱离web环境使用
	- 缺陷：在web环境下一些特定的需求需要手动编写代码定制

Spring Security 是 Spring 家族中的一个安全管理框架，实际上，在 Spring Boot 出现之前，Spring Security 就已经发展了多年了，但是使用的并不多，安全管理这个领域，一直是 Shiro 的天下。

相对于 Shiro，在 SSM 中整合 Spring Security 都是比较麻烦的操作，所以，Spring Security 虽然功能比 Shiro 强大，但是使用反而没有 Shiro 多（Shiro 虽然功能没有Spring Security 多，但是对于大部分项目而言，Shiro 也够用了）。

自从有了 Spring Boot 之后，Spring Boot 对于 Spring Security 提供了自动化配置方案，可以使用更少的配置来使用 Spring Security。

因此，一般来说，常见的安全管理技术栈的组合是这样的：

- SSM + Shiro
- Spring Boot/Spring Cloud + Spring Security
以上只是推荐的组合而已，如果单纯从技术上来说，无论怎么组合，都是可以运行的。


### 3. Spring Security 入门案例

1. 创建一个项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/6b822c91b427493dbc69d7399fb3abb7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
2. ![在这里插入图片描述](https://img-blog.csdnimg.cn/12dce6faed8a41d38479daee9828d8ed.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
3. ![在这里插入图片描述](https://img-blog.csdnimg.cn/a622879d95e6479b8833d4a129e019ad.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
4. 编写controller进行测试

```java
@RestController
@RequestMapping("/test")
public class HelloController {
    @GetMapping("test")
    public String hello(){
        return "hello security" ;
    }
 }
```

5. 浏览器输入```localhost:8080/test/hello```跳转到认证页面
![在这里插入图片描述](https://img-blog.csdnimg.cn/1ec1e17fe9364516ac27f50ce413ae72.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_14,color_FFFFFF,t_70,g_se,x_16)
6. 默认用户名为root
    默认密码在启动时 显示在控制台（每次启动都会不一样）： 
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/3d5b93f6bf314d3d919f0c9cce1515d7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
###  Spring Security 原理
SpringSecurityy本质是一个过滤器链：
启动时是可以获取到过滤器链的：

```java
org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFilter
org.springframework.security.web.context.SecurityContextPersistenceFilter 
org.springframework.security.web.header.HeaderWriterFilter
org.springframework.security.web.csrf.CsrfFilter
org.springframework.security.web.authentication.logout.LogoutFilter 
org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter 
org.springframework.security.web.authentication.ui.DefaultLoginPageGeneratingFilter 
org.springframework.security.web.authentication.ui.DefaultLogoutPageGeneratingFilter
org.springframework.security.web.savedrequest.RequestCacheAwareFilter
org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter
org.springframework.security.web.authentication.AnonymousAuthenticationFilter 
org.springframework.security.web.session.SessionManagementFilter 
org.springframework.security.web.access.ExceptionTranslationFilter 
org.springframework.security.web.access.intercept.FilterSecurityInterceptor
```
代码底层流程：重点看三个过滤器

- ```FilterSecurityInterceptor```: 是一个方法级的权限过滤器, 基本位于过滤链的最底部。
	- ![在这里插入图片描述](https://img-blog.csdnimg.cn/53431aea54114df3ac777bd09ec79efb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
	- ```super.beforeInvocation(fi)```: 表示查看之前的 filter 是否通过。
	- ```fi.getChain().doFilter(fi.getRequest(), fi.getResponse());```: 表示真正的调用后台的服务。



- ```ExceptionTranslationFilter```: 是个异常过滤器，用来处理在认证授权过程中抛出的异常
	- ![在这里插入图片描述](https://img-blog.csdnimg.cn/90d188cb2cd445fe8a72eea56fe0372d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)

- ```UsernamePasswordAuthenticationFilter```: 对/login 的 POST 请求做拦截，校验表单中用户名，密码。
	- ![在这里插入图片描述](https://img-blog.csdnimg.cn/7804d8c80cbe4bc6bb4d4cafa97b444c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)


![在这里插入图片描述](https://img-blog.csdnimg.cn/72cf93aa3fbb46c294083c910d40c220.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)

### ```UserDetailsService```接口讲解
当什么也没有配置的时候，账号和密码由SpringSecurity定义生成的。而在实际项目中，账号和密码都是从数据库中查询出来的。所以我们要通过自定义逻辑控制认证逻辑。

如果需要自定义逻辑时，只需要实现UserDetailsService接口即可。接口定义如下

![在这里插入图片描述](https://img-blog.csdnimg.cn/03238e9a48d04b1ca071821ca3c23249.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_19,color_FFFFFF,t_70,g_se,x_16)

#### 返回值```UserDetails```

这个类是系统默认的用户"主体"

```java
// 表示获取登录用户所有权限
Collection<? extends GrantedAuthority> getAuthorities();
// 表示获取密码
String getPassword();
// 表示获取用户名
String getUsername();
// 表示判断账户是否过期
boolean isAccountNonExpired();
// 表示判断账户是否被锁定
boolean isAccountNonLocked();
// 表示凭证{密码}是否过期
boolean isCredentialsNonExpired();
// 表示当前用户是否可用
boolean isEnabled();
```
下面是UserDetails的实现类
![在这里插入图片描述](https://img-blog.csdnimg.cn/5ed1087110a24ff1b1f41f16821b1f06.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
以后只需要使用```User```这个实现类即可！


#### ```PasswordEncoder```接口讲解

```java
// 表示把参数按照特定的解析规则进行解析
String encode(CharSequence rawPassword);
// 表示验证从存储中获取的编码密码与编码后提交的原始密码是否匹配。如果密码匹配，则返回 true；如果不匹配，则返回 false。第一个参数表示需要被解析的密码。第二个参数表示存储的密码。
boolean matches(CharSequence rawPassword, String encodedPassword);
// 表示如果解析的密码能够再次进行解析且达到更安全的结果则返回 true，否则返回false。默认返回 false。
default boolean upgradeEncoding(String encodedPassword) {
return false
```
接口实现类
![在这里插入图片描述](https://img-blog.csdnimg.cn/071ceb4f33e74449ba64132f61b0fc70.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_19,color_FFFFFF,t_70,g_se,x_16)

BCryptPasswordEncoder 是 Spring Security 官方推荐的密码解析器，平时多使用这个解析器。
BCryptPasswordEncoder 是对 bcrypt 强散列方法的具体实现。是基于 Hash 算法实现的单向加密。可以通过 strength 控制加密强度，默认 10.

### 4 SpringSecurity Web 权限方案

### 认证

#### 设置登陆系统的账号、密码
- 设置登陆系统的账号、密码有三种方式
	- 通过配置文件```application.properties```设置
	- 通过配置类
	- **自定义编写实现类（通常使用这种方式读取数据库中的账号信息）**

#### 方式一：通过配置文件
在```application.properties```中添加代码
```properties
# 设置登陆的用户和密码 第一种方式 配置文件
spring.security.user.name=zf
spring.security.user.password=123456
```

#### 方式二：通过配置类

新建一个类添加```@Configuration```注解表示被Spring认为是一个配置类，实现```WebSecurityConfigurerAdapter```接口，重写```configure(AuthenticationManagerBuilder auth)```方法。然后在容器中添加```PasswordEncoder```对象

```java
//设置登陆的用户名和密码 第二种方式 配置类
@Configuration
public class HelloConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder(); //密码加密
        String password = passwordEncoder.encode("123456");
        auth.inMemoryAuthentication().withUser("zf").password(password).roles("admin") ;
    }

    // 加密密码需要用到PasswordEncoder接口，所以需要在容器中注入，否则会报错
    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder() ;
    }
}
```
#### 方式三：自定义编写实现类实现数据库认证完成用户登陆


![在这里插入图片描述](https://img-blog.csdnimg.cn/5ac5ece951c34d6e9634029e3444c7a1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)


完成数据库以及表和数据的添加，这里不赘述。

添加依赖：

```xml
        <!--mybatis puls -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.0.5</version>
        </dependency>
        <!--mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
```

##### 制作实体类

```java
@Data
public class Users {
    private Integer id ;
    private String username ;
    private String password ;
}
```

#### 整合MybatisPlus制作mapper

```java
@Repository
public interface UsersMapper extends BaseMapper<Users> {
}
```

#### 配置文件中添加数据库配置

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/demo?serverTimezone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=123456
```

#### 编写登陆实现类
实现```UserDetailsService```接口
```java
@Service("userDetailsService")
public class MyUserService implements UserDetailsService {

    @Autowired
    private UsersMapper usersMapper ;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        QueryWrapper wrapper = new QueryWrapper();
        wrapper.eq("username" , username) ;
        Users users = usersMapper.selectOne(wrapper);

        if( users == null ){
            throw new UsernameNotFoundException("用户名不存在") ;
        }
        //设置用户角色或权限
        List<GrantedAuthority> role = AuthorityUtils.commaSeparatedStringToAuthorityList("admins,ROLE_sale");
        return new User(users.getUsername(), new BCryptPasswordEncoder().encode(users.getPassword()) , role) ;
    }
}

```

#### 实现配置类

```java
//设置登陆的用户名和密码 第三种方式 自定义实现类
@Configuration
public class HelloConfig1 extends WebSecurityConfigurerAdapter {

    @Autowired
    private UserDetailsService userDetailsService ;

    @Autowired
    private DataSource dataSource ;

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder()) ;
    }
   @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder() ;
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/ddbc8d1a8a524b4f9282f41f80076aa7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
### 5 通过配置类授权
在类上添加```@Configuration```注解表示被Spring认为是一个配置类，实现```WebSecurityConfigurerAdapter```接口，重写```configure(HttpSecurity http)```方法。然后在容器中添加```PasswordEncoder```对象

![在这里插入图片描述](https://img-blog.csdnimg.cn/f29f6c0e688747a098b08541d89f0750.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)


- ```hasAuthority```: 如果当前的主体具有指定的权限，则返回 true,否则返回 false
- ```hasAnyAuthority```: 如果当前的主体有任何提供的角色（给定的作为一个逗号分隔的字符串列表）的话，返回true.
- ```hasRole```: 如果用户具备给定角色就允许访问,否则出现 403。
- ```hasAnyRole```: 表示用户具备任何一个条件都可以访问。
	- 底层源码![在这里插入图片描述](https://img-blog.csdnimg.cn/c2417eb8cb24459da2eeddda641902a4.png)
	- 给用户添加角色时需要添加```ROLE_```前缀
	- ![在这里插入图片描述](https://img-blog.csdnimg.cn/f91388f7a3a64a279d5320ef455ff039.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_19,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/1a74cadc693549a1b94d6f0abc21d4cf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)


```java
 @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 没有访问权限， 跳转到页面
        http.exceptionHandling().accessDeniedPage("/unauth.html") ;

        http.formLogin()    //自定义登录页面
                .loginPage("/login.html") //登陆页面设置
                .loginProcessingUrl("/user/login")      //登陆访问路径，内部由security完成
                .defaultSuccessUrl("/success.html").permitAll()   //登陆成功跳转路径，跳转到一个controller方法
            .and()
            .authorizeRequests()
                .antMatchers("/" , "/test/test", "/user/login").permitAll() //设置哪些路劲可以访问，不需要认证
                 //当前登陆用户，只有具有admin权限才可以访问该路径
              //  .antMatchers("/test/index").hasAuthority("admins")
                .antMatchers("test/index").hasAnyAuthority("admins") //需要有admins权限
                .anyRequest().authenticated()   //其他路径都需要认证
   }
```


### 6 自定义没有权限访问页面
![在这里插入图片描述](https://img-blog.csdnimg.cn/3bc1e8e4efab46528a830c858af6f17f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)


### 7 注解实现授权

![在这里插入图片描述](https://img-blog.csdnimg.cn/03c943ff49054cd0af63660f1967ab38.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)

```@Secured```判断是否具有角色，另外需要注意的是这里匹配的字符串需要添加前缀“ROLE_“
使用注解先要开启注解功能！

```java
@SpringBootApplication
@MapperScan("com.zf.security.mapper")
@EnableGlobalMethodSecurity(securedEnabled = true)  //开启security的注解功能
public class SecurityApplication {

    public static void main(String[] args) {
        SpringApplication.run(SecurityApplication.class, args);
    }

}
```

在控制器方法上添加注解

```java
    @GetMapping("update")
    @Secured({"ROLE_sale" })
    public String update(){
        return "hello update" ;
    }
```


### 8 用户注销
![在这里插入图片描述](https://img-blog.csdnimg.cn/3e77c75b8ffd43b29ca3ab8485513abe.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
    @Override
    protected void configure(HttpSecurity http) throws Exception {
            //退出登陆
        http.logout()
                .logoutUrl("/logout")   //退出路径
                .logoutSuccessUrl("/test/test").permitAll(); //退出成功后访问路径
    }
```

### 9 基于数据库的一段时间内免密登陆

![在这里插入图片描述](https://img-blog.csdnimg.cn/6a056b59d007499b9b0b76ce504ae81f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/90087d3aa2574d679584167d4e676273.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
//设置登陆的用户名和密码 第三种方式 自定义实现类
@Configuration
public class HelloConfig1 extends WebSecurityConfigurerAdapter {

    @Autowired
    private UserDetailsService userDetailsService ;

    @Autowired
    private DataSource dataSource ;

    @Bean
    public PersistentTokenRepository persistentTokenRepository(){
        JdbcTokenRepositoryImpl jdbcTokenRepository = new JdbcTokenRepositoryImpl();
        jdbcTokenRepository.setDataSource(dataSource);  //设置数据源
        jdbcTokenRepository.setCreateTableOnStartup(true);  //在启动时创建存储对应Token的数据库表
        return jdbcTokenRepository ;
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder()) ;
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 没有访问权限， 跳转到页面
        http.exceptionHandling().accessDeniedPage("/unauth.html") ;

        http.formLogin()    //自定义登录页面
                .loginPage("/login.html") //登陆页面设置
                .loginProcessingUrl("/user/login")      //登陆访问路径，内部由security完成
                .defaultSuccessUrl("/success.html").permitAll()   //登陆成功跳转路径，跳转到一个controller方法
            .and()
            .authorizeRequests()
                .antMatchers("/" , "/test/test", "/user/login").permitAll() //设置哪些路劲可以访问，不需要认证
                 //当前登陆用户，只有具有admin权限才可以访问该路径
              //  .antMatchers("/test/index").hasAuthority("admins")
                .antMatchers("test/index").hasAnyAuthority("admins") //需要有admins权限
                .anyRequest().authenticated()   //其他路径都需要认证
                //免密登陆
            .and().rememberMe().tokenRepository(persistentTokenRepository())
                  .tokenValiditySeconds(600) //设置免密登陆有效时长，单位秒
                  .userDetailsService(userDetailsService)
            .and().csrf().disable() ; //关闭csrf防护

        //退出登陆
        http.logout()
                .logoutUrl("/logout")   //退出路径
                .logoutSuccessUrl("/test/test").permitAll(); //退出成功后访问路径
    }

    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder() ;
    }
}

```
