---
title: SpringBoot+Vue(CRUD)
date: 2023-07-05 10:47:48
categories: 学习笔记
tags: [Java]
top_img: https://pic.imgdb.cn/item/64a7b65f1ddac507ccbdad50.png
cover: https://pic.imgdb.cn/item/64a7b65f1ddac507ccbdad50.png
description: 
---

## 环境准备和简述

### C/S 架构和 B/S 架构的特点

C/S架构的特点是交互性强，有安全访问模式，网络流量低，响应速度快。因为客户端负责大多数业务逻辑和UI演示，所以被称为**胖客户端**, 缺点是，需要针对不同的操作系统，开发不同版本的软件，但随着互联网的发展，更新和修改的愈发频繁，B/S架构愈发主流。

B/S架构的主要特点是分散性高，维护方便，开发简单，共享性高，拥有总成本低。

### Maven简述

Maven是一个项目管理工具，可以对Java项目进行自动化的构建和依赖管理

![](https://pic.imgdb.cn/item/64a38c071ddac507cc62deca.jpg)

Maven作用可以分为三大类

- 项目构建：提供标准的，跨平台的自动化构建项目的方式
- 依赖管理：方便快捷的管理项目依赖的资源(jar包),避免资源间的版本冲突等问题
- 统一开发结构：提供标准的，统一的项目开发结构

运行Maven时，Maven所需要的任何构建都是从本地仓库直接获取的，如果本地仓库没有，他会首先尝试从远程仓库下载构建至本地仓库。

本地仓库配置：修改maven安装包中的conf/settings.xml文件，指定本地仓库位置，如果不配置的画会放在用户目录下的.m2文件夹中;

```xml
<localRepository>D:\maven-repository(路径)</localRepository>
```

远程仓库配置：也是修改conf/settings.xml文件 ,默认的远程仓库在国外 会很慢，我们采用国内站点镜像

```xml
    <mirror>
      <id>aliyunmaven</id>
      <mirrorOf>*</mirrorOf>
      <name>阿里云公共仓库</name>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
  </mirrors>
```

配置好之后要在idea中配置：进入 Settings | Build, Execution, Deployment | Build Tools | Maven

## SpringBoot快速上手

在创建SpringBoot项目时，配置一下项目的热部署(这里他就帮你自动重启了)，配置文件中添加dev-tools依赖,添加到dependencies标签内。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

在application.properties中配置devtools

```properties
# 热部署生效
spring.devtools.restart.enabled=true
# 设置重启目录
spring.devtools.restart.additional-paths=src/main/java
```

如果用了Eclipse，项目会自动编译并且重启，用idea还需配置自动编译

1. 打开 Settings | Build, Execution, Deployment | Compiler,勾选Build project automatically
2. 按Ctrl+Shift+Alt+/快捷键调出Maintenance页面，单击Registry,勾选compiler.automake.allow.when.app.running复选框

在Application类上面创建一个controller包，新建一个Controller类,写一个简单示例

```java
@RestController
public class HelloController {
//  http://localhost:8080/hello
    @GetMapping("/hello")
    public String hello(){
        return "你好222";
    }
}
```

编写完成后启动 Application ， 输入路径，查看效果。

## Web入门

### 启动器

SpringBoot 将传统web开发的web,webnmvc,json,tomcat等框架整合成了**spring-boot-starter-web**启动器。作用是提供web开发所需的所有底层依赖。

webmvc为Web开发的基础框架，json为JSON数据解析组件，tomcat为自带的容器依赖。

### 控制器

先了解一下mcv设计模式：

- m：model用于封装数据 

- c：controller用于协调和控制，把数据发给视图，接收用户的请求

- v：view用于显示数据  

  用户发送HTTP请求给控制器，控制器向Model请求信息，Model响应信息给控制器，控制器再把数据返回给View，视图再通过HTTP响应给用户
  
  ![](https://pic.imgdb.cn/item/64ad04111ddac507cc6a457a.jpg)

SpringBoot mvc组件提供**@Controller**和**@RestController**两种注解来标识此类负责接受和处理HTTP请求，如果想返回给用户页面和数据，使用@Controller注解即可，他要返回一个页面；如果只是返回数据，则可以使用@RestController注解。 

#### Controller的用法(了解)

示例返回了hello页面和name的数据，在前端页面可以用${name}参数获取后台的数据并显示，通常与Thymeleaf模板引擎结合使用

```java
@Controller
public class HelloController {
    @RequestMapping("/hello")
    public String index(ModelMap map){
        map.addAttribute("name","zhangsan");
        return "hello";//要有一个hello.html的页面
    }
}
```

#### RestController的用法

默认情况下，@RestController注解会将返回的对象数据转换为JSON格式

```java
@RestController
public class HelloController {
//  http://localhost:8080/user
    @GetMapping("/user")
    public User getUser(){
        User user = new User();
        user.setUsername("张三");
        user.setPassword("123");
        return user;
    }
}
```

```json
//输出
{
    "username":"张三",
    "password":"123"
}
```

### 路由映射

URL映射：@RequestMapping注解主要负责URL的路由映射，可以添加在Controller类或者具体的方法上。

@RequestMapping注解包含很多属性参数来定义HTTP的请求映射规则。常用的属性参数如下：

- value：请求URL的路径，支持URL模板，正则表达式，“* ” 匹配任意字符，“ ** ”匹配任意路径，“ ? ”匹配单个字符

- method：HTTP请求方法：GET,POST,PUT,DELETE等方式

  ```java
  @RequestMapping(value="/getData",method= RequestMethod.GET)//规定前端发送get请求
  //上述等价于
  GetMapping("/getData")
  ```

- consumes：请求媒体类型(Content-Type),如application/json

- produces：响应的媒体类型

- params,headers:请求的参数及请求头的值

### 参数传递

@RequestParam将请求参数绑定到控制器的方法参数上，接收的参数来自HTTP请求体或请求体url的QueryString，当请求的参数名称与Controller的业务方法参数一致时，@RequestParam可以省略。

示例：

```java
//获取前端传过来的数据
@RestController
public class HelloController {
/* http://localhost:8080/user?username=张三&phone=123123
   因为请求的参数username和方法中的形参一样 所以省略RequestOaran
   传递多个值用&符号连接
  http://localhost:8080/user*/
    @GetMapping("/user")
    public String getUsername(String username,String phone){
        System.out.println(phone);
        return "你好"+ username;
    }
```

多种传值方式

```java
@RestController
public class ParamsController {
    //最基础的传值
    @RequestMapping(value = "/getTest1",method = RequestMethod.GET)
    public String getTest1(){
        return "GET请求";
    }

    //传递过来多个值,请求的值和形参名字一样
    @RequestMapping(value = "/getTest2",method = RequestMethod.GET)
//  http://localhost:8080/getTest2?username=xxx&phone=xxx
    public String getTest2(String username,String phone){
        System.out.println("username"+username);
        System.out.println("phone"+phone);
        return "GET请求";
    }

    /*传递过来多个值,请求的值和形参名字不一样,加上注解
    注解中的required为false表示路径后面的username传不传递都可以
    如果为true(默认),那地址栏中的路径后面必须加上username=xxx。
    */
    @RequestMapping(value = "/getTest3",method = RequestMethod.GET)
//  http://localhost:8080/getTest2?username=xxx
    public String getTest3(@RequestParam(value="username",required = false)String name){
        System.out.println("username" + name);
        return "GET请求";
    }

    //post基础传值
    @RequestMapping(value = "/postTest1",method = RequestMethod.POST)
//  http://localhost:8080/postTest1
    public String postTest1(){
        return "POST请求";
    }

    @RequestMapping(value = "/postTest2",method = RequestMethod.POST)
    public String postTest2(String username,String password){
        System.out.println("username:"+username);
        System.out.println("password:"+password);
        return "POST请求";
    }
    //类的名称必须和前台传过来的参数名要一致
    @RequestMapping(value = "/postTest3",method = RequestMethod.POST)
    public String postTest3(User user){
        System.out.println(user);
        return "POST请求";
    }
    //如果要接收前端json格式的数据，需要单独加上RequestBody的注解
    @RequestMapping(value = "/postTest4",method = RequestMethod.POST)
    public String postTest4(@RequestBody User user){
        System.out.println(user);
        return "POST请求";
    }
    // test后面可以加任意层级的路径
   @GetMapping("/test/**")
    public String test(){
        return "通配符请求";
   }
}
```



@PathVaraible:用来处理动态的URL，URL的值可以作为控制器中处理方法的参数

@RequestBody接受的参数来自于requestBody中，即请求体，一般用于处理‘application/json’.'application/xml'等类型数据

## Web进阶

### 静态资源的访问

一般项目默认创建classpath:/static/目录并且静态资源一般放在此目录下。想要定义过滤规则和静态资源位置：

```properties
spring.mvc.static-path-pattern=/static/**
spring.web.resources.static-locations=classpath:/static/
```

假如我在static文件夹下放了一张test.jpg的图片 。地址栏输入localhost:8080/static/test.jpg就可以访问到该图片。如果去掉配置文件中的第一句 直接在8080后面写test.jpg就可以访问到

### 文件上传

  如果通过表单来上传文件，要求改表单的enctype类型为multipart/form-data。如果输入 是普通的用户名字段，直接正常传输用户名字段，如果传输的是文件，就包括form中的name值，文件名(filename)，文件类型(Content-type)，文件内容，如果是图片视频的内容，就会以二进制的格式存储

```html
enctype="application/x-www-form-urlencoded"(默认)
enctype="multipart/form-data"(修改)
```

当表单的enctype="multipart/form-data"时，可以使用MultipartFile(spring用来接收文件的类型)获取上传的文件数据，再通过transferTo方法将其写入到磁盘中，**就是把用户上传上来的文件传给Web服务器的本地**

springBoot工程嵌入的tomcat限制了请求的文件大小，每个文件的配置最大为1mb，单次文件的总数不能大于10mb

可以通过更改applicaton.properties中两个默认值

```properties
#单个文件大小
spring.servlet.multipart.max-file-size=10MB
#每次请求所有文件大小
spring.servlet.multipart.max-request-size=10MB
```

文件上传小demo

先创建一个FileUploadController类

```java
package com.example.demo1.controller;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;
import javax.servlet.http.HttpServletRequest;
import java.io.File;
import java.io.IOException;

@RestController
public class FileUploadController {

    @PostMapping("/upload")
    public String up(String nickname, MultipartFile photo, HttpServletRequest request) throws IOException{
        System.out.println(nickname);
        //获取图片原始名字
        System.out.println(photo.getOriginalFilename());
        //获取文件类型
        System.out.println(photo.getContentType());
        //通过获取web服务器所对应的路径，request.getServletContext()动态获取路径
        String path = request.getServletContext().getRealPath("/upload/");
        saveFile(photo,path);
         //获取路径
        System.out.println(path);
        return "上传成功";
    }
    public void saveFile(MultipartFile photo,String path) throws IOException{
        //判断存储的目录是否存在，如果不存在则创建
        File dir = new File(path);
        if(!dir.exists()){
            //创建目录
            dir.mkdir();
        }
        //拼接出来的完整路径
        File file = new File(path + photo.getOriginalFilename());
        //通过transferTo方法接送网络上传过来的文件，放入file路径中
        photo.transferTo(file);
    }
}
//用aippost工具发送参数名为nickname，值为张三，参数名为photo，值为icon.jpg(随意图片文件)的post的请求。
/*输出结果
张三
icon.jpg
image/jpeg
C:\Users\86187\AppData\Local\Temp\tomcat-docbase.8080.1110413597493093875\upload\*/
```

我们把图片放入upload目录下那我们如何在浏览器里访问到该图片呢，只需增加一条配置信息

```properties
#配置静态目录
spring.web.resources.static-locations=/upload/
```

输入localhost:8080/icon.jpg就能访问到该图片(如果前面有配置spring.mvc.static-path-pattern=/static/**，要在图片前面加上static目录)，如果图片访问失败，再发送一次post请求后刷新页面，因为路径是动态变化的。

### 拦截器

在Web系统中十分常见，对于某些全局统一的操作，我们可以把它提取到拦截器中实现，大致有以下几种场景

- 权限检查：如登陆检测，进入处理程序检测是否登录，如果没有，则直接返回登陆页面。
- 性能监控：有时系统在某段时间莫名其妙很慢，可以通过拦截器在进入处理程序之前记录开始时间，在处理完后记录结束时间，从而得到该请求的处理时间
- 通用行为，读取cookie得到用户信息并将用户对象放入请求，从而方便后续流程使用，还有读取Locale，Theme信息等，只要是多个处理程序都需要的，即可使用拦截器实现。

SpringBoot定义了HandlerInterceptor接口来实现自定义拦截器的功能，HandlerInterceptor接口定义了preHandle，postHandle,afterCompletion三种方法，通过重写这三种方法实现请求前，请求后等操作。

前端请求先进入拦截器中的preHandle方法，最终会调用到目标方法。控制器做出响应后，调用postHandle方法返回给前端页面，等页面完成渲染了，再去调用afterCompletion方法

拦截器定义：

```java
public class LoginInterceptor extends HandlerInterceptor{
    /*
    * 在请求处理之前进行调用(Controller方法调用之前)
    * */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        if(条件){
            System.out.println("通过");
            return true;
        }else {
            System.out.println("不通过");
            return false;
        }
    }
}
```

拦截器注册（否则拦截器无法使用）

- addPathPatterns方法定义拦截的地址，添加的一个拦截器没有addPathPatterns任何一个url则默认拦截所有请求

- excludePathPatterns定义排除某些地址不被拦截，如果没有excludePathPatterns任何一个请求，则默认不放过任何一个请求

```java
//要加上注解，SpringBoot会自动读取此类
@Configuration
public class WebConfigurer implements WebMvcConfigurer{
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //创造注册刚刚新建的拦截器，设置拦截的路径
        registry.addInterceptor(new LoginInterceptor()).addPathPatterns("/user/**")
    }
}
```

## RESTful服务+Swagger

### RESTful服务

RESTful是一种互联网软件服务架构设计风格

符合RESTful规范的Web API需要具备如下两个关键特性：

- 安全性：安全的方法被期望不会产生任何副作用，当我们使用GET操作获取资源时，不会引起资源本身的改变，也不会引起服务器资源的改变
- 幂等性：幂等的方法保证了重复进行一个请求和一次请求的效果相同(并不是指响应总是相同，而是指服务器上的资源状态从第一次请求后就不再改变了，请求发1次和100次的服务器上的资源状态是一样的)，在数学上幂等性是指N次变化和一次变化相同

举个栗子：传统删除用户的请求和RESTful风格的请求

```java
//传统
get http://localhost/del?id=10 //尽量不要出现动词
//RESTful风格
delete http://localhost/user/10
```



RESTful设计风格 实现RESTful AP

```java
@RestController
public class UserController {
    @GetMapping("/user/{id}")
    /*PathVariable是映射URL绑定的占位符
    * 可以将URL中占位符参数（“{id}”）绑定到控制器处理方法的入参中(int id)
    * 如果没有这个注解直接int id，拿到的id就会变成null。
    * */
    public String getUserById(@PathVariable int id){
        System.out.println(id);
        return "根据ID去获取用户";
    }
    @PostMapping("/user")
    public String save(User user){
        return "添加用户";
    }
    @PutMapping ("/user")
    public String update(User user){
        return "更新用户";
    }
    @DeleteMapping("/user/{id}")
    public String deleteById(@PathVariable int id){
        return "根据ID去删除用户";
    }
}

