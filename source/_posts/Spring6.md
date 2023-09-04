# Spring6

## 概述

Spring Framework特点：

1. 非侵入式：对结构影响小，功能组件只需注解，不破坏原有结构

2. 控制反转-IOC ：把自己创建的资源，向环境索取资源变成环境将资源准备好，只管资源注入

3. 面向切面编程：AOP-再不修改源代码的基础上增强代码功能

4. 容器：包含管理组件对象的生命周期，组件得到了容器化管理

5. 组件化：实现使用简单的组件配置组合成一个复杂的应用

6. 一站式：在IOC和AOP的基础上整合其他开源框架和第三方库


基础配置：

在porn.xml中添加配置文件和测试工具

```xml
<!--        核心依赖-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>6.0.2</version>
        </dependency>
<!--        junits5测试-->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <version>5.8.2</version>
        </dependency>
```

创建spring-first模块，创建User对象

```java
public class User {
    public void add(){
        System.out.println("add.....");
    }
}

```

在resources中创建Spring.Config , bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--完成对user对象创建
    bean标签
    id：唯一标识(自定义)
    class属性：要创建对象所在类的全路径-->
    <bean id="user" class="com.jason.spring6.User"></bean>
</beans>
```

创建UserTest测试类

```java
public class UserTest {
    @Test
    public void testUserObject(){
        //通过读取类路径下的xml配置文件来创建ioc容器对象
        ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
        //获取创建的对象
        User user = (User)context.getBean("user");
        //调用方法测试
        user.add();
    }
}
```

补充：无参数构造方法也会被执行， 也可以用反射来创建对象,

```java
/**
* 反射来创建对象
*/
Class clazz = Class.forName("com.jason.spring6.User");
//调用方法创建对象
// Object o = clazz.newInstance();(老)
User user= (User)clazz.getDeclareConstructor().newInstance();
```

### Log4j2日志

引入Log4j2的依赖

```xml
 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-log4j2</artifactId>
        </dependency>
```

log4j2.xml配置模板

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <loggers>
        <!--
            level指定日志级别，从低到高的优先级：
                TRACE < DEBUG < INFO < WARN < ERROR < FATAL
                trace：追踪，是最低的日志级别，相当于追踪程序的执行
                debug：调试，一般在开发中，都将其设置为最低的日志级别
                info：信息，输出重要的信息，使用较多
                warn：警告，输出警告的信息
                error：错误，输出错误信息
                fatal：严重错误
        -->
        <root level="DEBUG">
            <appender-ref ref="spring6log"/>
            <appender-ref ref="RollingFile"/>
            <appender-ref ref="log"/>
        </root>
    </loggers>

    <appenders>
        <!--输出日志信息到控制台-->
        <console name="spring6log" target="SYSTEM_OUT">
            <!--控制日志输出的格式-->
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss SSS} [%t] %-3level %logger{1024} - %msg%n"/>
        </console>

        <!--文件会打印出所有信息，这个log每次运行程序会自动清空，由append属性决定，适合临时测试用-->
        <File name="log" fileName="d:/spring6_log/test.log" append="false">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} %-5level %class{36} %L %M - %msg%xEx%n"/>
        </File>

        <!-- 这个会打印出所有的信息，
            每次大小超过size，
            则这size大小的日志会自动存入按年份-月份建立的文件夹下面并进行压缩，
            作为存档-->
        <RollingFile name="RollingFile" fileName="d:/spring6_log/app.log"
                     filePattern="log/$${date:yyyy-MM}/app-%d{MM-dd-yyyy}-%i.log.gz">
            <PatternLayout pattern="%d{yyyy-MM-dd 'at' HH:mm:ss z} %-5level %class{36} %L %M - %msg%xEx%n"/>
            <SizeBasedTriggeringPolicy size="50MB"/>
            <!-- DefaultRolloverStrategy属性如不设置，
            则默认为最多同一文件夹下7个文件，这里设置了20 -->
            <DefaultRolloverStrategy max="20"/>
        </RollingFile>
    </appenders>
</configuration>
```

## IoC容器

### 概念

控制反转,所有Java对象的实例化和初始化，控制对象与对象之间的依赖关系 ，通过ioc管理的对象叫做Spring Bean,使用map集合去存放bean对象

基本流程，在xml文件中配置Bean定义信息，经过BeanDefinitionReader进行读取加载,ioc通过BeanFactory工厂+反射来创建对象(实例化),再通过初始化和content.getBean(id)来获取最终对象

```java
  public void testUserObject(){
        //通过读取类路径下的xml配置文件来创建ioc容器对象
        ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
        //获取创建的对象
        User user = (User)context.getBean("user");
        //调用方法测试
        user.add();
    }
```

BeanFactory是ioc容器的基本实现，面向Spring本身，我们使用的是他的子接口 如 ApplicationContext的

ClassPathXMLApplicationContext :通过读取类路径下的XML格式的配置文件创建IOC容器对象，还有其他，FileSystem——，Configurable——等。

依赖注入(DI)的两种方法：set注入和构造注入

### 基于XML管理bean

#### 获取bean

预先加入依赖， 创建User对象，创建bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--User对象的创建-->
<bean id="user" class="com.jason.test.User"></bean>
</beans>
```

```java
/**
 * 获取bean
 * */
public class TestUser {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
        //根据id获取
        //User user = (User) context.getBean("user");
        //根据类型获取bean,但xml配置中只能有一种类型，不能重复
        //User user2 = context.getBean(User.class);
        //根据id和类型获取
        User user3 = context.getBean("user", User.class);

    }
}

```

实验：1：组件实现了接口，可以根据接口获取bean吗 2：如果一个接口有多个实现类，这些实现类都配置了bean，根据接口可以实现bean吗

先验证第一个： 创建UserDao接口和UserDaoImpl实现类

bean.xml创建实现类对象

```xml
<!--一个接口实现类获取-->
    <bean id="userdao" class="com.jason.test.bean.UserDaoImpl"></bean>
```

测试是否调用成功

```java
public static void main(String[] args) {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
       //根据类型获取接口对应的bean
        UserDao userdao = context.getBean(UserDao.class);
        userdao.run();

    }
```

接口可以获取bean，前提是bean是唯一的

同理可得，在创建一个personimpl的实现类继承Userdao接口，然后再配置了bean ， 接口类型就不可以获取bean了，因为bean不唯一

#### 依赖注入之setter注入

在bean中配置

```xml
   <!--set注入-->
    <bean id="book" class="com.jason.test.setter.Book">
        <property name="bname" value="三体1"></property>
        <property name="author" value="刘慈欣"></property>
    </bean>
