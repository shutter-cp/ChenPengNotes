# 1. SpringBoot入门

## 1.1 SpringBoot 介绍

- 简化Spring应用开发的一个框架	

- 整个Spring技术栈的一个整合
- J2EE开发的一站式解决方案

## 1.2 微服务

> 微服务：架构风格（服务微化）

- 一个应用为一组小型服务
- 可以通过http的方式进行互通
- 每个元素最终都是一个可独立替换和独立升级的软件单元

# 2. 搭建

## 2.1  直接通过idea搭建

### 2.1.1 创建工程

1. new project
![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190525010816.png)

2. 填写项目信息
![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190525010933.png)

3. 根据需要选择的
![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190525011029.png)

### 2.1.2 修改配置

1. 修改 application.properties
![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190525011142.png)
```shell
# 端口设置为80
server.port=80

# mysql
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/bike?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=root
```
2. 修改 pom.xml 配置为实时刷新
    1. 
    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <scope>true</scope>
    </dependency>
    ```
    2. 
    ```xml
    <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <configuration>
            <fork>true</fork>
        </configuration>
    </plugin>
    ```
    3. 
    ```xml
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
            </resource>
        </resources>
        ...
        ...
    </build>
    ```
    
    4. File-> Settings-> Build,Execution, Deployment-> Compiler->Build project automatically
    ![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190525011835.png)
    
    5. ctrl+alt+shift+/ -> Registry...
    ![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190525012028.png)
    



## 2.2 手动创建

1. pom.xml

   ```xml
    	<!-- Inherit defaults from Spring Boot -->
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>2.1.5.RELEASE</version>
       </parent>
   
       <!-- Add typical dependencies for a web application -->
       <dependencies>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
       </dependencies>
   ```

2. 创建一个SpringBoot启动类

```java
    @SpringBootApplication
    public class HelloWordApplication {
        public static void main(String[] args) {
            SpringApplication.run(HelloWordApplication.class, args);
        }
    }
```

3. 编写业务逻辑

```java
@Controller
public class HellowDemo01 {
    @GetMapping("/hello")
    @ResponseBody
    public String hello(){
        return "hello word!";
    }
}
```

## 2.3 jar包生成

1. pom.xml

```xml
<!-- 这个插件，可以将应用打包成一个可执行的jar包；-->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190605000803.png)

将这个应用打成jar包，直接使用java -jar的命令进行执行；

# 3. 简介

## 3.1 pom.xml

1. pom.xml中的

   ```xml
   <!-- Inherit defaults from Spring Boot -->
   <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>2.1.5.RELEASE</version>
   </parent>
   ```

   

2. spring-boot-starter-parent的父依赖是

   ```xml
   <!-- 它才是真正管理SpringBoot里面依赖版本的 -->
   <parent>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-dependencies</artifactId>
     <version>2.1.5.RELEASE</version>
     <relativePath>../../spring-boot-dependencies</relativePath>
   </parent>
   ```

   其他依赖默认无需写版本号

   没有在dependencies里面管理的还是需要些版本号

## 3.2 主入口类

1. @SpringBootApplication

 Spring Boot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用

2. SpringBootApplication的父类

   ```java
   @Target({ElementType.TYPE})
   @Retention(RetentionPolicy.RUNTIME)
   @Documented
   @Inherited
   @SpringBootConfiguration
   @EnableAutoConfiguration
   @ComponentScan(
       excludeFilters = {@Filter(
       type = FilterType.CUSTOM,
       classes = {TypeExcludeFilter.class}
   ), @Filter(
       type = FilterType.CUSTOM,
       classes = {AutoConfigurationExcludeFilter.class}
   )}
   )
   public @interface SpringBootApplication {
   ```

   1. @**SpringBootConfiguration**:Spring Boot的配置类  

      标注在某个类上，表示这是一个Spring Boot的配置类；

   2. @**Configuration**:配置类上来标注这个注解；

   3. @**EnableAutoConfiguration**：开启自动配置功能；

3. EnableAutoConfiguration的父类

   ```java
   @Target({ElementType.TYPE})
   @Retention(RetentionPolicy.RUNTIME)
   @Documented
   @Inherited
   @AutoConfigurationPackage
   @Import({AutoConfigurationImportSelector.class})
   public @interface EnableAutoConfiguration {
       String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";
   
       Class<?>[] exclude() default {};
   
       String[] excludeName() default {};
   }
   ```

   1. @**AutoConfigurationPackage**：自动配置包

   2. @**Import({AutoConfigurationImportSelector.class})**

      - AutoConfiguration

        Spring的底层注解@Import，给容器中导入一个组件；导入的组件由AutoConfigurationPackages.Registrar.class；

        将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器

      - ImportSelector 

        导入哪些组件的选择器 将所有需要导入的组件以全类名的方式返回；

        这些组件就会被添加到容器中；

      ​	   会给容器中导入非常多的自动配置类（xxxAutoConfiguration）；就是给容器中导入这个场景需要的所有组	件，并配置好这些组件；		

# 4. 配置文件

## 4.1 配置文件

SpringBoot使用一个全局的配置文件，配置文件名是固定的；

- application.properties

- application.yml

配置文件的作用：修改SpringBoot自动配置的默认值；SpringBoot在底层都给我们自动配置好；



1. YAML（YAML Ain't Markup Language）
   	- YAML  A Markup Language：是一个标记语言
      	- YAML   isn't Markup Language：不是一个标记语言；

```yaml
server:
  port: 8080
```

​	XML：

```xml
<server>
	<port>8080</port>
</server>
```



## 4. 2、YAML语法：

### 1、基本语法

k:(空格)v：表示一对键值对（空格必须有）；

以**空格**的缩进来控制层级关系；只要是左对齐的一列数据，都是同一个层级的

```yaml
server:
    port: 8080
    path: /helloWord
```

属性和值大小写铭感

### 2、值的写法

- **字面量：普通的值（数字，字符串，布尔）**