```

### Swagger

Swagger是一个规范和完整的框架，用于生成，描述，调用和可视化RESTful风格的Web服务，是非常流行的API表达工具，Swagger可以自动生成完善的RESTful API文档，同时根据后台代码的修改同步更新，同时提供完整的测试页面来调试API

在项目中集成Swagger，需要在项目配置文件中引入

```xml
<!--        添加swagger2相关功能-->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
<!--        添加swagger-ui相关功能-->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
```

再config包下添加SwaggerConfig类

```java
@Configuration //告诉Spring容器，这个类是一个配置类
@EnableSwagger2 //告诉Swagger2功能
public class SwaggerConfig {
    @Bean
    public Docket createRestApi(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com")) //com包下所有API都交给Swagger2管理
                .paths(PathSelectors.any()).build();
    }
    //API文档显示信息
    private ApiInfo apiInfo(){
        return new ApiInfoBuilder()
                .title("演示项目API") //标题
                .description("学习Swagger2的演示项目") //描述
                .version("1.0")
                .build();
    }
}
```

Spring Boot 2.6.X后与Swagger有版本冲突问题，所以要加入以下配置再application.properties文件下添加

```properties
spring.mvc.pathmatch.matching-strategy= ant_path_matcher
```

访问http://localhost:8080/swagger-ui.html 进入可视化测试页面

Swagger提供一系列注解来描述接口信息，包括接口说明，请求方法，请求参数，返回信息。

![](https://pic.imgdb.cn/item/64a7c3761ddac507cceab244.jpg)

比如我在User控制器里的获取用户方法前面加入@ApiOperation("获取用户"),在可视化测试页面，也会有相应的提示信息哦！

## MybatisPlus快速上手

### MyBatisPlus介绍

先了解ORM：对象关系映射，为了解决面向对象与关系型数据库存在的互不匹配现象的一种技术，比如说想要把数据库表中的字段值变成一个对象，或者说一个对象变为数据库表中分别对应的字段值，其中的过程就是ORM来完成的，ORM框架的本质是简化编程中操作数据库的编码

MyBatis就是一款数据持久层ORM框架。能够灵活的实现动态SQL，使用XML或注解来配置和映射原生信息，能够轻松访问将Java的普通对象与数据库中的表和字段进行映射关联

MyBatis-Plus是一个MyBatis的增强工具，在MyBatis的基础上做出了增强，简化了开发

### MyBatisPlus配置

application.properties

```properties
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost3306/mydb?userSSL=false
spring.datasource.username=root
spring.datasource.password=123456
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