```

```java
   @Test
    public void testSetter(){
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
        Book book = context.getBean("book", Book.class);
        System.out.println(book);
    }
```

#### 构造器注入

```java
<!--    构造器注入-->
    <bean id="book2" class="com.jason.test.setter.Book">
        <constructor-arg name="bname" value="三体2"></constructor-arg>
        <constructor-arg name="author" value="刘慈欣"></constructor-arg>
    </bean>
```

#### 对象类型属性注入

```xml
<!--在bean.xml中先创建两个对象-->
<bean id="dept" class"---略">
	<property name="dname" value="技术部"></property>
</bean>
<bean id="emp" class="--略">
    <!--普通属性注入-->
    <property name="ename" value="lucy"></property>
    <!--外部bean方式注入对象类型属性 , 通过ref引入 需要注入的对象id-->
    <property name="dept" ref="dept"></property>
    <!---内部bean方式注入-->
    <property name="dept">
    	<bean id="dept2" class="---略">
        	<property name="dname" value="技术部"></property>
        </bean>
    </property>
    <!--级联赋值-->
    <property name="dept.dname" value="技术部"></property>
</bean>
```

#### 数组类型注入

```xml
<bean id="dept" class"---略">
	<property name="dname" value="技术部"></property>
</bean>
<bean id="emp" class="--略">
    <!--普通属性注入-->
    <property name="ename" value="lucy"></property>
    <!---数组类型的注入-->
    <property name="loves">
    	<array>
        	<value>1</value>
            <value>1</value>
            <value>1</value>
        </array>
    </property>
</bean>
```

List类型和Map类型的注入注入

```xml
<property name="emplist">
	<list>
    	<ref bean="emp1"></ref>
        <ref bean="emp2"></ref>
    </list>
</property>
<!--Map类型注入-->
<property name="empmap">
	<map>
    	<entry key="001" value-ref="dept1"></entry>
        <entry key="002" value-ref="dept2  "></entry>
    </map>
</property>
```

#### p命名空间注入

```xml
<!--添加命名空间-->
 ...
 xmlns:p="http://www.springframework.org/schema/p"
...
 http://www.springframework.org/schema/p
<!--p命名空间注入-->
<bean id="detp" class="...略" p:sid="100" p:sname="sont" p:emplist-ref="emplist" p:empmap-ref="empmap"></bean>
```

#### 模拟

添加数据库依赖

```xml
<!--        MySQL驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.30</version>
        </dependency>
<!--        数据源-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.31</version>
        </dependency>
```

创建外部属性文件

```properties
jdbc.user=root
jdbc.password=atguigu
jdbc.url=jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC
jdbc.driver=com.mysql.cj.jdbc.Driver
```

创建bean-jdbc.xm

```xml
<?xml version="1.0" encoding="UTF-8"?>
 <!--添加context配置-->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

<!--    引入外部属性文件-->
    <context:property-placeholder location="jdbc.properties"></context:property-placeholder>

<!--    完成数据库信息的注入-->
    <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="url" value="${jdbc.url}"></property>
        <property name="name" value="${jdbc.user}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>
</beans>
```



#### bean作用域

通过配置bean标签中的scope属性来指定bean的作用于范围

singletom(默认) ：在IOC容器中，这个bean的对象始终为单实例，在IOC容器初始化时创建对象

prototype:在IOC容器中有多个实例，在获取bean时创建对象

#### bean的生命周期

1. bean对象的创建(调用构造器)
2. 给bean对象设置相关属性
3. bean调用后置处理器BeanPostProcessor(初始化前方法) 记得放入ioc容器中
4. bean对象初始化(调用指定初始化方法，init-method)
5. bean调用后置处理器BeanPostProcessor(初始化后方法)
6. bean对象创建完成，使用
7. bean对象销毁(配置指定销毁方法 destroy-method)
8. loC容器关闭

#### 自动装配

根据制定策略，在ioc容器中匹配某一个bean，自动为指定的bean中所依赖的类型或接口属性赋值

准备工作：UserDao，UserService的接口和实现类里面添加自定义方法，添加UserController类，在UserController中添加 UserService属性，添加UserService的set方法，然后在方法中调用UserService中的方法，然后再UserServiceImpl中添加Userdao属性进行相同操作，调用Userdao的方法

原始操作要在UserService中创建UserImpl的实现类，然后再UserController创建UserServiceImpl的实现类，现在用spring来进行简化

创建bean-auto.xml (根据类型来自动装配) autowire="byType"

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="userController" class="com.jason.test.auto.UserController" autowire="byType"></bean>
    <bean id="userService" class="com.jason.test.auto.UserServiceImpl" autowire="byType"></bean>
    <bean id="userDao" class="com.jason.test.auto.UserDaoImpl"></bean>
</beans>
```

若在IOC中，没有任何一个兼容类型的bean能为属性赋值，默认为null。

byName方法通过属性的Set方法名去除Set后的方法名来判断的，非属性名

### 基于注解管理Bean

#### 步骤

通过注解自动装配的步骤，简化xml配置

1. 引入依赖
2. 开启组件扫描
3. 使用注解定义Bean
4. 依赖注入

#### 组件扫描

```xml
<!--    开启组件扫描,类中加入@Component注解，就将该类装配到容器中-->
    <context:component-scan base-package="com.jason.test">
<!--        context:exclude-filter标签：指定排除规则-->
<!--        type:设置排除或包含的依据
            type="annotation",根据注解排除，expression中设置全路径
            type="assignable",根据类型排除，expression中设置全路径
-->
<!--       include-filter 代表只扫描此类上的注解-->
        <context:include-filter type="annotation" expression="com.jason.test.User"/>
```

#### 使用注解定义Bean

| 注解        | 说明                                   |
| ----------- | -------------------------------------- |
| @Compnent   | 用于描述spring中的Bean，可用于任何层次 |
| @Repository | 多用于数据访问层(Dao层)                |
| @Service    | 多用于业务层(Service层)                |
| @Controller | 多用于控制层(Controller)               |

在需要定义的类上添加注解即可

#### @Autowired注入

@Autowired默认用类型进行装配