​	k: v：字面直接来写；

​		字符串默认不用加上单引号或者双引号；

​		""：双引号；不会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思

​				name:   "zhangsan \n lisi"：输出；zhangsan 换行  lisi

​		''：单引号；会转义特殊字符，特殊字符最终只是一个普通的字符串数据

​				name:   ‘zhangsan \n lisi’：输出；zhangsan \n  lisi

- **对象、Map（属性和值）（键值对）：**

​	k: v：在下一行来写对象的属性和值的关系；注意缩进

​		对象还是k: v的方式

```yaml
friends:
		lastName: zhangsan
		age: 20
```

行内写法：

```yaml
friends: {lastName: zhangsan,age: 18}
```



#### 数组（List、Set）：

用- 值表示数组中的一个元素

```yaml
pets:
 - cat
 - dog
 - pig
```

行内写法

```yaml
pets: [cat,dog,pig]
```

## 4.3 demo @ConfigurationProperties

### 列子

1. 导入依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-configuration-processor</artifactId>
       <optional>true</optional>
   </dependency>
   ```

2. bean类

   ```java
   @Component
   @ConfigurationProperties(prefix="person")
   public class Person {
       private String name;
       private Integer age;
       private Boolean boss;
       private Date birth;
   
       private Map<String,Object> maps;
       private List<Object> lists;
       private Dog dog;
   }
   ```

   - 有**@Component**才能使用**@ConfigurationProperties(prefix="person")**
   - **prefix="person"** 是指在配置文件中标记为**person**

3. application.yml

   ```yml
   person:
     name: chenpeng
     age: 21
     boss: true
     birth: 2019/07/12
     maps: {k1: v1,k2: v2}
     lists:
       - list1
       - list2
     dog:
       name: 小狗
       age: 12
   ```

4. 测试类

   ![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190605013620.png)

5. application.properties

   ```properties
   person.name=fengyogn
   person.age=21
   person.birth=1992/12/12
   person.boss=false
   person.maps.k1=v1
   person.maps.k2=v2
   person.lists=1,2,3,4,5
   person.dog.name=haha
   pseron.dog.age=10
   ```



## 4.4 demo2 @Value

```java
@Component
public class Dogs {
    @Value("${dogs.test}")
    private String name;
    @Value("#{11*2}")
    private Integer age;
    @Value("true")
    private Boolean boss;
	/*get set*/
}
```

## 4.5 demo3 @Validated

@ConfigurationProperties 支持

@Value 不支持

![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190609220736.png)

## 4.6 注意

1. **properties配置文件在idea中默认utf-8可能会乱码**

   file-> settings-> Editor-> File Encodings

   ![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190605014430.png)

2. **@Value获取值和@ConfigurationProperties获取值比较**

|                      | @ConfigurationProperties | @Value     |
| -------------------- | ------------------------ | ---------- |
| 功能                 | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法） | 支持                     | 不支持     |
| SpEL                 | 不支持                   | 支持       |
| JSR303数据校验       | 支持                     | 不支持     |
| 复杂类型封装         | 支持                     | 不支持     |

​	配置文件yml还是properties他们都能获取到值；

​	如果说，我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用@Value；

​	如果说，我们专门编写了一个javaBean来和配置文件进行映射，我们就直接使用@ConfigurationProperties；

## 4.7 @PropertySource&@ImportResource&@Bean

1. @**PropertySource**：加载指定的配置文件；

   ```java
   @PropertySource(value = {"classpath:person.properties"})
   @Component
   @ConfigurationProperties(prefix = "person")
   public class Person {
       ...
   }
   ```

2. @**ImportResource**：导入Spring的配置文件，让配置文件里面的内容生效；

   Spring Boot里面没有Spring的配置文件，自己编写的配置文件，也不能自动识别；

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
   
       <bean id="helloService" class="com.atguigu.springboot.service.HelloService"></bean>
   </beans>
   ```

   

   - 想让Spring的配置文件生效，加载进来；@**ImportResource**标注在一个配置类上

     ```java
     @ImportResource(locations = {"classpath:beans.xml"})
     导入Spring的配置文件让其生效
     ```

   - Spring推荐使用全注解的方式

     1、配置类**@Configuration**------>Spring配置文件

     2、使用**@Bean**给容器中添加组件

     ```java
     /**
      * @Configuration：指明当前类是一个配置类；就是来替代之前的Spring配置文件
      *
      * 在配置文件中用<bean><bean/>标签添加组件
      *
      */
     @Configuration
     public class MyAppConfig {
     
         //将方法的返回值添加到容器中；容器中这个组件默认的id就是方法名
         @Bean
         public HelloService helloService02(){
             System.out.println("配置类@Bean给容器中添加组件了...");
             return new HelloService();
         }
     }
     ```

     

## 4.8 配置文件占位符

1. 随机数

   ```java
   ${random.value}、${random.int}、${random.long}
   ${random.int(10)}、${random.int[1024,65536]}
   ```

   

2. 占位符获取之前配置的值，如果没有可以是用:指定默认值

   ```properties
   person.last-name=张三${random.uuid}
   person.age=${random.int}
   person.birth=2017/12/15
   person.boss=false
   person.maps.k1=v1
   person.maps.k2=14
   person.lists=a,b,c
   person.dog.name=${person.hello:hello}_dog
   person.dog.age=15
   ```

   

## 4.9 配置文件切换

1. 多Profile文件

   我们在主配置文件编写的时候，文件名可以是   application-{profile}.properties/yml

   默认使用application.properties的配置；

2. yml支持多文档块方式

```yml

server:
  port: 8081
spring:
  profiles:
    active: prod

---
server:
  port: 8083
spring:
  profiles: dev


---

server:
  port: 8084
spring:
  profiles: prod  #指定属于哪个环境
```

3. 激活指定profile

   ​	1.  在配置文件中指定  spring.profiles.active=dev

   ​	2. 命令行：

   ​		`java -jar spring-boot-02-config-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev；`

   ​		可以直接在测试的时候，配置传入命令行参数

   ​	3. 虚拟机参数；

   ​		`-Dspring.profiles.active=dev`

