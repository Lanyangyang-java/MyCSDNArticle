@[TOC](SpringBoot框架学习总结 及 整合 JDBC Mybatis-plus JPA Redis 我的学习笔记)

---

# 一、SpringBoot概述
**SpringBoot是由Pivotal团队提供的开源框架，它并不是对Spring功能上的增强，而是提供了一种快速使用Spring的方式。通过提供默认配置和丰富的组件封装，SpringBoot简化了配置，使得开发者能够更快地构建基于Spring的应用程序。**

**其设计目的是用来简化Spring应用的初始搭建以及开发过程**



![Alt](https://img-blog.csdnimg.cn/img_convert/3cc84855edff4919576fb38579aca217.jpeg =900x)


---
**Spring Boot的主要优点**
 - 内嵌服务器使得SpringBoot应用可以独立运行
 - 内嵌式容器简化Web项目
 - 开箱即用提供各种默认配置来简化项目配置
 - 没有冗余代码生成和XML配置的要求
# 二、创建SpringBoot程序 
## 1. 使用maven方式创构建
**（1）创建Maven项目**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8652641941aa4b47986a34578d035696.png =600x)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f8339eec085645a9bd31357900192322.png =600x)

**（2）在pom.xml中添加Spring Boot相关依赖**
```xml
<!-- 引入Spring Boot父依赖 -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.3.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```
**（3）编写主程序启动类**
```java
//标记该类为主程序启动类
@SpringBootApplication
public class HelloSpringBoot {
    public static void main(String[] args) {
        //方法启动主程序类 
        // SpringApplication.run(HelloSpringBoot.class,args)
        //代表运行SpringBoot的启动类，参数为SpringBoot启动类的字节码对象
        SpringApplication.run(HelloSpringBoot.class,args);
    }
}
```
**（4）创建一个用于Web访问的Controller**
```java
@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String hello(){
        return "Hello SpringBoot!";
    }
}
```
**（5）运行项目 访问 localhost：8080/hello**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/01b71a0e4f334d8abe8d3a0ce8fa274f.png =600x)

---
## 2. 使用Spring Initializr构建

**(1) 创建新模块，选择Spring初始化，并配置模块相关基础信息**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/9a5b6906b2e746538efa8250f2a7a99f.png =600x)

**(2) 选择当前模块需要使用的技术集**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/50dd0b52f5184970a30e699125b54ef2.png =600x)
**(3) Controller**
```java
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String hello(){
        return "Hello SpringBoot!";
    }
}
```

**(4) 运行自动生成的Application类**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/442f05f259cd405392f2354b3a007166.png)


## 3. SpringBoot热部署
**热部署允许开发人员在代码修改后无需手动重启应用程序。这大大缩短了开发和调试周期，只需保存文件，系统就会自动重新加载相关的类和资源，使得每次代码修改都能立即生效。**
```xml
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
```

**使用 双击 或者 Ctrl + F9**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1bd9f576b3514f72b1e927a9cb4ffb55.png)

## 4. SpringBoot的跨域处理
**跨域 ：当一个请求url的协议，域名，端口三个种任意一个与当前页面url不一样就是跨域**
**SpringBoot常用的跨域有两个**

**(1) 重写 WebMvcConfigurer(全局跨域)**
- 实现 WebMvcConfigurer 接口中的 addCorsMappings 方法，在此方法中配置全局跨域处理。
- 在工程中添加 WebMvcConfig 类。此类配置了 @Configuration 注解，工程启动时会自动加载此类中的配置。

```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
       /**
		* addMapping：配置可以被跨域的路径，可以任意配置，可以具体到直接请求路径。 
		* allowCredentials：是否开启Cookie
		* allowedMethods：允许的请求方式，如：POST、GET、PUT、DELETE等。
		* allowedOrigins：允许访问的url，可以固定单条或者多条内容
		* allowedHeaders：允许的请求header，可以自定义设置任意请求头信息。 
		* maxAge：配置预检请求的有效时间
		*/

		registry.addMapping("/**")
                //是否发送Cookie
                .allowCredentials(true)
                //放行哪些原始域
                .allowedOrigins("*")
                .allowedMethods(new String[]{"GET", "POST", "PUT", "DELETE"})
                .allowedHeaders("*")
                .maxAge(36000);
    }
}
```
**(2) 使用注解 (局部跨域)**
- 使用 @CrossOrigin 注解配置某一个 Controller 允许跨域。
```java
@RestController
@CrossOrigin(origins = "*")
public class HelloController {
    @RequestMapping("/hello")
    public String hello() {
        return "hello world";
    }
}
```
*[HTML]:  ## 5. @RequestBody实现参数序列化