```java
@Controller
public class UserController {
    //注入Service
    //1. 属性注入
//    @Autowired//根据类型找到对应对象，完成注入
//    private UserService userService;

    //2.set方法注入
//    private UserService userService;
//    @Autowired
//    public void setUserService(UserService userService) {
//        this.userService = userService;
//    }

    //3.构造方法注入
//    private UserService userService;
//    @Autowired
//    public UserController(UserService userService) {
//        this.userService = userService;
//    }

    //4. 只有一个有参构造器时，可以省略@Autowired注解
//    private UserService userService;
//    public UserController(UserService userService) {
//        this.userService = userService;
//    }

    //5.@Autowired和@Qualifier注解联合,根据名称和类型注入
    @Autowired
    @Qualifier(value = "userServiceImpl")//默认名字是类的首字母小写
    private UserService userService;


    public void addUser(){
        userService.addUserService();
    }
}
```

#### @Resource注入

@Resource默认用名称进行装配byName，未指定名称时，使用属性名作为name，找不到name时自动启动通过类型byType装配

需要提前引入依赖

```xml
<!--        @Resource依赖-->
        <dependency>
            <groupId>jakarta.annotation</groupId>
            <artifactId>jakarta.annotation-api</artifactId>
            <version>2.1.1</version>
        </dependency>
```

```java
public class UserController {
    //1.    根据名称进行注入
    @Resource(name = "myuserService")
    private UserService userService;

```

#### Spring全注解开发

用配置类去替代配置文件(bean.xml)

创建SpringConfig配置类

```java
package com.jason.test.bean.auto.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration //配置类
@ComponentScan("com.jason.test.bean.auto")//开启组件扫描
public class SpringConfig {
}
```

测试类

```java
 public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
        UserController bean = context.getBean("userController", UserController.class);
        bean.addUser();
    }
}
```

### 手写SpringIoc

先新建子模块，然后创建service接口和dao接口以及他们的实现类，再UserServiceImpl中获取userdao中的方法，新建Bean注解和DI注解，Bean注解用于创建对象，DI注解用于进行属性注入

创建anno包，新建Bean注解和DI注解

```java
/**
 * Bean注解
 * */
//能在类，接口上使用，作用范围是在运行时生效
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Bean {
}
```

```java
/**
 * DI注解
 * */
//能在属性上使用，作用范围是在运行时生效
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface DI {
}

```

创建bean包，新建ApplicationContext接口和AnnotationApplicationContext类

```java
package com.jason.bean;
/**
 * ApplicationContext 接口
 * */
public interface ApplicationContext {
    //返回对象
    Object getBean(Class clazz);
}
```

```java
package com.jason.bean;

import com.jason.anno.Bean;
import com.jason.anno.DI;

import java.io.File;
import java.lang.reflect.Field;
import java.net.URL;
import java.net.URLDecoder;
import java.util.Enumeration;
import java.util.HashMap;
import java.util.Map;
/**
 * AnnotationApplicationContext实现类
 * */
public class AnnotationApplicationContext implements ApplicationContext{
    //创建map集合，放bean对象
    private  Map<Class,Object> beanFactory= new HashMap<>();
    private static String rootPath;

    //根据传入的class返回对象
    @Override
    public Object getBean(Class clazz) {
        return beanFactory.get(clazz);
    }
    //创建有参数构造，包路径,设置包扫描规则
    //当类中有@bean注解，就将类反射进行实例化
     public AnnotationApplicationContext(String basePackage){
        //我们传进来的包名不能直接用作路径，我们要把com.jason.anno中的点替换成\方可使用
         String packagePath = basePackage.replaceAll("\\.", "\\\\");
         //获取包所在绝对路径
         try {
             Enumeration<URL> urls = Thread.currentThread().getContextClassLoader().getResources(packagePath);
             URL url = urls.nextElement();
             //需要进行转码操作,获得的斜杠是其他类型编码
             String filePath = URLDecoder.decode(url.getFile(), "utf-8");
             //因为包前面的路径
             // 都是一样的，所以进行包前面路径的字符串截取
             rootPath = filePath.substring(0, filePath.length() - packagePath.length());
             //包扫描
             loadBean(new File(filePath));
         } catch (Exception e) {
             e.printStackTrace();
         }

         //属性注入
         loadDi();
     }
    //包扫描的过程，Bean实例化
    private void loadBean(File file) throws Exception{
        //1.判断当前内容是否为文件夹
        if(file.isDirectory()){
            //2.获取文件夹中的所有内容(文件和文件夹)
            File[] childFiles = file.listFiles();
            //3. 判断文件夹里面为空，直接返回
            if(childFiles ==null || childFiles.length==0){
                return;
            }
            //4，如果文件夹里面不为空，遍历文件夹所有的内容\
            for (File child : childFiles) {
                //4.1 遍历得到每一个File对象，继续判断，如果还是文件夹，递归
                if(child.isDirectory()){
                    //递归
                    loadBean(child);
                }else{
                    //4.2 遍历得到File对象不是文件夹，是文件
                    //4.3 得到包路径+类名称部分-字符串截取
                    String pathWithClass = child.getAbsolutePath().substring(rootPath.length() - 1);
                    //4.4 判断文件是否为.class类型
                    if(pathWithClass.contains(".class")){
                        //4.5 如果是.class类型，把路径\替换成.  把.class去掉
                        String allName = pathWithClass.replaceAll("\\\\", ".").replace(".class","");
                        //4.6 判断类上面是否有注解@Bean 如果有实例化过程
                        //获取类的Class
                        Class<?> clazz = Class.forName(allName);
                        //判断不是接口
                        if(!clazz.isInterface()){
                            //判断类上是否有@Bean注解
                            Bean annotation = clazz.getAnnotation(Bean.class);
                            if(annotation !=null){
                                //实例化
                                Object instance = clazz.getConstructor().newInstance();
                                //4.7 把对象实例化之后，放入map集合beanFactory
                                //4.7.1 判断当前类如果有接口，让接口class作为map的key
                                if(clazz.getInterfaces().length>0){
                                    beanFactory.put(clazz.getInterfaces()[0],instance);
                                }else {
                                    beanFactory.put(clazz,instance);
                                }
                            }
                        }
                    }
                }
            }
        }
    }
    //属性注入
    private void loadDi() {
        //1.遍历beanFactory得map集合获取里面的对象(value)，每个对象属性获取到
        for (Map.Entry<Class, Object> entries : beanFactory.entrySet()) {
            //2.遍历的到的每个对象属性数组，得到每个属性
            Object obj = entries.getValue();
            //获取对象Class
            Class<?> clazz = obj.getClass();
            for (Field filed : clazz.getDeclaredFields()) {
                //3.判断属性上是否有@DI注解
                if(filed.getAnnotation(DI.class)!=null) {
                    filed.setAccessible(true);
                    //4. 如果有注解 把对象进行设置(注入)
                    try {
                        //根据类型 获得对象 然后进行注入
                        filed.set(obj,beanFactory.get(filed.getType()));
                    } catch (IllegalAccessException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
}
```