## 4.10 配置文件加载位置

springboot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件

- file:./config/

- file:./

- classpath:/config/

- classpath:/

优先级由高到底，高优先级的配置会覆盖低优先级的配置；

SpringBoot会从这四个位置全部加载主配置文件；**互补配置**；



==还可以通过spring.config.location来改变默认的配置文件位置==

**项目打包好以后，可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；指定配置文件和默认加载的这些配置文件共同起作用形成互补配置；**

`java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --spring.config.location=G:/application.properties`

## 4.11 外部配置加载顺序

**==SpringBoot也可以从以下位置加载配置； 优先级从高到低；高优先级的配置覆盖低优先级的配置，所有的配置会形成互补配置==**

**1.命令行参数**

所有的配置都可以在命令行上进行指定

java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --server.port=8087  --server.context-path=/abc

多个配置用空格分开； --配置项=值



**2.来自java:comp/env的JNDI属性**

**3.Java系统属性（System.getProperties()）**

**4.操作系统环境变量**

**5.RandomValuePropertySource配置的random.*属性值**



==**由jar包外向jar包内进行寻找；**==

==**优先加载带profile**==

**6.jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件**

**7.jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件**



==**再来加载不带profile**==

**8.jar包外部的application.properties或application.yml(不带spring.profile)配置文件**

**9.jar包内部的application.properties或application.yml(不带spring.profile)配置文件**



**10.@Configuration注解类上的@PropertySource**

**11.通过SpringApplication.setDefaultProperties指定的默认属性**