# 三、基础配置
## 1.配置文件的作用
- **灵活性和可配置性：通过配置文件，可以轻松更改应用程序的行为和属性，而无需重新编译和部署代码。**
- **避免硬编码：将应用程序的配置信息从代码中分离出来，使得代码更加清晰、可维护和可重用。通过修改配置文件而不是代码，可以更容易地进行配置调整和部署管理**。
- **多环境支持：可以针对不同的运行环境（如开发环境、测试环境、生产环境）使用不同的配置文件，以适应不同环境下的需求。这样可以在不同环境下灵活切换配置，减少了手动修改的工作量。**

---
## 2.配置文件格式
**SpringBoot提供了多种属性配置方式**
**application.properties文件 ：**  
- 语法结构 ：key=value
- 使用.properties作为文件扩展名。
- 是Spring Boot项目默认的配置文件格式。
- 使用#注释信息


**application.yml ：**
- 语法结构 ：key：空格 value
- 采用缩进和冒号的结构化格式，其中key和value之间使用英文冒号和空格组成，空格不可省略
- 使用.yml或.yaml作为文件扩展名。
- 结构更加清晰易读
- 支持更多的数据类型

---


>SpringBoot配置文件加载顺序  **application.properties > application.yml > application.yaml**

**application.properties**
```
server.port=8080
```
**application.yml**
```yml
server:
  port: 8081
```

**application.yaml**
```yaml
server:
  port: 8082
```
**运行SpringBoot工程 分别使用# 注释各个配置文件 观察端口号**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/001987a3aec34cfb846ac443d1b8ce20.png)

## 2.yaml
**YAML（YAML Ain't Markup Language），一种数据序列化格式**
**优点：**
- 容易阅读
- 容易与脚本语言交互
- 以数据为核心，重数据轻格式

**YAML文件扩展名**
- yml（主流）
- yam

>**这种语言以数据作为中心，而不是以标记语言为重点！重数据轻格式，以前的配置文件，大多数都是使用xml来配置**

---
 **yaml语法规则**
 >**核心规则：数据前面要加空格与冒号隔开**
 - 大小写敏感
- 属性层级关系使用多行描述，每行结尾使用冒号结束
- 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格（不允许使用Tab键）
- 属性值前面添加空格（属性名与属性值之间使用冒号+空格作为分隔）
- #表示注释

---

**yaml数组数据**
> **数组数据在数据书写位置的下方使用减号作为数据开始符号，每行书写一个数据，减号与数据间空格分隔**

**数组（ List、set ）**
```yml
#数组
pets:
  - cat
  - dog
  - pig

# 或 
pets: [cat,dog,pig]
```

**对象、Map（键值对）**
```yml
dog:
    name: "中华田园犬"
    age: 3
    
# 或
dog: {name: "中华田园犬",age: 3}
```
---

**注入配置文件** 
```java
//注册bean到容器中
@Component
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Dog {
    @Value("${dog.name}")
    private String name;
    @Value("${dog.age}")
    private Integer age;
}
```

**测试**
```java
@SpringBootTest
class SpringBootInitApplicationTests {
    @Autowired //将狗狗自动注入进来
    private Dog dog;
    @Test
    public void contextLoads() {
        System.out.println(dog); //打印看下狗狗对象
    }
}
```

---