porm.xml

```xml
<!--        MyBatisPlus依赖-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.2</version>
        </dependency>
<!--        mysql驱动依赖-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
<!--        数据库连接池druid-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.20</version>
        </dependency>
```

在启动类里面添加@MapperScan("com.example.mpdemo.mapper")注解(括号里面为mapper包路径)

准备工作，先创建一个数据库表

![](https://pic.imgdb.cn/item/64a802841ddac507ccb7ebde.jpg)

在entity包(没有的话就新建)下新建一个User实体类，加入四个私有变量，创建构造器和getset方法。

在controller(没有的话就新建)下创建一个UserController类

Mybatis中的操作

```java
@RestController
public class UserController {
//    @Autowired 可以使用此注解，但是userMapper会爆红，因为Autowired是spring提供的
//    而@Mapper是Mybatis提供的，识别不来，
//    解决方法 换成@Resource,或者在接口中写@Repository。
//    @Resource //UserMapper实例的对象自动注入userMapper,
    @Autowired
    private UserMapper userMapper;
    @GetMapping("/user")
    public List query(){
        List<User> userlist = userMapper.QueryUser();
        System.out.println(userlist);
        //如果要返回json格式，只需把返回值改为List即可，他会自动转成Json格式
        return userlist;
    }
    @PostMapping("/user")
    public String add(User user){
        int num = userMapper.addUser(user);
        if(num >0){
            return "插入成功";
        }else{
            return "插入失败";
        }
    }
    @PostMapping("/update")
    public String update(User user){
        int num = userMapper.updateUser(user);
        if(num >0){
            return "更新成功";
        }else{
            return "更新失败";
        }
    }
    @PostMapping("/del")
    public String delete(int id){
        int num = userMapper.delUser(id);
        if(num >0){
            return "删除成功";
        }else{
            return "删除失败";
        }
    }
    @PostMapping("/byid")
    public String queryByid(int id){
        User user = userMapper.findById(id);
        System.out.println(user);
      return "查询成功";
    }
}
```

在mapper包(没有就新建)下创建一个UserMapper接口,先写一个查询

Spring项目会自动实例化这个UserMapper，管理UserMapper的实例，内部通过动态代理方式生成一个实现类并且继承了此接口

```java
//CRUD操作
@Repository
public interface UserMapper {
//查询所有用户
    @Select("select * from user")
    List<User> QueryUser();
//新增用户
    @Insert("insert into user(username,password,birthday) values(#{username},#{password},#{birthday})")
    int addUser(User user);
//更新用户
    @Update("Update user set username=#{username},password =#{password},birthday=#{birthday} where id=#{id}")
    int updateUser(User user);
//删除用户
    @Delete("delete from user where id=#{id}")
    int delUser(int id);
//查询指定用户
    @Select("select * from user where id =#{id}")
    User findById(int id);
}
```

在Mybatis-Plus里面可以更加简化

修改UserMapper

```java
/*只需继承BaseMapper，传入User类，他会自动根据User类去找数据库中的user表，帮你去做操作
前提类的名字要和表的名字保持一致 如果过表名与类不一致，可以使用@TableName("")注解去帮助他找到表，在User实体类前面添加TableName注解
*/
@Repository
public interface UserMapper extends BaseMapper<User> {}
```

修改UserController,使用BaseMapper的方法，修改usermapper调用的方法名

```java
..(略) 
List<User> userlist = userMapper.selectList(null);//因为是全部查询，所以条件为null
..
int num = userMapper.insert(user);
..
int num = userMapper.updateById(user);
..
int num = userMapper.deleteById(id);
..
User user = userMapper.selectById(id);
```

也可以通过添加@TableId注解，告诉mbp，id是自增的，这样 我们在新增用户的时候输出user，原本的信息里面是id为0，但是输出的user对象会自动输出该记录的id。以便后续更好的操作

```java
//User实体类
@TableId(type = IdType.AUTO)
    private int id;
//UserController类
 @PostMapping("/user")
    public String add(User user){
        int num = userMapper.insert(user);
//加了注解的效果：User{id=7, username='infnitas', password='123456', birthday='2001-01-24'}
//不加的：User{id=0, username='infnitas', password='123456', birthday='2001-01-24'}
        System.out.println(user); //y
        if(num >0){
            return "插入成功";
        }else{
            return "插入失败";
        }
    }
```

### 多表查询以及分页查询

实现复杂关系映射，可以使用@Result注解，@Results注解,@One注解，@Many注解组合完成复杂关系的配置

| 注解     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| @Results | 代替<resultMap>标签，该注解中嗯可以加入单个或多个@Result注解 |
| @Result  | 代替<id>标签和<Result>标签，@Result中可以使用以下属性<br />- column:数据表的字段名称 <br>- property:类中对应的属性名<br>- one:与@One注解配合，进行一对一的映像<br>- many:与@Many注解配合，进行一对多的映像 |
| @One     | 代替<assocation>标签，用于指定查询中返回的单一对象<br>通过select属性指定用于多表查询的方法<br>使用格式：@Result(column="",property="",one=@One(select="")) |
| @Many    | 代替<collection>标签，用于指定查询中返回的集合对象<br>使用格式：@Result(column="",property="",Many=@Many(select="")) |

示例：需求在查询用户的同时，也查询用户对应的订单信息，需要自己完成映射关系，不能去依靠myp   

在User实体类中添加orders属性

```java
//  描述用户所有的订单
    @TableField(exist = false)
    /*
    加exist的原因，因为mybatis-plus里面select方法是继承baseMapper自动执行的
    不加的话他会把orders也当作表中存在的字段，这样会报错。
    如果你要调用baseMapper中的，一定要写此注解
    * */
    private List<Order> orders;
//  记得生成相对应的get，set方法
```



修改UserMapper，添加查询用户以及其所有订单的注解

```java
//    查询用户以及其所有的订单
    @Select("select * from t_user")
    @Results(//如果使用结果集映射，所有字段都要手动映射
            {
                    @Result(column = "id",property = "id"),
                    /*column代表表里面的id，property代表类里面的id
                    * 查询出来的id要赋值给user类里面的id属性
                    * */
                    @Result(column = "username",property = "username"),
                    @Result(column = "password",property = "password"),
                    @Result(column = "birthday",property = "birthday"),
                    @Result(column = "id",property = "orders",javaType = List.class,
                            many = @Many(select = "com.example.mpdemo.mapper.OrderMapper.selectByUid")
                    ),
                    /*把user表中的id到ordermapper，然后调用orderMapper中的selectByUid方法把id传到方法中
                    再调用sql语句查询出对应id的对应订单信息 返回给orders;因为一个用户对应多个订单，所以用				       @Many注解
                    */
            }
    )
    List<User> selectAllUserAndOrders();
}
```

创建OrderMapper

```java
@Repository
public interface OrderMapper extends BaseMapper<Order> {
    //通过id去获取订单
    @Select("select * from t_order where uid =#{uid}")
    List<Order> selectByUid(int uid);
}
//usermapper中id传过来之后调用此方法，然后返回给orders；
```

修改UserController 

```java
//多表查询
    @GetMapping("/user/order")
    public List<User> selectAllUserAndOrder(){
        return userMapper.selectAllUserAndOrders();
    }
```

输入localhost:8080/user/order 查看输出效果

需求2：查询订单的同时，查询出所属用户

在Orders实体类中添加user属性

```java
    @TableField(exist = false)
    private User user;
```

在orderMapper中添加注解

```java
//    查询所有订单，同时查询订单的用户
    @Select("select * from t_order")
    @Results({
            @Result(column = "id",property = "id"),
            @Result(column = "ordertime",property = "ordertime"),
            @Result(column = "total",property = "total"),
            @Result(column = "uid",property = "uid"),
            @Result(column = "uid",property = "user",javaType = User.class,
                    one=@One(select ="com.example.mpdemo.mapper.UserMapper.findById")
                    //与usermapper同理
            )
    })
    List<Order> selectAllOrderAndUser();
```

在usermapper中添加注解

```java
//查询指定用户
    @Select("select * from t_user where id =#{id}")
    User findById(int id);
```

新建ordercontroller类

```java
@RestController
public class OrderController {

    @Autowired
    private OrderMapper orderMapper;

    @GetMapping("/order/findAll")
    public List selectAllorderAndUser(){
        //调用ordermapper的方法
        List<Order> orders = orderMapper.selectAllOrderAndUser();
        return orders;
    }
}
```

输入localhost:8080/order/findAll查看效果

需求3，条件查询

修改usercontroller

```java
 //条件查询
    @GetMapping("/user/find")
    public List<User> findByCond(){
        QueryWrapper<User> queryWrapper = new QueryWrapper();//QueryWrapper表示约束条件
        queryWrapper.eq("username","milet");//eq表示XX等于XX
        return userMapper.selectList(queryWrapper);
        //然后调用basemapper方法，把约束条件传入mbp的selectlist方法条件中去
    }
```

需求4：分页查询

需要先做一个配置,新建MyBatisPlusConfig类,这样就能调用selectPage方法了

```java
@Configuration
public class MyBatisPlusConfig {
    @Bean
    public MybatisPlusInterceptor paginationInterceptor(){
        //相当于做了一个mybatisplus的拦截器
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
        PaginationInnerInterceptor paginationInnerInterceptor = new PaginationInnerInterceptor(DbType.MYSQL);//告诉mbp数据库类型
        mybatisPlusInterceptor.addInnerInterceptor(paginationInnerInterceptor);
        return mybatisPlusInterceptor;
    }
}
```

修改UserController

```java
   //分页查询 select * from xx limit 0,10
    @GetMapping("/user/findByPage")
    public IPage findByPage(){
        //设置枚举类型,查询哪一张表，current表示起始点，size表示步长
        Page<User> page = new Page<>(0, 2);
//        分页查询也可以插入约束条件，QueryWrapper
        IPage iPage = userMapper.selectPage(page,null);
        //iPage用来描述结果集
        return iPage;
    }
```

输入localhost:8080/user/findByPage查看效果

## Vue快速上手

### Vue介绍

MVVM模式介绍

- M:model
- V:view
- VM:viewModel用于完成数据的监听和数据的绑定，可以根据model的变化而变化页面(单向)，也可以根据view的变化去变化model(双向)

![](https://pic.imgdb.cn/item/64ad080d1ddac507cc7b4113.jpg)

 这样就无需手动绑定，只需要关系数据和业务逻辑就可以

### Vue基本上手

安装vue

- 脚本方式

```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
```

- npm命令方式

```shell
npm init vue@latest
```

基本用法

- 导入vus.js的script脚本文件

```html
<script src="https://unpkg.com/vue@3"></script>
```

- 在页面中声明一个将要被Vue控制的DOM区域，既MVVM中的View

```html
 <div id="app">
        {{ message}}
    </div>
```

- 创建Vue实例对象(Vue实例对象)

```javascript
 Vue.createApp({
     //指定数据源，既MVVM中的Model
        data(){
            return{
                message:"Hello Vue!"
            }
        }
       }).mount('#app')//指定当前vue要实例要控制页面的哪个区域
```

#### 内容渲染指令

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://unpkg.com/vue@3"></script>
    <title>Document</title>
</head>
<body>
    <div id="app">
        <ul>
            <!-- 可以带一个或多个参数，一个参数的话取出来是一个元素，两个参数前面是元素后面是索引 -->
            <li v-for="(user,i) in userList">索引是:{{i}}，姓名是:{{user.name}}</li>
        </ul>
    </div>
    <script>
        const vm={
            data:function(){
                return{
                    userList:[
                        {id:1,name:'zhangsan'},
                        {id:2,name:'lisi'},
                        {id:3,name:'wangwu'},
                    ]
                }
            }
        }
        const app = Vue.createApp(vm)
        app.mount('#app')
    </script>
</body>
</html>
```

#### 属性绑定指令

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://unpkg.com/vue@3"></script>
    <title>Document</title>
</head>
<body>
    <div id="app">
        <a :href="link">百度</a>
        <!-- 前面加上":"使其变成一个表达式，进行属性的绑定 -->
        <input type="text" :placeholder="inputValue">
        <img :src="imgSrc" :style="{width:w}" alt="">
    </div>
    <script>
        const vm={
            data:function(){
                return{
                    link:"http://www.baidu.com",
                    //文本框的占位符内容
                    inputValue:'请输入内容',
                    //图片src地址
                    imgSrc:'./images/miletmfm-29.jpg',
                    w:'500px'
                }
            }
        }
        const app = Vue.createApp(vm)
        app.mount('#app')
    </script>
</body>
</html>
```

#### 使用js表达式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width= , initial-scale=1.0">
    <script src="https://unpkg.com/vue@3"></script>
    <title>Document</title>
</head>
<body>
    <!-- vue要控制的DOM区域 -->
    <div id="app">
        <p>{{number+1}}</p>
        <p>{{ok ? 'True' : 'False'}}</p>
        <!-- 将字符串分割成数组然后倒序输出 -->
        <p>{{message.split('').reverse().join('')}}</p>
        <p :id="'list-' + id">xxxx</p>
        <p>{{user.name}}</p>
    </div>
    <script>
        const vm ={
            data :function() {
                return {
                    number:9,
                    ok:false,
                    message:'ABC',
                    id:3,
                    user:{
                        name:'zs',
                    }
                }
            }
        }
        const app = Vue.createApp(vm)
        app.mount('#app')
    </script>
</body>
</html>
```

#### 事件绑定指令

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://unpkg.com/vue@3"></script>
    <title>Document</title>
</head>
<body>
     <div id="app">
        <h3>count的值为:{{count}}</h3>
        <!-- v-on做事件监听  也可以简写成@，如果逻辑很简单，也可以直接写表达式-->
        <button v-on:click="addCount">+1</button>
        <button @click="count-=1">-1</button>
     </div>
     <script>
        const vm={
            data:function(){
                return {
                    count:0,
                }
            },
            //自定义的方法要放在methods中
            methods: {
                //点击按钮让count自增+1
                addCount(){
                    //要通过this去访问count，this代表的是vm
                    this.count +=1
                },
            },
        }
        const app = Vue.createApp(vm)
        app.mount('#app')
     </script>
</body>
</html>
```

#### 条件渲染指令

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://unpkg.com/vue@3"></script>
    <title>Document</title>
</head>
<body>
    <div id="app">
        <button @click="flag =!flag">Toggle Flag</button>
        <!-- 如果v-if或者v-show的值为false，则标签不会被显示 -->
        <!-- v-if和v-show的区别 ，如果标签会被频繁的切换，v-show性能会更高
        v-if如果为false，标签不会被创建
        v-show如果为false，标签是被display:none的css属性给隐藏了 -->
        <p v-if="flag">请求成功 ------ 被 v-if控制</p>
        <p v-show="flag">请求成功 ------ 被 v-show控制</p>
    </div>
    <script>
        const vm = {
            data: function(){
                return{
                    flag:false
                }
            }
        }
        const app = Vue.createApp(vm)
        app.mount('#app')
    </script>
</body>
</html>
```

#### v-else和v-else-if指令

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://unpkg.com/vue@3"></script>
    <title>Document</title>
</head>
<body>
    <div id="app">
        <p v-if="num >0.5">随机数 > 0.5</p>
        <p v-else>随机数 ≤ 0.5</p>
        <hr />
        <p v-if="type === 'A'">优秀</p>
        <p v-else-if="type === 'B'">良好</p>
        <p v-else-if="type === 'C'">一般</p>
        <p v-else>差</p>

        <div v-show="a">
            测试
        </div>
        <button @click="a=!a">点击</button>
    </div>
    <script>
        const vm = {
            data: function(){
                return{
                    //生成1以内的随机数
                    num:Math.random(),
                    type:'A',
                    a:false,
                }
            }
        }
        const app = Vue.createApp(vm)
        app.mount('#app')
    </script>
</body>
</html>
```

#### 列表渲染指令

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://unpkg.com/vue@3"></script>
    <title>Document</title>
</head>
<body>
    <div id="app">
        <ul>
            <!-- 可以带一个或多个参数，一个参数的话取出来是一个元素，两个参数前面是元素后面是索引 这边其实要加key，但是先忽略了 -->
            <li v-for="(user,i) in userList">索引是:{{i}}，姓名是:{{user.name}}</li>
        </ul>
    </div>
    <script>
        const vm={
            data:function(){
                return{
                    userList:[
                        {id:1,name:'zhangsan'},
                        {id:2,name:'lisi'},
                        {id:3,name:'wangwu'},
                    ]
                }
            }
        }
        const app = Vue.createApp(vm)
        app.mount('#app')
    </script>
</body>
</html>
```

#### v-for中的key

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://unpkg.com/vue@3"></script>
    <title>Document</title>
</head>
<body>
    <div id="app">
        <!-- 添加用户的区域 -->
        <div>
            <!-- v-model双向绑定，当页面的值发生变化，属性值也会改变，反之也成立
            v-model用户输入的值会自动影响name值
            如果是value，name发生变化的时候 页面的值会改变，单反之则不行 -->
            <input type="text" v-model="name">
            <button @click="addNewUser">添加</button>
        </div>

        <!-- 用户列表区域 -->
        <ul>
            <!-- 加入一个key属性
            如果不加入key标签，当我们勾选zs用户后，添加一个新用户，
            页面会变成勾选新用户，而不是勾选zs用户
            虽然在我们勾选zs用户之后，zs用户的标签状态已经发生了改变，
            但是vue会重用标签，他会让新标签变成勾选状态(之前的状态)，
            还有一点key值设置索引也会发生问题，原本勾选的zs索引为0
            当我们新增用户时，新用户的索引会变成0，还是会变成勾选状态
            所以说要把key值设置数据库组件的id，这个id是我们自己设置的，
            不会像index一样自动变化
        -->
            <li v-for="(user,index) in userList" :key="user.id">
                <input type="checkbox" />
                索引: {{index}}
                姓名：{{user.name}}
            </li>
        </ul>
    </div>
    <script>
        const vm ={
            data:function(){
                return{
                    userList:[
                        {id:1,name:'zs'},
                        {id:2,name:'ls'},
                        {id:3,name:'js'}
                    ],
                    //输入的用户名
                    name: '',
                    //下一个可用的id值
                    nextId:4
                }
            },
            methods: {
                //点击了添加按钮
                addNewUser(){
                    this.userList.unshift({id:this.nextId,name:this.name})
                    this.name=''
                    this.nextId++
                }
            }
        }
        const app = Vue.createApp(vm)
        app.mount('#app')
    </script>
</body>
</html>
```

## Vue组件化开发

Vue CLI是Vue官方提供的构建工具，用于快速搭建一个带有热重载及构建生产版本等功能的单页面应用。Vue CLI基于webpack构建，也可以通过项目内的配置文件进行配置，

安装：npm install -g @vue/cli

创建文件夹，在文件夹路径上输入cmd打开命令窗口，输入vue create hello 选择第三个

```
Vue CLI v5.0.8
? Please pick a preset:
  Default ([Vue 3] babel, eslint)
  Default ([Vue 2] babel, eslint)
> Manually select features
```

取消linter选项,按空格切换

```
Vue CLI v5.0.8
? Please pick a preset: Manually select features
? Check the features needed for your project: (Press <space> to select, <a> to toggle all, <i> to invert selection, and
<enter> to proceed)
 (*) Babel
 ( ) TypeScript
 ( ) Progressive Web App (PWA) Support
 ( ) Router
 ( ) Vuex
 ( ) CSS Pre-processors
>( ) Linter / Formatter
 ( ) Unit Testing
 ( ) E2E Testing
```

选择配置存放的文件，创建快照依自己所需

```
? Where do you prefer placing config for Babel, ESLint, etc.?
  In dedicated config files
> In package.json //类似于springboot中的porm.xml
```

就会联网下载配置文件和项目

组件构成

- Vue中规定组件的后缀名是.vue
- 每个.vue组件都由3部分构成
  - template:组件的模板结构，可以包含html标签及其他的组件
  - script：组件的javascript代码
  - style：组件的样式

示例

```vue
<!-- 模板结构 -->
<template> 
  <img alt="Vue logo" src="./assets/logo.png">
</template>
<!-- js脚本 -->
<script>
import HelloWorld from './components/hello.vue'//模块化开发思想
export default {
  name: 'App',
  components: {
  }
}
</script>
<!-- 组件样式 -->
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

vue支持自定义标签，在components中添加hello.vue,创立基本的结构

```vue
<template>
    <h1>Hello</h1>
</template>
<script>
</script>
```

然后再app.vue的script标签内中导入hello.vue组件,再componens组件中进行局部注册

```vue 
<script>
import Hello from './components/hello.vue'
export default {
  name: 'App',
  components: {//局部注册
    Hello
  }
}
</script>
```

就可以再template标签内使用

```vue
<template> 
    <div>
      <img alt="Vue logo" src="./assets/logo.png">
      <Hello></Hello>
      <Hello></Hello>
</div>
</template>
```

main.js中 

```js
import { createApp } from 'vue'
import App from './App.vue'
//app.vue导入然后mount到id为app的标签中，这个标签在index.html中
createApp(App).mount('#app')
```

组件间可以由内部的Data提供数据，也可以由父组件通过prop的方式传值，兄弟组件之间可以通过Vuex等统一数据源提供数据共享

我们这里使用vue2 ，注意 vue2的所有组件要包含在一个父标签下，所以写之前外面 套个div标签

新建一个vue项目。创建movie.vue 我们直接在app.vue提供数据

```vue
<template>
    <div>
        <h1>{{ title }}</h1>
        <span>{{ rating }}</span>
        <button @click="fun">点击收藏</button>
    </div>
</template>
<script>
export default {
    name:"Movie",//这个名称可以再app.vue中用Movie.name调用
    //现在希望实现使用者提供数据,用props，app.vue就可以直接调用这个title
    props:["title","rating"],
    data:function(){
        return{ 
        }
    },
    methods: {
        fun(){
            alert("收藏成功!")
        }
    }
}
</script>
```

修改app.vue ，使用循环将标题和日期输出

```vue
<template>
  <div id="app">
   <Movie v-for="movie in movies" :key="movie.id" :title="movie.title" :rating="movie.rating"></Movie>
  </div>
</template>

<script>
import Movie from './components/Movie.vue'

export default {
  name: 'App',
  data:function(){
    return {
      movies:[
        {id:1,title:"金刚狼1",rating:8.7},
        {id:2,title:"金刚狼2",rating:8.5},
        {id:3,title:"金刚狼3",rating:8.1},
      ]
    }
  },
  components: {
    Movie
  }
}
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```

## 第三方组件element-ui

#### 安装及简单使用

如果在网络上下的项目没有node_modules，可以用此命令去自动下载

```
npm install
```

安装 element-ui

```
npm install element-ui
```

在main.js下导入，然后进行全局注册

```js
import Vue from 'vue'
import App from './App.vue'
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

Vue.config.productionTip = false
Vue.use(ElementUI)//全局注册
new Vue({
  render: h => h(App),
}).$mount('#app')//挂载

```

示例1：建立一个表格

```vue
<template>
    <!-- 表格 -->
    <el-table :data="tableData" style="width: 100%;" :row-class-name="tablerowcolor">
        <el-table-column
            prop="date"
            label="日期"
            width="180"
        >
        </el-table-column>
        <el-table-column
            prop="name"
            label="姓名"
            width="180"
        >
        </el-table-column>
        <el-table-column
            prop="address"
            label="地址"
        >
        </el-table-column>
    </el-table>
</template>
<script>
     export default {
    methods: {
        tablerowcolor({row,rowIndex}){
            if(rowIndex===1){
                return 'warning-row';
            }else if (rowIndex ===3){
                return 'success-row';
            }
            return '';
        }
    },
    data() {
      return {
        tableData: [{
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
        }, {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }]
      }
    }
  }
</script>
<style>
 .el-table .warning-row {
    background: oldlace;
  }
  .el-table .success-row {
    background: #f0f9eb;
  }
</style>
```

示例2 日期选择器

```vue
<template>
  <div>
  <!-- 日期选择器-默认 -->
    <div class="block">
        <span class="demontration">默认</span>
        <!-- v-model绑定值 -->
        <el-date-picker
            v-model="value1"
            type ="date"
            placeholder="选择日期">
        </el-date-picker>
    </div>
 <!-- 日期选择器-快捷选项 -->
    <div class="block">
        <span class="demontration">带快捷选项</span>
        <el-date-picker
            v-model="value2"
            align="right"
            type ="date"
            placeholder="选择日期"
            :picker-options="pickerOptions">
            <!-- picker-options当前时间日期选择器特有的选项 -->
        </el-date-picker>
    </div>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        // picker-options当前时间日期选择器特有的选项
        pickerOptions:{
            //disableDate设置禁用状态，参数为当前日期
            disabledDate(time){
                return time.getTime() > Date.now();
            },
            //shortcuts 设置快捷选项，需要传入{text，onclick}对象
            shortcuts:[{
                //text：标题文本
                text:'今天',
                //选中后的回调函数，参数是 vm，可通过触发 'pick' 事件设置选择器的值。例如 vm.$emit('pick', new Date())
                onClick(picker){
                    const date = new Date();
                    prick.$emit('pick',date);
                }
            },{
                text:'昨天',
                onClick(picker){
                    const date = new Date();
                    //减去一天的毫秒数
                    date.setTime(date.getTime() - 3600* 1000 *24);
                    picker.$emit('pick',date);
                }
            },{
                text:'一周前',
                onClick(picker){
                    const date = new Date();
                    //减去一周的毫秒数
                    date.setTime(date.getTime() - 3600* 1000 *24) * 7;
                    picker.$emit('pick',date);
                }
            }]
        },
        value1:'',
        value2:'',
      }
    }
  }
</script>
<style>
</style>
```

#### 第三方图标库

- 文档地址 http://fontawesome.dashgame.com/

- 安装 

  ``` 
  npm install font-awesome
  ```

- 使用,导入进main.js中

  ``` 
  import 'font-awesome/css/font-awesome.min.css'
  ```

  ```vue
  <!--所有的样式前面都要加上 fa 后面也可以加入fa-xx来自定义大小-->
  <i class="fa fa-camera-retro fa-lg"></i> fa-camera-retro 
  ```