测试类测试

```java
package com.jason;
public class TestUser {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationApplicationContext("com.jason");
        UserService userService =(UserService) context.getBean(UserService.class);
        userService.add();
    }
}

```

## 面向切面：AOP

### 代理模式

调用目标方法时，先调用代理对象的方法，减少对目标方法的调用，同时让附加功能能集中在一起方便维护，把不属于核心逻辑的代码从方法中剥离出来，达到解耦的目的

静态代理确实实现了解耦，但是由于代码都写死了，完全不具备任何的灵活性。就拿日志功能来说，将来其他地方也需要附加日志，那还得再声明更多个静态代理类，那就产生了大量重复的代码，日志功能还是分散的，没有统一管理。提出进一步的需求：将日志功能集中到一个代理类中，将来有任何日志需求，都通过这一个代理类来实现。这就需要使用动态代理技术了。

### 动态代理

```java
/**
* 接口
*/
public interface Calcaulator {
    int add(int i,int j);//加
    int sub(int i,int j);//减
    int mul(int i,int j);//乘
    int div(int i,int j);//除
}
```

```java
package com.jason.aop;

import org.aopalliance.intercept.Invocation;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.util.Arrays;
/**
 * 生产代理对象的工具类
 * */
public class ProxyFactory {
    //目标对象
    private Object target;

    public ProxyFactory(Object target) {
        this.target = target;
    }

    //返回代理对象
    public Object getProxy(){
        /**
         * newProxyInstance()：创建一个代理实例
         * 其中有三个参数：
         * 1、classLoader：加载动态生成的代理类的类加载器
         * 2、interfaces：目标对象实现的所有接口的class对象所组成的数组
         * 3、invocationHandler：设置代理对象实现目标对象方法的过程，即代理类中如何重写接口中的抽象方法
         */
        ClassLoader classLoader = target.getClass().getClassLoader();
        Class<?>[] interfaces = target.getClass().getInterfaces();
        InvocationHandler invocationHandler = new InvocationHandler(){
            /**
             * proxy 代理对象
             * method 需要重写目标对象的方法
             * method 方法里面参数
             * args method方法里面的参数
             * */
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                //方法调用之前输出
                System.out.println("[动态代理][日志] "+method.getName()+"，参数："+ Arrays.toString(args));
                //调用目标方法
                Object result = method.invoke(target, args);
                //方法调用之后输出
                System.out.println("[动态代理][日志] "+method.getName()+"，结果："+ result);
                return result;
            }
        };
        return Proxy.newProxyInstance(classLoader,interfaces,invocationHandler);
    }
}

```

```java
package com.jason.aop;
/**
 * 测试类
 * */
public class Test1 {
    public static void main(String[] args) {
        //创建代理对象(动态)
        ProxyFactory proxyFactory = new ProxyFactory(new CalcaulatorImpl());
        //得到具体对象
        Calcaulator proxy = (Calcaulator) proxyFactory.getProxy();
        //调用目标方法
        proxy.add(2,3);
    }
}
```

### 基于注解的AOP

JDK动态代理：JDK原生的实现方式，代理对象和和目标对象实现同样的接口

cglib动态代理：没有接口，通过继承被代理的目标类  ，生成子类代理对象

AspectJ：本质上是静态代理，Spring只是借用了AspectJ中的注解。

引入依赖

```xml
<!--spring aop依赖-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aop</artifactId>
        <version>6.0.2</version>
    </dependency>
    <!--spring aspects依赖-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>6.0.2</version>
    </dependency>
```

创建接口和实现类

```java
package com.jason.aop.annoaop;
/**
 * 接口
 * */
public interface Calcaulator {
    int add(int i,int j);//加
    int sub(int i,int j);//减
    int mul(int i,int j);//乘
    int div(int i,int j);//除
}
```

```java
package com.jason.aop.annoaop;
/**
 * 实现类
 * */
@Component
public class CalcaulatorImpl implements Calcaulator {
    @Override
    public int add(int i, int j) {
        int result =i+j;
        System.out.println("方法内部result="+result);
        return result;
    }
    @Override
    public int sub(int i, int j) {
        int result =i-j;
        System.out.println("方法内部result="+result);
        return result;
    }
    @Override
    public int mul(int i, int j) {
        int result =i*j;
        System.out.println("方法内部result="+result);
        return result;
    }
    @Override
    public int div(int i, int j) {
        int result =i/j;
        System.out.println("方法内部result="+result);
        return result;
    }
}

```

添加前置通知 创建切面类

```java
package com.jason.aop.annoaop;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

import java.util.Arrays;

/**
 * 切面类
 * */
@Aspect //切面类
@Component //ioc容器
public class LogAspect {
    //设置切入点和通知类型
    //切入点表达式：execution(访问修饰符 增强方法返回类型 增强方法所在类全路径.方法名称(方法参数))
    //通知类型：前置，后置，异常，返回，环绕
    //前置通知 @Before(value="切入点表达式配置切入点")
    //在此类下的所有方法做前置通知
    @Before("execution(public int com.jason.aop.annoaop.CalcaulatorImpl.*(..))")
    //通过JoinPoint获取他的方法名字和参数等
    public void beforeMethod(JoinPoint joinPoint){
        String name = joinPoint.getSignature().getName();
        Object[] args = joinPoint.getArgs();
        System.out.println("Logger-->前置通知，"+"增强方法的名字"+name+"参数"+ Arrays.toString(args));
    }
    //返回通知
    @AfterReturning("execution(public int com.jason.aop.annoaop.CalcaulatorImpl.*(..))", returning = "result")
    //多一个返回参数
    public void afterReturningMethod(JoinPoint joinPoint, Object result){
        String methodName = joinPoint.getSignature().getName();
        System.out.println("Logger-->返回通知，方法名："+methodName+"，结果："+result);
    }
    //异常通知
    @AfterThrowing("execution(public int com.jason.aop.annoaop.CalcaulatorImpl.*(..))", throwing = "ex")
    public void afterThrowingMethod(JoinPoint joinPoint, Throwable ex){
        String methodName = joinPoint.getSignature().getName();
        System.out.println("Logger-->异常通知，方法名："+methodName+"，异常："+ex);
    }
    //环绕通知
    @Around("execution(public int com.jason.aop.annoaop.CalcaulatorImpl.*(..))")
    public Object aroundMethod(ProceedingJoinPoint joinPoint){
        String methodName = joinPoint.getSignature().getName();
        String args = Arrays.toString(joinPoint.getArgs());
        Object result = null;
        try {
            System.out.println("环绕通知-->目标对象方法执行之前");
            //目标对象（连接点）方法的执行
            result = joinPoint.proceed();
            System.out.println("环绕通知-->目标对象方法返回值之后");
        } catch (Throwable throwable) {
            throwable.printStackTrace();
            System.out.println("环绕通知-->目标对象方法出现异常时");
        } finally {
            System.out.println("环绕通知-->目标对象方法执行完毕");
        }
        return result;
    }
}

```