所有支持的配置加载来源:[参考官方文档](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#boot-features-external-config)

## 4.12 自动配置原理

### 1. 总结

[配置文件能配置的属性参照](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#common-application-properties)

1. SpringBoot启动会加载大量的自动配置类

2. 我们看我们需要的功能有没有SpringBoot默认写好的自动配置类；

3. 我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件有，我们就不需要再来配置了）

4. 给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们就可以在配置文件中指定这些属性的值；

**xxxxAutoConfigurartion：自动配置类；**

**给容器中添加组件**

**xxxxProperties:封装配置文件中相关属性；**



#### 2. 细节

**@Conditional派生注解（Spring注解版原生的@Conditional作用）**

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；

| @Conditional扩展注解            | 作用（判断是否满足当前指定条件）                 |
| ------------------------------- | ------------------------------------------------ |
| @ConditionalOnJava              | 系统的java版本是否符合要求                       |
| @ConditionalOnBean              | 容器中存在指定Bean；                             |
| @ConditionalOnMissingBean       | 容器中不存在指定Bean；                           |
| @ConditionalOnExpression        | 满足SpEL表达式指定                               |
| @ConditionalOnClass             | 系统中有指定的类                                 |
| @ConditionalOnMissingClass      | 系统中没有指定的类                               |
| @ConditionalOnSingleCandidate   | 容器中只有一个指定的Bean，或者这个Bean是首选Bean |
| @ConditionalOnProperty          | 系统中指定的属性是否有指定的值                   |
| @ConditionalOnResource          | 类路径下是否存在指定资源文件                     |
| @ConditionalOnWebApplication    | 当前是web环境                                    |
| @ConditionalOnNotWebApplication | 当前不是web环境                                  |
| @ConditionalOnJndi              | JNDI存在指定项                                   |

**自动配置类必须在一定的条件下才能生效；**

我们怎么知道哪些自动配置类生效；

**==可以通过启用  debug=true属性；来让控制台打印自动配置报告==**，即可知道哪些自动配置类生效；



# 5. 日志

## 5.1 日志框架介绍

**市面上的日志框架；**

JUL、JCL、Jboss-logging、logback、log4j、log4j2、slf4j....

| 日志门面  （日志的抽象层）                                   | 日志实现                                             |
| ------------------------------------------------------------ | ---------------------------------------------------- |
| ~~JCL（Jakarta  Commons Logging）~~    SLF4j（Simple  Logging Facade for Java）    **~~jboss-logging~~** | Log4j  JUL（java.util.logging）  Log4j2  **Logback** |

​	**SpringBoot选用 SLF4j和logback；**



## 5.2 SLF4j使用

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190610002836.png)

## 5.3 SpringBoot日志使用

1. **依赖**

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter</artifactId>
   </dependency>
   
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-logging</artifactId>
   </dependency>
   ```

   **==SpringBoot能自动适配所有的日志，而且底层使用slf4j+logback的方式记录日志，引入其他框架的时候，只需要把这个框架依赖的日志框架排除掉即可；==**

2. **使用**

   ```java
   	//记录器
   	Logger logger = LoggerFactory.getLogger(getClass());
   	@Test
   	public void contextLoads() {
   		//System.out.println();
   
   		//日志的级别；
   		//由低到高   trace<debug<info<warn<error
   		//可以调整输出的日志级别；日志就只会在这个级别以以后的高级别生效
   		logger.trace("这是trace日志...");
   		logger.debug("这是debug日志...");
   		//SpringBoot默认给我们使用的是info级别的，没有指定级别的就用SpringBoot默认规定的级别；root级别
   		logger.info("这是info日志...");
   		logger.warn("这是warn日志...");
   		logger.error("这是error日志...");
   
   
   	}
   ```

   ```
       日志输出格式：
   		%d表示日期时间，
   		%thread表示线程名，
   		%-5level：级别从左显示5个字符宽度
   		%logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
   		%msg：日志消息，
   		%n是换行符
       -->
       %d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n
   ```

   SpringBoot修改日志的默认配置

```properties
logging.level.com.atguigu=trace


#logging.path=
# 不指定路径在当前项目下生成springboot.log日志
# 可以指定完整的路径；
#logging.file=G:/springboot.log

# 在当前磁盘的根路径下创建spring文件夹和里面的log文件夹；使用 spring.log 作为默认文件
logging.path=/spring/log

#  在控制台输出的日志的格式
logging.pattern.console=%d{yyyy-MM-dd} [%thread] %-5level %logger{50} - %msg%n
# 指定文件中日志输出的格式
logging.pattern.file=%d{yyyy-MM-dd} === [%thread] === %-5level === %logger{50} ==== %msg%n
```

| logging.file | logging.path | Example  | Description                        |
| ------------ | ------------ | -------- | ---------------------------------- |
| (none)       | (none)       |          | 只在控制台输出                     |
| 指定文件名   | (none)       | my.log   | 输出日志到my.log文件               |
| (none)       | 指定目录     | /var/log | 输出到指定目录的 spring.log 文件中 |

3. **指定配置文件**

   给类路径下放上每个日志框架自己的配置文件即可；SpringBoot就不使用他默认配置的了

| Logging System          | Customization                                                |
| ----------------------- | ------------------------------------------------------------ |
| Logback                 | `logback-spring.xml`, `logback-spring.groovy`, `logback.xml` or `logback.groovy` |
| Log4j2                  | `log4j2-spring.xml` or `log4j2.xml`                          |
| JDK (Java Util Logging) | `logging.properties`                                         |

​	logback.xml：直接就被日志框架识别了；

​	**logback-spring.xml**：日志框架就不直接加载日志的配置项，由SpringBoot解析日志配置，可以使用SpringBoot的高级Profile功能

```xml
<springProfile name="staging">
    <!-- configuration to be enabled when the "staging" profile is active -->
  	可以指定某段配置只在某个环境下生效
</springProfile>
```

如：

```xml
<appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
        <!--
        日志输出格式：
			%d表示日期时间，
			%thread表示线程名，
			%-5level：级别从左显示5个字符宽度
			%logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
			%msg：日志消息，
			%n是换行符
        -->
        <layout class="ch.qos.logback.classic.PatternLayout">
            <springProfile name="dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ----> [%thread] ---> %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
            <springProfile name="!dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ==== [%thread] ==== %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
        </layout>
    </appender>
```



如果使用logback.xml作为日志配置文件，还要使用profile功能，会有以下错误

 `no applicable action for [springProfile]`

4. **切换日志框架**可以按照slf4j的日志适配图，进行相关的切换；

   slf4j+log4j的方式；

   ```xml
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-web</artifactId>
     <exclusions>
       <exclusion>
         <artifactId>logback-classic</artifactId>
         <groupId>ch.qos.logback</groupId>
       </exclusion>
       <exclusion>
         <artifactId>log4j-over-slf4j</artifactId>
         <groupId>org.slf4j</groupId>
       </exclusion>
     </exclusions>
   </dependency>
   
   <dependency>
     <groupId>org.slf4j</groupId>
     <artifactId>slf4j-log4j12</artifactId>
   </dependency>
   
   ```

   切换为log4j2

   ```xml
      <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
               <exclusions>
                   <exclusion>
                       <artifactId>spring-boot-starter-logging</artifactId>
                       <groupId>org.springframework.boot</groupId>
                   </exclusion>
               </exclusions>
           </dependency>
   
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-log4j2</artifactId>
   </dependency>
   ```





# 6. WEB开发

## 6.1 SpringBoot对静态资源的映射规则

1. 所有 /webjars/** ，都去 classpath:/META-INF/resources/webjars/ 找资源；

   webjars：以jar包的方式引入静态资源；

   http://www.webjars.org/

2.  "/**" 访问当前项目的任何资源，都去（静态资源的文件夹）找映射

   ```
   "classpath:/META-INF/resources/", 
   "classpath:/resources/",
   "classpath:/static/", 
   "classpath:/public/" 
   "/"：当前项目的根路径
   ```

3. 欢迎页； 静态资源文件夹下的所有index.html页面；被"/**"映射；

4. 所有的 **/favicon.ico  都是在静态资源文件下找；

## 6.2 模板引擎 th语法

![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190610120456.png)

JSP、Velocity、Freemarker、Thymeleaf

1.  引入thymeleaf；

   ```xml
   <dependency>
   	<groupId>org.springframework.boot</groupId>
   	<artifactId>spring-boot-starter-thymeleaf</artifactId>
   </dependency>
   ```

   HTML页面放在classpath:/templates/，thymeleaf就能自动渲染；

2. 导入thymeleaf的名称空间

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

3. 使用thymeleaf语法

```html
 <div th:text="${hello}">这是显示欢迎信息</div>
```

4. 语法规则

   - th:text；改变当前元素里面的文本内容；

   ​	![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190610204059.png)

5. 表达式

   ```properties
   Simple expressions:（表达式语法）
       Variable Expressions: ${...}：获取变量值；OGNL；
       		1）、获取对象的属性、调用方法
       		2）、使用内置的基本对象：
       			#ctx : the context object.
       			#vars: the context variables.
                   #locale : the context locale.
                   #request : (only in Web Contexts) the HttpServletRequest object.
                   #response : (only in Web Contexts) the HttpServletResponse object.
                   #session : (only in Web Contexts) the HttpSession object.
                   #servletContext : (only in Web Contexts) the ServletContext object.
                   
                   ${session.foo}
               3）、内置的一些工具对象：
               #execInfo : information about the template being processed.
               #messages : methods for obtaining externalized messages inside variables expressions, in the same way as they would be obtained using #{…} syntax.
               #uris : methods for escaping parts of URLs/URIs
               #conversions : methods for executing the configured conversion service (if any).
               #dates : methods for java.util.Date objects: formatting, component extraction, etc.
               #calendars : analogous to #dates , but for java.util.Calendar objects.
               #numbers : methods for formatting numeric objects.
               #strings : methods for String objects: contains, startsWith, prepending/appending, etc.
               #objects : methods for objects in general.
               #bools : methods for boolean evaluation.
               #arrays : methods for arrays.
               #lists : methods for lists.
               #sets : methods for sets.
               #maps : methods for maps.
               #aggregates : methods for creating aggregates on arrays or collections.
               #ids : methods for dealing with id attributes that might be repeated (for example, as a result of an iteration).
   
       Selection Variable Expressions: *{...}：选择表达式：和${}在功能上是一样；
       	补充：配合 th:object="${session.user}：
      <div th:object="${session.user}">
       <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
       <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
       <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
       </div>
       
       Message Expressions: #{...}：获取国际化内容
       Link URL Expressions: @{...}：定义URL；
       		@{/order/process(execId=${execId},execType='FAST')}
       Fragment Expressions: ~{...}：片段引用表达式
       		<div th:insert="~{commons :: main}">...</div>
       		
   Literals（字面量）
         Text literals: 'one text' , 'Another one!' ,…
         Number literals: 0 , 34 , 3.0 , 12.3 ,…
         Boolean literals: true , false
         Null literal: null
         Literal tokens: one , sometext , main ,…
   Text operations:（文本操作）
       String concatenation: +
       Literal substitutions: |The name is ${name}|
   Arithmetic operations:（数学运算）
       Binary operators: + , - , * , / , %
       Minus sign (unary operator): -
   Boolean operations:（布尔运算）
       Binary operators: and , or
       Boolean negation (unary operator): ! , not
   Comparisons and equality:（比较运算）
       Comparators: > , < , >= , <= ( gt , lt , ge , le )
       Equality operators: == , != ( eq , ne )
   Conditional operators:条件运算（三元运算符）
       If-then: (if) ? (then)
       If-then-else: (if) ? (then) : (else)
       Default: (value) ?: (defaultvalue)
   Special tokens:
       No-Operation: _ 
   ```

## 6.3 全面接管SpringMVC

SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己配置；所有的SpringMVC的自动配置都失效了

**我们需要在配置类中添加@EnableWebMvc即可；**

```java
//使用WebMvcConfigurerAdapter可以来扩展SpringMVC的功能
@EnableWebMvc
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
       // super.addViewControllers(registry);
        //浏览器发送 /atguigu 请求来到 templates下的 success 
        registry.addViewController("/atguigu").setViewName("success");
    }
    //所有的WebMvcConfigurerAdapter组件都会一起起作用
    @Bean //将组件注册在容器
    public WebMvcConfigurerAdapter webMvcConfigurerAdapter(){
        WebMvcConfigurerAdapter adapter = new WebMvcConfigurerAdapter() {
            @Override
            public void addViewControllers(ViewControllerRegistry registry) {
                registry.addViewController("/").setViewName("login");
                registry.addViewController("/index.html").setViewName("login");
            }
        };
        return adapter;
    }
}
```



## 6.4 如何修改SpringBoot的默认配置

模式：

​	1. SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（@Bean、@Component）如果有就用用户配置的，如果没有，才自动配置；如果有些组件可以有多个（ViewResolver）将用户配置的和自己默认的组合起来；

​	2. 在SpringBoot中会有非常多的xxxConfigurer帮助我们进行扩展配置

	3. 在SpringBoot中会有很多的xxxCustomizer帮助我们进行定制配置





## 6.5 国际化

1. 创建配置文件

![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190611011011.png)



2. application.properties

   ```properties
   spring.messages.basename=i18n.login
   ```

3. 取值

   ```html
   <label class="sr-only" th:text="#{login.password}">
       Password
   </label>
   
   <input type="password"placeholder="Password" th:placeholder="#{login.password}" >
   
   <div class="checkbox mb-3">
       <label>
           <input type="checkbox" value="remember-me"/> [[#{login.remember}]]
       </label>
   </div>
   ```

4. 可能出现中文乱码

   file-> Other Setting-> setting for nea projects

   ![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190611011819.png)

   

5. 按钮切换
   1. html

         ```html
         <a class="btn btn-sm" th:href="@{/index(l='zh_CN')}">中文</a>
         <a class="btn btn-sm" th:href="@{/index(l='en_US')}">English</a>
         ```
   
   2. 创建一个Java类
   
      ```java
      public class MyLocalResolver implements LocaleResolver {
          @Override
          public Locale resolveLocale(HttpServletRequest httpServletRequest) {
              String l = httpServletRequest.getParameter("l");
              Locale locale = Locale.getDefault();
              if(!StringUtils.isEmpty(l)){
                  String[] s = l.split("_");
                  locale = new Locale(s[0],s[1]);
              }
              return locale;
          }
      
          @Override
          public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {
      
          }
      }
      ```
      
   3. 在一个配置类中注入bean
   
         ```java
         @Configuration
         public class MyMvcConfig extends WebMvcConfigurerAdapter {
             @Override
             public void addViewControllers(ViewControllerRegistry registry) {
                 registry.addViewController("/").setViewName("login");
                 registry.addViewController("/test").setViewName("successs");
                 registry.addViewController("/index").setViewName("login");
             }
         
             @Bean
             public LocaleResolver localeResolver(){
                 return new MyLocalResolver();
             }
         
         }
         ```

## 6.6 拦截器

1. 创建拦截器类

   ```java
   public class MyLoginInterceptor implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           Object username = request.getSession().getAttribute("loginUser");
   
           if (null == username){
               request.setAttribute("msg","没有权限登录");
               request.getRequestDispatcher("/index").forward(request, response);
               return false;
           }else{
               return true;
           }
   
       }
   
       @Override
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
   
       }
   
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
   
       }
   }
   ```

2. 在配置类配置

   ```java
   @Configuration
   public class MyMvcConfig extends WebMvcConfigurerAdapter {
       /**
        * @Author chenpeng
        * @Description //TODO 登录拦截
        * @Date 2:24
        * @Param [registry]
        * @return void
        **/
       @Override
       public void addInterceptors(InterceptorRegistry registry) {
           registry.addInterceptor(new MyLoginInterceptor()).
                  addPathPatterns("/**").excludePathPatterns("/index","/","/user/login");
       }
   }
   ```

   

## 6.7  thymeleaf公共页面元素抽取

```properties
1、抽取公共片段
<div th:fragment="copy">
&copy; 2011 The Good Thymes Virtual Grocery
</div>