## 3.SpringBoot配置信息的查询
> **以查阅SpringBoot的官方文档文档URL： [https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#common-application-properties](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#common-application-properties)**

```java
# QUARTZ SCHEDULER (QuartzProperties)
spring.quartz.jdbc.initialize-schema=embedded # Database schema initialization mode.
spring.quartz.jdbc.schema=classpath:org/quartz/impl/jdbcjobstore/tables_@@platform@@.sql # Path to the SQL file to use to initialize the database schema.
spring.quartz.job-store-type=memory # Quartz job store type.
spring.quartz.properties.*= # Additional Quartz Scheduler properties.
# ----------------------------------------
# WEB PROPERTIES
# ----------------------------------------
# EMBEDDED SERVER CONFIGURATION (ServerProperties)
server.port=8080 # Server HTTP port.
server.servlet.context-path= # Context path of the application.
server.servlet.path=/ # Path of the main dispatcher servlet.
# HTTP encoding (HttpEncodingProperties)
spring.http.encoding.charset=UTF-8 # Charset of HTTP requests and responses. Added to the "Content-Type" header if not set explicitly.
# JACKSON (JacksonProperties)
spring.jackson.date-format= # Date format string or a fully-qualified date format class name. For instance, `yyyy-MM-dd HH:mm:ss`.
# SPRING MVC (WebMvcProperties)
spring.mvc.servlet.load-on-startup=-1 # Load on startup priority of the dispatcher servlet.
spring.mvc.static-path-pattern=/** # Path pattern used for static resources.
spring.mvc.view.prefix= # Spring MVC view prefix.
spring.mvc.view.suffix= # Spring MVC view suffix.
# DATASOURCE (DataSourceAutoConfiguration & DataSourceProperties)
spring.datasource.driver-class-name= # Fully qualified name of the JDBC driver. Auto-detected based on the URL by default.
spring.datasource.password= # Login password of the database.
spring.datasource.url= # JDBC URL of the database.
spring.datasource.username= # Login username of the database.
```


# 四、SpringBoot注解
## 1.核心注解
 **@SpringBootApplication**
- 标注一个主程序类，表明这是一个Spring Boot应用程序的入口。它是一个复合注解，组合了
@Configuration、@EnableAutoConfiguration 和 @ComponentScan。
- 直接标注在应用程序的主类上。

 **@Configuration**
- 标识一个类作为配置类，类似于Spring XML配置文件。
- 在配置类上添加此注解，通常与@Bean注解配合使用，用于定义Bean和配置信息


**@EnableAutoConfiguration**
- 启用Spring Boot的自动配置机制，根据项目中的依赖和应用上下文自动配置Spring应用程序。
- 在启动类或配置类上添加此注解。@SpringBootApplication注解已经组合了这个注解，通常不需要在启动类再添加此注解。

 **@ComponentScan**
- 自动扫描并加载符合条件的组件或Bean，定义扫描的路径。
- 在配置类上添加此注解，并指定扫描的包范围。通常与@SpringBootApplication一起使用，无需单独添加。但也可以根据需要单独使用，并设置扫描的过滤条件。


---
## 2.Web注解
 **@Controller**
- 标记一个类作为控制器，用于处理HTTP请求并返回视图。
- 放在控制器类上，与@RequestMapping一起使用，方法的返回值通常是视图的名称或ModelAndView对象。

 **@RestController**
- 结合了@Controller和@ResponseBody两个注解的功能，用于标记一个类或者方法，表示该类或方法用于处理HTTP请求，并将响应的结果直接返回给客户端，而不需要进行视图渲染。
- 通常放在控制器类上，配合@RequestMapping使用。

 **@RequestMapping**
- 用于映射web请求（如URL路径）到具体的方法上。可以标注在类上或方法上，标注在类上时，表示类中的所有响应请求的方法都是以该类路径为父路径。
- 可以通过设置value、path、method、params、headers等属性来指定请求的URL、HTTP方法、请求参数等。

 **@GetMapping**
-  映射HTTP GET请求到处理方法上。


 **@PostMapping**
-  映射HTTP POST请求到处理方法上。


---
## 3.组件注解

 **@Component**
- 将一个类标识为Spring组件（Bean），可以被Spring容器自动检测和注册。
@Component是一个通用的注解，可以用来标注任何Spring管理的Bean。


 **@Service**
- 标识服务层组件，实际上是@Component的一个特化，用于表示业务逻辑服务。

 **@Repository**
- 标识持久层组件，实际上是@Component的一个特化，用于表示数据访问组件。

---
## 4.依赖注入注解
 
 **@Autowired**
- 用于自动装配（自动注入）Spring Bean到其他Bean中。
- 可以标注在属性、构造器或方法上，Spring会在运行时自动注入匹配的Bean。

 **@Value**
- 注入配置文件中的属性值

---
## 5.其他常用注解

**@ResponseBody**
- 将方法的返回值转换为指定格式（如JSON、XML）作为HTTP响应的内容返回给客户端。
- 常用于RESTful服务中，标识方法返回的对象不是视图名称，而是直接写入HTTP响应体中的数据

**@RequestBody**
- 将HTTP请求体的内容（如JSON、XML等）映射到一个Java对象。
- 通常用于POST请求中，将客户端发送的数据绑定到方法的参数上。

**@PathVariable**
- 从URI路径中提取参数值，将其映射到方法的参数上。
- 常用于RESTful服务中，允许动态地将URL中的部分作为方法参数使用。

**@Import**
- 用来导入其他配置类。

**@ImportResource**
- 用来加载xml配置文件。

# 五、整合第三方技术
## 1.Springboot集成JDBC
**(1) 创建SpringBoot工程**
按照上面的过程创建 不选着任何技术集
**(2) pom.xml依赖**
```xml
		<!--web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--jdbc-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>

        <!--mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
```
**(3) application.yml**
```yml
spring:
    datasource:
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://localhost:3306/test?useSSL=false&characterEncoding=utf8&serverTimezone=Asia/Shanghai
        password: 1234
        username: root
```
**(4) 编写JDBCController**
```java
@RestController
public class JDBCController {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    //查询数据库的所有信息
    //没有实体类，数据库的数据   怎么获取？ Map
    @GetMapping("/StudentList")
    public List<Map<String,Object>> StudentList(){
        String sql = "select * from student";
        List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
        return maps;
    }
}
```
**(5) 测试页面展示**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/abfa78f739e04ba9a8d90eb985642a4c.png)