创建bean.xml 开启组件扫描和aspectj自动代理

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">
    <!--   开启组件扫描-->
    <context:component-scan base-package="com.jason.aop.annoaop"></context:component-scan>
    <!--开启aspectj自动代理，为目标对象生成代理-->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```

测试

```java
public class Test1 {
    @Test
    public void add(){
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
        Calcaulator calcaulator = context.getBean(Calcaulator.class);
        System.out.println(calcaulator.add(1, 2));
    }
}
```

优化

切入点表达式配置切入点

```java
@Pointcut("execution(public int com.jason.aop.annoaop.*.*(..))")
public void pointCut(){}
```

```java
//同切面直接调用即可，不同切面前面加上包调用
@Before("pointCut()")
@Before("com.atguigu.aop.CommonPointCut.pointCut()")
```

使用@order来控制切面优先级 数越小，优先级越高

### 基于xml的AOP

删除切面类中的切面类注解和通知类型注解，只保留ioc容器注解 创建beanaop.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">
    <!--   开启组件扫描-->
    <context:component-scan base-package="com.jason.aop.annoaop"></context:component-scan>
<!--    配置aop五种通知类型-->
    <aop:config>
        <!--配置切面类-->
        <aop:aspect ref="logAspect">
<!--            配置切入点-->
            <aop:pointcut id="pointcut" expression="execution(public int com.jason.aop.annoaop.CalcaulatorImpl.*(..))"/>
<!--       配置通知类型 前置通知-->
            <aop:before method="beforeMethod" pointcut-ref="pointcut"></aop:before>
<!--            返回通知-->
           <aop:after-returning method="afterReturningMethod" returning="result" pointcut-ref="pointcut"></aop:after-returning>
        </aop:aspect>
    </aop:config>
</beans>
```

### JUnit5 整合spring6

```xml
    <!--spring对junit的支持相关依赖-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>6.0.2</version>
    </dependency>
```

写一个User类，创建一个方法，创建bean.xml开启组件扫描

```java
package com.jason.spring6;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.junit.jupiter.SpringJUnitConfig;

//通过添加注解指定路径
@SpringJUnitConfig(locations = "classpath:bean.xml")
public class JUit5Test {
    //注入
    @Autowired
    private User user;
    //测试
    @Test
    public void testUser(){
        System.out.println(user);
        user.run();
    }
}
```

## 事务

### 使用 JdbcTemplate 方便实现对数据库操作

 添加依赖

```xml
  <!--spring jdbc  Spring 持久化层支持jar包-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>6.0.2</version>
        </dependency>
<!--        MySQL驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.30</version>
        </dependency>
<!--        数据源-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.31</version>
        </dependency>
```

创建jdbc.properties

```java
jdbc.user=root
jdbc.password=root
jdbc.url=jdbc:mysql://localhost:3306/spring?characterEncoding=utf8&useSSL=false
jdbc.driver=com.mysql.cj.jdbc.Driver
```

创建数据表

```sql
CREATE DATABASE `spring`;

use `spring`;

CREATE TABLE `t_emp` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(20) DEFAULT NULL COMMENT '姓名',
  `age` int(11) DEFAULT NULL COMMENT '年龄',
  `sex` varchar(2) DEFAULT NULL COMMENT '性别',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

```java
/**
* 测试类
*/
@SpringJUnitConfig(locations = "classpath:bens.xml")
public class JdbcTemplateTest {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Test
    public void testUpdate(){
        //sql语句
        String sql ="insert into t_emp values(null,?,?,?)";
        //返回影响行数
        int update = jdbcTemplate.update(sql, "millet", 20, "女", 1);
    }
}