2、引入公共片段
<div th:insert="~{footer :: copy}"></div>
~{templatename::selector}：模板名::选择器
~{templatename::fragmentname}:模板名::片段名

3、默认效果：
insert的公共片段在div标签中
如果使用th:insert等属性进行引入，可以不用写~{}：
行内写法可以加上：[[~{}]];[(~{})]；
```

三种引入公共片段的th属性：

**th:insert**：将公共片段整个插入到声明引入的元素中

**th:replace**：将声明引入的元素替换为公共片段

**th:include**：将被引入的片段的内容包含进这个标签中

```html
<footer th:fragment="copy">
&copy; 2011 The Good Thymes Virtual Grocery
</footer>

引入方式
<div th:insert="footer :: copy"></div>
<div th:replace="footer :: copy"></div>
<div th:include="footer :: copy"></div>

效果
<div>
    <footer>
    &copy; 2011 The Good Thymes Virtual Grocery
    </footer>
</div>

<footer>
&copy; 2011 The Good Thymes Virtual Grocery
</footer>

<div>
&copy; 2011 The Good Thymes Virtual Grocery
</div>
```

demo:

```html
<nav class="col-md-2 d-none d-md-block bg-light sidebar" id="sidebar">
    <div class="sidebar-sticky">
        <ul class="nav flex-column">
            <li class="nav-item">
                <a class="nav-link active"
                   th:class="${activeUri=='main.html'?'nav-link active':'nav-link'}"
                   href="#" th:href="@{/main.html}">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-home">
                        <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path>
                        <polyline points="9 22 9 12 15 12 15 22"></polyline>
                    </svg>
                    Dashboard <span class="sr-only">(current)</span>
                </a>
            </li>