---
## 2.SpringBoot集成JPA
**(1) 创建SpringBoot工程**
按照上面的过程创建 不选着任何技术集
**(2) pom.xml依赖**
```xml
<!-- SpringDataJPA依赖 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <!-- MySQL驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
```
**(3) application.yml**
```yml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/test?useSSL=false&characterEncoding=utf8&serverTimezone=Asia/Shanghai
    password: 1234
    username: root

  jpa:
    hibernate:
      ddl-auto: update # 第一次建表create  后面用update，要不然每次重启都会新建表
    #在控制台显示Hibernate的sql
    show-sql: true
```
**(4) 创建一个entity**
```java
@Data
@Entity //声明实体类
@AllArgsConstructor
@NoArgsConstructor
public class User {
    @Id //表示该属性作为表的的主键
    @Generated
    private Long id;
    private String name;
    private String gender;
}
```
**(5) 创建UserRepository**
```java
public interface UserRepository extends JpaRepository<User,Long> {
	/**
     * 我们在这里直接继承JpaRepository
     * 这里面有很多的方法
     * */
}
```
**这里可以打开JpaRepository源码 里面提供了很多方法**

![请添加图片描述](https://i-blog.csdnimg.cn/direct/abbe2324f1714356aaa884634cb9454c.png =600x)


**(6) 创建Controller**
```java
@RestController
public class UserController {
    @Autowired
    private UserRepository userRepository;

    @GetMapping("/findAll")
    public List<User> findAll(){
        List<User> userList = userRepository.findAll();
        return userList;
    }
}
```
**(7) 测试运行**
我们发现JPA已经给我们自动生成了一张User表
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ebc3f824337f4aebbcd1ea2a005dd88e.png)

**(8) 给新建表添加数据**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d1f3f1273846401883e0b95e93859cf7.png)

**(9) 访问Controller的findAll**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/3150e6724da14ef79d71c0dad8db43bd.png =600x)

---
## 3.SpringBoot集成Mybatis-Plus
**在我的另一篇文章 ：[MyBatis与Mybatis-plus的学习总结 及 两者的区别 我的学习笔记](https://blog.csdn.net/qq_60972641/article/details/143271487?spm=1001.2014.3001.5501) 中 Mybatis-plus 的 快速入门有详细介绍**

---
## 4.SpringBoot集成Redis
**(1) 创建SpringBoot工程**
按照上面的过程创建 不选着任何技术集

**(2) pom.xml依赖**
```xml
		<!--redis-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
```

**(3) application.yml**
```yml
spring:
  redis:
    host: 127.0.0.1  #redis主机ip
    port: 6379       #端口号
    password: 1234   #有密码就填写密码 没有可以省
```
**(4) 填写redis配置类**
```java
@Configuration   //redis配置类
public class RedisConfig {
    //固定的模板   直接拿去用即可
    @Bean
    @SuppressWarnings("all")
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<String, Object>();
        template.setConnectionFactory(factory);
        //json序列化配置
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        //String的序列化
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
        // key采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);
        // hash的key也采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        // value序列化方式采用jackson
        template.setValueSerializer(jackson2JsonRedisSerializer);
        // hash的value序列化方式采用jackson
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();

        return template;
    }
}
```
**(5) 填写RedisController**
```java
@RestController
public class RedisController {
    @Autowired
    private RedisTemplate redisTemplate;

    @GetMapping("/setName")
    public String SetName(){
    	//设置存活name存活时间 为 1 单位TimeUnit.MINUTES 为分钟
        redisTemplate.opsForValue().set("name","蔡徐坤",1, TimeUnit.MINUTES);
        String name = (String)redisTemplate.opsForValue().get("name");
        return name;
    }
}
```

**(6) 运行测试**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2378bfd21c96431ca5f7c9b40e811264.png =600x)

**(7) 打开Redis图形化工具 发现值存在**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e68076e08491468dbe4dea6307ef6f9c.png)

**(8) 一分钟后发现值不存在**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/00dc05411e7847b6b10c7cd8a02e406b.png)