```

```java
@SpringJUnitConfig(locations = "classpath:bens.xml")
public class JdbcTemplateTest {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Test
    public void testUpdate(){
        //sql语句
        String sql ="insert into t_emp values(null,?,?,?)";
        //返回影响行数
        int update = jdbcTemplate.update(sql, "millet", 20, "女", 1);
    }
    @Test
    //1.根据结果集手动封装 返回单个对象,单个查询
    public void testSelect(){
//        User user = new User();
//        String sql="select * from t_emp where id=?";
//        User userresult = jdbcTemplate.queryForObject(sql,
//                (rs, rowNum) -> {
//                    user.setName(rs.getString("name"));
//                    user.setAge(rs.getInt("age"));
//                    user.setSex(rs.getString("sex"));
//                    return user;
//                },1);
//        System.out.println(userresult);
        // 2.通过实现类实现查询
//        String sql="select * from t_emp where id=?";
//        User user = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<>(User.class),1);
//        System.out.println(user);
        //3. 返回list集合
//        String sql="select * from t_emp where id=?";
//        List<User> userList = jdbcTemplate.query(sql, new BeanPropertyRowMapper<>(User.class));
    }
    //查询表里有多少记录
    @Test
    public void testSelectValue(){
        String sql ="select count(*) from t_emp";
        //第二个参数表示返回类型的class
        Integer count = jdbcTemplate.queryForObject(sql, Integer.class);
        System.out.println(count);
    }

```

### 基于注解的声明式事务

事务在代码里或者数据库中都可以配置。

其含义理解为 一系列的数据操作，要么全部执行完成、要么都不执行。归纳为

1、原子性：事务是一个原子操作，由一系列动作组成。事务的原子性确保动作要么全部完成，要么完全不起作用。

2、一致性：一旦事务完成（不管成功还是失败），系统必须确保它所建模的业务处于一致的状态，而不会是部分完成部分失败

3、隔离性：事务之间应该隔离开来。因为可能有许多事务会同时处理相同的数据，每个事务都应该与其他事务有隔离策略。

4、持久性：一旦事务完成，它的结果不会收到影响。通常情况下，事务的结果被写到持久化存储器中。

声明式事务管理建立在AOP之上，其本质是对方法前后进行拦截，然后在目标方法开始之前创建或者加入一个事务，执行完目标方法之后根据执行的情况提交或者回滚。

模拟场景 买书流程

创建dao和service的接口和实现类，添加买书方法，添加Controller类

创建bens.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

    <context:component-scan base-package="com.jason.spring6.tx"></context:component-scan>

<!--    引入外部属性文件,创建数据源对象-->
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
<!--    创建jdbcTemplate对象，注入数据源-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="druidDataSource"></property>
    </bean>
</beans>
```

```java
package com.jason.spring6.tx.dao;
public interface BookDao {
    Integer getBookPriceByBookId(Integer bookid);
    void updateStock(Integer bookid);
    void updateUserBalance(Integer userid, Integer price);
}
```

```java
/**
 * Dao实现类
 * */
@Repository
public class BookDaoImpl implements BookDao{
    @Autowired
    private JdbcTemplate jdbcTemplate;
    @Override
    //根据图书id查询图书的价格
    public Integer getBookPriceByBookId(Integer bookid) {
        String sql ="select price from t_book where book_id=?";
        Integer price = jdbcTemplate.queryForObject(sql, Integer.class, bookid);
        return price;
    }
    @Override
    //更新库存量 -1
    public void updateStock(Integer bookid) {
        String sql = "UPDATE t_book SET stock=stock-1 WHERE book_id =?";
        jdbcTemplate.update(sql,bookid);
    }
    @Override
    //更新用户表用户余额 -图书价格
    public void updateUserBalance(Integer userid, Integer price) {
        String sql ="UPDATE t_user SET balance= balance -? WHERE user_id=?";
        jdbcTemplate.update(sql,price,userid);
    }
}
```

```java
package com.jason.spring6.tx.service;
public interface BookService {
    void buyBook(Integer bookid, Integer userid);
}
```

```java
/**
 * Service层
 * */
@Service
public class BookServiceImpl implements BookService{
    @Autowired
    private BookDao bookDao;
    @Override
    public void buyBook(Integer bookid, Integer userid) {
        //根据图书id查询图书的价格
        Integer price =bookDao.getBookPriceByBookId(bookid);
        //更新库存量 -1
        bookDao.updateStock(bookid);
        //更新用户表用户余额 -图书价格
        bookDao.updateUserBalance(userid,price);
    }
}
```

```java
/**
 * Controller
 * */
@Controller
public class BookController {
    @Autowired
    private BookService bookService;
    //买书：图书id和用户id
    public void buyBook(Integer bookid,Integer userid){
        bookService.buyBook(bookid,userid);
    }
}
```

```java
@SpringJUnitConfig(locations = "classpath:bens.xml")
public class TestBook {
    @Autowired
    private BookController bookController;
    @Test
    public void testBuyBook(){
        bookController.buyBook(1,1);
    }
}
```

但是当我们余额小于买书价格时，库存也减少了，我们希望这三个方法在执行时，只要一个不满足就会失败，方法回滚，声明式事务

配置文件中添加配置

```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="druidDataSource"></property>
</bean>

<!--
    开启事务的注解驱动
    通过注解@Transactional所标识的方法或标识的类中所有的方法，都会被事务管理器管理事务
-->
<!-- transaction-manager属性的默认值是transactionManager，如果事务管理器bean的id正好就是这个默认值，则可以省略这个属性 -->
<tx:annotation-driven transaction-manager="transactionManager" />
```

在ServiceImpl类上写上 @Transactional 就可以实现声明式事务

### 事务属性

只读 ： @Transactional(readOnly = true)  操作不涉及写操作。这样数据库就能够针对查询操作来进行优化，但是只能用在查询操作中，在增删改操作中不被允许

超时：在程序长时间占用资源或者出问题时，程序应该被回滚，释放资源  

```java
//超时时间单位秒
@Transactional(timeout = 3)
public void buyBook(Integer bookId, Integer userId) {
    try {
        TimeUnit.SECONDS.sleep(5);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
//...
}
```

回滚策略

声明式事务默认只针对运行时异常回滚，编译时异常不回滚。可以通过@Transactional中相关属性设置回滚策略

- rollbackFor属性：需要设置一个Class类型的对象
- rollbackForClassName属性：需要设置一个字符串类型的全类名

- noRollbackFor属性：需要设置一个Class类型的对象
- rollbackFor属性：需要设置一个字符串类型的全类名

```java
@Transactional(noRollbackFor = ArithmeticException.class)
//@Transactional(noRollbackForClassName = "java.lang.ArithmeticException")
public void buyBook(Integer bookId, Integer userId) {
    //查询图书的价格
    Integer price = bookDao.getPriceByBookId(bookId);
    //更新图书的库存
    bookDao.updateStock(bookId);
    //更新用户的余额
    bookDao.updateBalance(userId, price);
    System.out.println(1/0);
}
```

### 隔离行为

数据库系统必须具有隔离并发运行各个事务的能力，使它们不会相互影响，避免各种并发问题。一个事务与其他事务隔离的程度称为隔离级别。SQL标准中规定了多种事务隔离级别，不同隔离级别对应不同的干扰程度，隔离级别越高，数据一致性就越好，但并发性越弱。

隔离级别一共有四种：

- 读未提交：READ UNCOMMITTED

  允许Transaction01读取Transaction02未提交的修改。

- 读已提交：READ COMMITTED、

  要求Transaction01只能读取Transaction02已提交的修改。

- 可重复读：REPEATABLE READ

  确保Transaction01可以多次从一个字段中读取到相同的值，即Transaction01执行期间禁止其它事务对这个字段进行更新。

- 串行化：SERIALIZABLE