<!--引入侧边栏;传入参数-->
<div th:replace="commons/bar::#sidebar(activeUri='emps')"></div>
```

## 6.8 thymeleaf 总结

| code                                                         |                             |
| ------------------------------------------------------------ | --------------------------- |
| <html lang="en" xmlns:th="http://www.thymeleaf.org">         | 引入                        |
| th:href="@{/asserts/css/bootstrap.min.css}"                  | 资源定位                    |
| th:replace="commons/bar::topbar                              | 引入模板   模板位置::模板名 |
| th:replace="commons/bar::#sidebar(activeUri='emps')"         | 引入模板通过id并带参数      |
| th:fragment="topbar"                                         | 设置模板                    |
| th:class="${activeUri=='main.html'?'nav-link active':'nav-link'}" | 取模板中带的值              |
| th:each="emp:${emps}"                                        | foreach                     |
| [[${emp.lastName}]]                                          | 行中取值                    |
| th:text="${#dates.format(emp.birth, 'yyyy-MM-dd HH:mm')}"    | 日期转换                    |
| th:if="${not #strings.isEmpty(msg)}"                         | 选择是否展示                |
| #strings.isEmpty(msg)                                        | 判断是否为空                |
|                                                              |                             |

|      | 普通CRUD（uri来区分操作） | RestfulCRUD       |
| ---- | ------------------------- | ----------------- |
| 查询 | getEmp                    | emp---GET         |
| 添加 | addEmp?xxx                | emp---POST        |
| 修改 | updateEmp?id=xxx&xxx=xx   | emp/{id}---PUT    |
| 删除 | deleteEmp?id=1            | emp/{id}---DELETE |

```
forward：转发
redirect：从定向
<input type="hidden" name="_method" value="put" th:if="${emp!=null}"/>  配置put请求
<input type="hidden" name="_method" value="delete"/>					配置delete请求
```

```html
button 提交form表单
<button th:attr="del_uri=@{/emp/}+${emp.id}" class="btn btn-sm btn-danger deleteBtn">删除</button>
 

<form id="deleteEmpForm"  method="post">
    <input type="hidden" name="_method" value="delete"/>
</form>


<script>
    $(".deleteBtn").click(function(){
        //删除当前员工的
        $("#deleteEmpForm").attr("action",$(this).attr("del_uri")).submit();
        return false;
    });
</script>
```



## 6.9 错误处理

默认效果：

  1. 浏览器，返回一个默认的错误页面

  2. 其他客户端：默认响应一个json数据

     

### 1. 定制错误页面

**- 有模板引擎的情况下；error/状态码;** 【将错误页面命名为  错误状态码.html 放在模板引擎文件夹里面的 error文件夹下】，发生此状态码的错误就会来到  对应的页面；

​			我们可以使用4xx和5xx作为错误页面的文件名来匹配这种类型的所有错误，精确优先（优先寻找精确的状态码.html）；		

​			页面能获取的信息；

​				timestamp：时间戳

​				status：状态码

​				error：错误提示

​				exception：异常对象

​				message：异常消息

​				errors：JSR303数据校验的错误都在这里

**- 没有模板引擎（模板引擎找不到这个错误页面）**  静态资源文件夹下找；

**- 以上都没有错误页面**  就是默认来到SpringBoot默认的错误提示页面；



### 2. 定制错误JSON信息
**- 自定义异常处理&返回定制json数据**

```java
@ControllerAdvice
public class MyExceptionHandler {
    @ResponseBody
    @ExceptionHandler(UserNotExistException.class)
    public Map<String,Object> handleException(Exception e){
        Map<String,Object> map = new HashMap<>();
        map.put("code","user.notexist");
        map.put("message",e.getMessage());
        return map;
    }
}
//没有自适应效果...
//都是返回json数据
```



**- 转发到/error进行自适应响应效果处理**

```java
@ExceptionHandler(UserNotExistException.class)
public String handleException(Exception e, HttpServletRequest request){
    Map<String,Object> map = new HashMap<>();
    //传入我们自己的错误状态码  4xx 5xx，否则就不会进入定制错误页面的解析流程
    /**
             * Integer statusCode = (Integer) request
             .getAttribute("javax.servlet.error.status_code");
             */
    request.setAttribute("javax.servlet.error.status_code",500);
    map.put("code","user.notexist");
    map.put("message",e.getMessage());
    //转发到/error
    return "forward:/error";
}
```



**- 将我们的定制数据携带出去**

出现错误以后，会来到/error请求，会被BasicErrorController处理，响应出去可以获取的数据是由getErrorAttributes得到的（是AbstractErrorController（ErrorController）规定的方法）；

​	1、完全来编写一个ErrorController的实现类【或者是编写AbstractErrorController的子类】，放在容器中；

​	2、页面上能用的数据，或者是json返回能用的数据都是通过errorAttributes.getErrorAttributes得到；

​			容器中DefaultErrorAttributes.getErrorAttributes()；默认进行数据处理的；

自定义ErrorAttributes

```java
//给容器中加入我们自己定义的ErrorAttributes
@Component
public class MyErrorAttributes extends DefaultErrorAttributes {
    @Override
    public Map<String, Object> getErrorAttributes(RequestAttributes requestAttributes, boolean includeStackTrace) {
        Map<String, Object> map = super.getErrorAttributes(requestAttributes, includeStackTrace);
        map.put("company","atguigu");
        return map;
    }
}
//响应是自适应的，可以通过定制ErrorAttributes改变需要返回的内容，
```



## 6.10 配置 servlet容器

> SpringBoot 默认采用Tomcat作为嵌入式Servlet容器





## 6. 11 SpringBoot配置文件

| 属性                                | 解释                            |
| ----------------------------------- | ------------------------------- |
| server.servlet.context-path=/demo3  | 配置浏览器访问路径 （IP/demo3） |
| spring.messages.basename=i18n.login | 配置国际化位置                  |
| spring.thymeleaf.cache=false        | 关闭缓存                        |
| spring.mvc.date-format=yyyy-MM-dd   | 配置日期解析格式                |
| server.port=8081                    | 设置端口号                      |





# 7. 数据访问

## 7.1 原始使用

1. 依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-jdbc</artifactId>
   </dependency>
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <scope>runtime</scope>
   </dependency>
   ```

2. 配置文件

   ````properties
   spring.datasource.username=root
   spring.datasource.password=root
   spring.datasource.url=jdbc:mysql://192.169.1.170:3306/spring_boot_test?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
   spring.datasource.driver-class-name=com.mysql.jdbc.Driver
   
   # 配置数据库建表
   spring.datasource.schema=classpath:department.sql
   spring.datasource.initialization-mode=EMBEDDED
   #ALWAYS 总是
   #EMBEDDED 更改之后
   #NEVER 从不
   -------------------------------------------------
   schema-*.sql、data-*.sql
   默认规则：schema.sql，schema-all.sql；
   可以使用   
   	schema:
         - classpath:department.sql
         指定位置
   -------------------------------------------------
   ````

3. 测试

   ```java
   @Controller
   public class SpringTestController {
   
       @Autowired
       JdbcTemplate jdbcTemplate;
       /**
        * @Author chenpeng
        * @Description //TODO 使用原生的jdbc来访问数据库
        * @Date 22:16 
        * @Param []
        * @return java.lang.String
        **/
       @ResponseBody
       @GetMapping("/test")
       public Map<String, Object> test(){
           List<Map<String, Object>> maps = jdbcTemplate.queryForList("select * from department");
           return maps.get(0);
       }
   }
   
   ```

   **默认是用org.apache.tomcat.jdbc.pool.DataSource作为数据源；**

   

## 7.2 整合Druid数据源

1. 导入依赖

   ```xml
   <!--Druid数据源-->
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid-spring-boot-starter</artifactId>
       <version>1.1.10</version>
   </dependency>
   ```

2. 配置文件

   ```properties
   spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
   #连接池的设置
   #初始化时建立物理连接的个数
   spring.datasource.druid.initial-size=5
   #最小连接池数量
   spring.datasource.druid.min-idle=5
   #最大连接池数量 maxIdle已经不再使用
   spring.datasource.druid.max-active=20
   #获取连接时最大等待时间，单位毫秒
   spring.datasource.druid.max-wait=60000
   #申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效。
   spring.datasource.druid.test-while-idle=true
   #既作为检测的间隔时间又作为testWhileIdel执行的依据
   spring.datasource.druid.time-between-eviction-runs-millis=60000
   #销毁线程时检测当前连接的最后活动时间和当前时间差大于该值时，关闭当前连接
   spring.datasource.druid.min-evictable-idle-time-millis=30000
   #用来检测连接是否有效的sql 必须是一个查询语句
   #mysql中为 select 'x'
   #oracle中为 select 1 from dual
   spring.datasource.druid.validation-query=select 'x'
   #申请连接时会执行validationQuery检测连接是否有效,开启会降低性能,默认为true
   spring.datasource.druid.test-on-borrow=false
   #归还连接时会执行validationQuery检测连接是否有效,开启会降低性能,默认为true
   spring.datasource.druid.test-on-return=false
   #当数据库抛出不可恢复的异常时,抛弃该连接
   #spring.datasource.druid.exception-sorter=true  报错？
   #是否缓存preparedStatement,mysql5.5+建议开启
   #spring.datasource.druid.pool-prepared-statements=true
   #当值大于0时poolPreparedStatements会自动修改为true
   spring.datasource.druid.max-pool-prepared-statement-per-connection-size=20
   #配置扩展插件
   spring.datasource.druid.filters=stat,wall
   #通过connectProperties属性来打开mergeSql功能；慢SQL记录
   spring.datasource.druid.connection-properties=druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
   #合并多个DruidDataSource的监控数据
   spring.datasource.druid.use-global-data-source-stat=true
   #设置访问druid监控页的账号和密码,默认没有
   #spring.datasource.druid.stat-view-servlet.login-username=admin
   #spring.datasource.druid.stat-view-servlet.login-password=admin
   
   ```