  确保Transaction01可以多次从一个表中读取到相同的行，在Transaction01执行期间，禁止其它事务对这个表进行添加、更新、删除操作。可以避免任何并发问题，但性能十分低下。

各个隔离级别解决并发问题的能力见下表：

| 隔离级别         | 脏读 | 不可重复读 | 幻读 |
| ---------------- | ---- | ---------- | ---- |
| READ UNCOMMITTED | 有   | 有         | 有   |
| READ COMMITTED   | 无   | 有         | 有   |
| REPEATABLE READ  | 无   | 无         | 有   |
| SERIALIZABLE     | 无   | 无         | 无   |

各种数据库产品对事务隔离级别的支持程度：

| 隔离级别         | Oracle  | MySQL   |
| ---------------- | ------- | ------- |
| READ UNCOMMITTED | ×       | √       |
| READ COMMITTED   | √(默认) | √       |
| REPEATABLE READ  | ×       | √(默认) |
| SERIALIZABLE     | √       | √       |

**②使用方式**

```java
@Transactional(isolation = Isolation.DEFAULT)//使用数据库默认的隔离级别
@Transactional(isolation = Isolation.READ_UNCOMMITTED)//读未提交
@Transactional(isolation = Isolation.READ_COMMITTED)//读已提交
@Transactional(isolation = Isolation.REPEATABLE_READ)//可重复读
@Transactional(isolation = Isolation.SERIALIZABLE)//串行化
```

### 全注解开发

用配置类去代替配置文件

```java
/**
 * 配置类
 * */
@Configuration
@ComponentScan("com.jason.spring6.tx") //组件扫描
@EnableTransactionManagement //开启事务
public class SpringConfig {
    @Bean
    public DataSource getDataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/spring?characterEncoding=utf8&useSSL=false");
        dataSource.setUsername("root");
        dataSource.setPassword("123456");
        return dataSource;
    }
//创建jdbcTemplate对象，注入数据源
    @Bean(name = "jdbcTemplate")
    public JdbcTemplate getJdbcTemplate(DataSource dataSource){
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    }

    @Bean //事务管理器
    public DataSourceTransactionManager getDataSourceTransactionManager(DataSource dataSource){
        DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
        dataSourceTransactionManager.setDataSource(dataSource);
        return dataSourceTransactionManager;
    }
}
```

### 基于xml声明式事务

添加依赖

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-aspects</artifactId>
  <version>6.0.2</version>
</dependency>
```

修改配置类

```xml
<aop:config>
    <!-- 配置事务通知和切入点表达式 -->
    <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.atguigu.spring.tx.xml.service.impl.*.*(..))"></aop:advisor>
</aop:config>
<!-- tx:advice标签：配置事务通知 -->
<!-- id属性：给事务通知标签设置唯一标识，便于引用 -->
<!-- transaction-manager属性：关联事务管理器 -->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <!-- tx:method标签：配置具体的事务方法 -->
        <!-- name属性：指定方法名，可以使用星号代表多个字符 -->
        <tx:method name="get*" read-only="true"/>
        <tx:method name="query*" read-only="true"/>
        <tx:method name="find*" read-only="true"/>
    
        <!-- read-only属性：设置只读属性 -->
        <!-- rollback-for属性：设置回滚的异常 -->
        <!-- no-rollback-for属性：设置不回滚的异常 -->
        <!-- isolation属性：设置事务的隔离级别 -->
        <!-- timeout属性：设置事务的超时属性 -->
        <!-- propagation属性：设置事务的传播行为 -->
        <tx:method name="save*" read-only="false" rollback-for="java.lang.Exception" propagation="REQUIRES_NEW"/>
        <tx:method name="update*" read-only="false" rollback-for="java.lang.Exception" propagation="REQUIRES_NEW"/>
        <tx:method name="delete*" read-only="false" rollback-for="java.lang.Exception" propagation="REQUIRES_NEW"/>
    </tx:attributes>