3. 监控页面

   <http://localhost:8080/druid/>

   

## 7.3 整合MyBatis

1. 依赖

   ```xml
   <!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
   <dependency>
       <groupId>org.mybatis.spring.boot</groupId>
       <artifactId>mybatis-spring-boot-starter</artifactId>
       <version>2.0.1</version>
   </dependency>
   ```

2. 配置驼峰式转换

   ```properties
   mybatis.configuration.map-underscore-to-camel-case=true
   ```

3. demo

   1. **注解方式**

      1. bean

         ```java
         public class Department {
             private Integer id;
             private String departmentName;
             //get set
         }
         ```

      2. Mapper

         ```java
         @Mapper
         public interface SpringDemoMapper {
         
             @Select("SELECT * from department where id=#{id}")
             Department mGet(Integer id);
         
             @Delete("delete from department where id=#{id}")
             int deleteD(Integer id);
         
             @Options(useGeneratedKeys = true,keyProperty = "id")
             @Insert("insert into department(departmentName) values(#{departmentName})")
             int addD(String deprecated);
         
             @Update("update department set departmentName=#{departmentName} where id=#{id}")
             int updataD(Department department);
         }
         
         ```

         ```properties
         com.atguigu.springboot.mapper
         # 可以配置mapper的位置  不用每个都写@Mapper
         ```

         

   2. **配置文件版**

      1. 配置文件

         ```properties
         # mybaits配置
         # 指定全局配置文件的位置
mybatis.type-aliases-package=com.example.entity
         # 指定sql映射文件的位置
         mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
         ```
         
      2. mapper
      
         ```java
         @Mapper
         public interface SpringDemoMapperFile {
             Department gets(Integer id);
         
             int inserts(String departmentName);
         }
         ```
      
      3. xml
      
         ```xml
         <?xml version="1.0" encoding="UTF-8" ?>
         <!DOCTYPE mapper
                 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
         <mapper namespace="com.cp.springbootdemo04.mapper.SpringDemoMapperFile">
         
             <select id="gets" parameterType="Integer" resultType="com.cp.springbootdemo04.bean.Department">
                 SELECT * FROM department WHERE id=#{id}
             </select>
         
             <insert id="inserts" useGeneratedKeys="true" keyProperty="id" parameterType="String">
                insert into department(departmentName) values(#{departmentName})
             </insert>
         </mapper>
         
         ```

## 7.4 SpringData JPA

1. 依赖

   ```xml
   <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
   </dependency>
   ```

2. 配置

   ```properties
   # 更新或创建数据表结构
   spring.jpa.hibernate.ddl-auto=update
   
   # 控制台输出sql
   spring.jpa.show-sql=true
   ```

3. 创建一个entity （相当于数据库中的表）

   ```java
   @Entity
   @Table(name = "user")
   public class User {
       @Id//主键
       @GeneratedValue(strategy = GenerationType.IDENTITY)//自增
       private Integer id;
   
       @Column(name="username" ,length =50)//表中的信息
       private String name;
   
       @Column  //默认记为属性名
       private String email;
   	
       //get set
   }
   ```

4. 创建一个Dao接口来操作实体类对应的数据表

   ```java
   import com.cp.springbootdemo5jpa.entity.User;
   import org.springframework.data.jpa.repository.JpaRepository;
   
   /**
    * @author chenPeng
    * @version 1.0.0
    * @ClassName JpaRep.java
    * @Description TODO  user为表，Integer为主键,继承JpaRepository来完成对数据库的操作
    * @createTime 2019年06月13日 01:48:00
    */
   public interface JpaRep extends JpaRepository<User,Integer> {}
   
   ```

5. 即可直接操作

   ```java
   @RestController
   public class UserController {
       @Autowired
       private JpaRep jpaRep;
   
       @GetMapping("/user/{id}")
       public User getUser(@PathVariable("id")Integer id){
           return jpaRep.getOne(id);
       }
   
       @GetMapping("/add")
       public User insUser(User user){
           return jpaRep.save(user);
       }
   }
   
   ```

6. 其他

   [JPA 连表查询](https://www.cnblogs.com/hanjun0612/p/10343151.html)

   A表和B表

   ```java
   @Entity
   @Table(name = "A", schema = "kps", catalog = "kps")
   @DynamicUpdate
   public class A implements java.io.Serializable {
   
       private String aUUID;
       //关联B
       private B b;
   
       @OneToOne(fetch = FetchType.EAGER, optional = true)
       @NotFound(action = NotFoundAction.IGNORE)
       @JoinColumn(name = "aUUID", referencedColumnName = "aUUID", insertable = false, updatable = false)
       public B getB() {
       	return b;
       }
   
       public void setB(B b) {
       	this.b= b;
       }
   }
   
   --------------------------------------------------------------------------
   @Entity
   @Table(name = "B", schema = "kps", catalog = "kps")
   @DynamicUpdate
   public class B implements java.io.Serializable {
       private String bUUID;
       private Integer age;
   
   }
    
   ```

   JPA查询时

   ```java
   @Override
   public PageResult<A> pagedListForVaild(A entity, Integer currentPage, Integer pageSize,
   Order... orders) throws Exception {
       // 构建查询条件
       Specification<A> sf = new Specification<A>() {
           @Override
           public Predicate toPredicate(Root<A> root, CriteriaQuery<?> query, CriteriaBuilder cb) {
               List<Predicate> list = new ArrayList<Predicate>();
               Join<A, B> join = root.join("b", JoinType.INNER);
               list.add(cb.equal(join.<String>get("age"), Age));
               return cb.and(list.toArray(p));
           }
       };
   
       return super.gerPageResult(currentPage, pageSize, sf, orders);
   
   }
   ```

   文章

   <https://www.jianshu.com/nb/5480364>

   <https://www.jianshu.com/p/c427d3519f46>