</tx:advice>
```

### Resource

#### Resource实现类

为了使用统一的方式访问资源，Spring 将资源抽象为 Resource，将资源的加载抽象为 ResourceLoader。Spring 配置文件的读取以及扫描包中的 bean 都会通过 Resource 访问资源。Resource接口继承了InputStreamSource接口，提供了很多InputStreamSource所没有的方法

UrlResource访问网络资源

```java
public class UrlResourceDemo {
    public static void loadAndReadUrlResource(String path){
        // 创建一个 Resource 对象
        UrlResource url = null;
        try {
            url = new UrlResource(path);
            // 获取资源名
            System.out.println(url.getFilename());
            System.out.println(url.getURI());
            // 获取资源描述
            System.out.println(url.getDescription());
            //获取资源内容
            System.out.println(url.getInputStream().read());
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
    public static void main(String[] args) {
        //访问网络资源
        loadAndReadUrlResource("http://www.baidu.com");
    }
}
```

ClassPathResource 访问类路径下资源

```java
public class ClassPathResourceDemo {

    public static void loadAndReadUrlResource(String path) throws Exception{
        // 创建一个 Resource 对象
        ClassPathResource resource = new ClassPathResource(path);
        // 获取文件名
        System.out.println("resource.getFileName = " + resource.getFilename());
        // 获取文件描述
        System.out.println("resource.getDescription = "+ resource.getDescription());
        //获取文件内容
        InputStream in = resource.getInputStream();
        byte[] b = new byte[1024];
        while(in.read(b)!=-1) {
            System.out.println(new String(b));
        }
    }
    public static void main(String[] args) throws Exception {
        loadAndReadUrlResource("atguigu.txt");
    }
}
```

使用FileSystemResource 访问文件系统资源

```java
public class FileSystemResourceDemo {

    public static void loadAndReadUrlResource(String path) throws Exception{
        //相对路径
        FileSystemResource resource = new FileSystemResource("atguigu.txt");
        //绝对路径
        //FileSystemResource resource = new FileSystemResource("C:\\atguigu.txt");
        // 获取文件名
        System.out.println("resource.getFileName = " + resource.getFilename());
        // 获取文件描述
        System.out.println("resource.getDescription = "+ resource.getDescription());
        //获取文件内容
        InputStream in = resource.getInputStream();
        byte[] b = new byte[1024];
        while(in.read(b)!=-1) {
            System.out.println(new String(b));
        }
    }
    public static void main(String[] args) throws Exception {
        loadAndReadUrlResource("atguigu.txt");
    }
}
```

还有ServletContextResource，InputStreamResource，ByteArrayResource

#### ResourceLoader接口

Resource getResource（String location）** ： 该接口仅有这个方法，用于返回一个Resource实例,我们可以分别用ClassPathXmlApplicationContext和FileSystemXmlApplicationContext来获取ApplicationContext对象 通过getResource来获取相对应的实例，实际上并不需要直接使用Resource实现类，而是调用ResourceLoader实例的getResource()方法来获得资源，ReosurceLoader将会负责选择Reosurce实现类，也就是确定具体的资源访问策略，从而将应用程序和具体的资源访问策略分离开来

## 数据校验Validation

在开发中，我们经常遇到参数校验的需求，比如用户注册的时候，要校验用户名不能为空、用户名长度不超过20个字符、手机号是合法的手机号格式等等。如果使用普通方式，我们会把校验的代码和真正的业务处理逻辑耦合在一起，而spring validation允许通过注解的方式来定义对象校验规则，把校验和业务逻辑分离开，让代码编写更加方便。Spring Validation其实就是对Hibernate Validator进一步的封装，方便在Spring中使用。

**第一种是通过实现org.springframework.validation.Validator接口，然后在代码中调用这个类**

创建Validator接口，实现接口方法指定校验规则

```java
public class PersonValidator implements Validator {
    //supports方法用来表示此校验用在哪个类型上
    @Override
    public boolean supports(Class<?> clazz) {
        return Person.class.equals(clazz);
    }

    //validate是设置校验逻辑的规则，其中ValidationUtils，是Spring封装的校验工具类
    @Override
    public void validate(Object object, Errors errors) {
        //name不能为空  ,错误参数，属性，错误编码，提示信息
        ValidationUtils.rejectIfEmpty(errors, "name", "name.empty","name is null");
        Person p = (Person) object;
        //age 不能小于0 ， 不能大于150
        if (p.getAge() < 0) {
            errors.rejectValue("age", "error value < 0");
        } else if (p.getAge() > 150) {
            errors.rejectValue("age", "error value too old");
        }
    }
}
```

测试类

```java
public class TestMethod1 {
    public static void main(String[] args) {
        //创建person对象
        Person person = new Person();
        person.setName("lucy");
        person.setAge(-1);
        // 创建Person对应的DataBinder
        DataBinder binder = new DataBinder(person);
        // 设置校验
        binder.setValidator(new PersonValidator());
        // 由于Person对象中的属性为空，所以校验不通过
        binder.validate();
        //输出结果
        BindingResult results = binder.getBindingResult();
        System.out.println(results.getAllErrors());
    }
}
```



**第二种是按照Bean Validation方式来进行校验，即通过注解的方式。**

spring默认有一个实现类LocalValidatorFactoryBean，它实现了上面Bean Validation中的接口，并且也实现了org.springframework.validation.Validator接口。

创建配置类

```java
@Configuration
@ComponentScan("com.atguigu.spring6.validation.method2")
public class ValidationConfig {

    @Bean
    public LocalValidatorFactoryBean validator() {
        return new LocalValidatorFactoryBean();
    }
}
```

再实体类上加入注解可实现校验

```java
public class User {
    @NotNull
    private String name;
    @Min(0)
    @Max(120)
    private int age;
    //...
```

**常用注解说明**
@NotNull	限制必须不为null
@NotEmpty	只作用于字符串类型，字符串不为空，并且长度不为0
@NotBlank	只作用于字符串类型，字符串不为空，并且trim()后不为空串
@DecimalMax(value)	限制必须为一个不大于指定值的数字
@DecimalMin(value)	限制必须为一个不小于指定值的数字
@Max(value)	限制必须为一个不大于指定值的数字
@Min(value)	限制必须为一个不小于指定值的数字
@Pattern(value)	限制必须符合指定的正则表达式
@Size(max,min)	限制字符长度必须在min到max之间
@Email	验证注解的元素值是Email，也可以通过正则表达式和flag指定自定义的email格式

使用jakarta.validation.Validator校验或使用org.springframework.validation.Validator校验

```java
@Service
public class MyService1 {

    @Autowired
    private Validator validator;
    //akarta.validation.Validator校验
    public  boolean validator(User user){
        Set<ConstraintViolation<User>> sets =  validator.validate(user);
        return sets.isEmpty();
    }
    //org.springframework.validation.Validator
     public boolean validaPersonByValidator(User user) {
        BindException bindException = new BindException(user, user.getName());
        validator.validate(user, bindException);
        return bindException.hasErrors();
    }
}
```

测试

```java
public class TestMethod2 {

    @Test
    public void testMyService1() {
        ApplicationContext context = new AnnotationConfigApplicationContext(ValidationConfig.class);
        MyService1 myService = context.getBean(MyService1.class);
        User user = new User();
        user.setAge(-1);
        boolean validator = myService.validator(user);
        System.out.println(validator);
    }

    @Test
    public void testMyService2() {
        ApplicationContext context = new AnnotationConfigApplicationContext(ValidationConfig.class);
        MyService2 myService = context.getBean(MyService2.class);
        User user = new User();
        user.setName("lucy");
        user.setAge(130);
        user.setAge(-1);
        boolean validator = myService.validaPersonByValidator(user);
        System.out.println(validator);
    }
}
```



**第三种是基于方法实现校验**

创建配置类，配置MethodValidationPostProcessor

```java
@Configuration
@ComponentScan("com.atguigu.spring6.validation.method3")
public class ValidationConfig {

    @Bean
    public MethodValidationPostProcessor validationPostProcessor() {
        return new MethodValidationPostProcessor();
    }
}
```

创建实体类,定义Service类，测试

```java
public class User {

    @NotNull
    private String name;

    @Min(0)
    @Max(120)
    private int age;

    @Pattern(regexp = "^1(3|4|5|7|8)\\d{9}$",message = "手机号码格式错误")
    @NotBlank(message = "手机号码不能为空")
    private String phone;
```

```java
@Service
@Validated
public class MyService {
    
    public String testParams(@NotNull @Valid User user) {
        return user.toString();
    }

}
```

```java
public class TestMethod3 {

    @Test
    public void testMyService1() {
        ApplicationContext context = new AnnotationConfigApplicationContext(ValidationConfig.class);
        MyService myService = context.getBean(MyService.class);
        User user = new User();
        user.setAge(-1);
        myService.testParams(user);
    }
}
```

## AOP提前编译

就是提前把字节码转成机器码，在程序运行前编译，可以避免在运行时的编译性能消耗和内存消耗，可以在程序运行初期就达到最高性能，程序启动速度快

缺点：由于是静态提前编译，不能根据硬件情况或程序运行情况择优选择机器指令序列，理论峰值性能不如JIT，没有动态能力，同一份产物不能跨平台运行



