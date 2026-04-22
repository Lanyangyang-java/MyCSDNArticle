@[TOC](Spring Cloud OpenFeign 声明式服务调用与负载均衡组件)

----


# 一、OpenFeign基础简介


**官方Spring Cloud OpenFeign 中文文档：[https://springdoc.cn/spring-cloud-openfeign/#netflix-feign-starter](https://springdoc.cn/spring-cloud-openfeign/#netflix-feign-starter)**

**Feign 对 Ribbon 进行了集成，OpenFeign 是 Spring Cloud 对 Feign 的二次封装，具有 Feign 的所有功能并在 Feign 的基础上增加了对 Spring MVC 注解的支持。**

**应用于微服务架构中的服务调用，它可以帮助开发人员简化服务调用，提高开发效率，并提供一些常用的功能来保证服务的可靠性和安全性（通过集成Spring Security）。**

---

**特点：**
- 是 Spring Cloud 下的远程调用和负载均衡组件
- 以实现服务的远程调用和负载均衡
- 对 Ribbon 进行了集成，都利用 Ribbon 维护了可用服务清单，并通过 Ribbon 实现了客户端的负载均衡。
- 在客户端定义服务绑定接口并通过注解的方式进行配置，以实现远程服务的调用。
- 支持JAX-RS注解和SpringMVC注解

---
# 二、OpenFeign 常用注解
注解|作用说明
-|-
@FeignClient| 用于指定要调用的服务名称，并可以配置服务的URL、负载均衡策略等信息。
@EnableFeignClients| 该注解用于开启 OpenFeign 功能，当 Spring Cloud 应用启动时，<br>OpenFeign 会扫描标有 @FeignClient 注解的接口，生成代理并注册到 Spring 容器中。
@RequestMapping|用于定义请求的URL、HTTP方法、请求参数等信息。
@GetMapping|用来映射 GET 请求
@PostMapping|用来映射 POST 请求


---
# 三、OpenFeign实现服务远程调用
**1.创建maven父模块，及其子模块eureka集群模块1和2 子模块order 子模块 stock**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/42a0968666c94242a2b19d85c95101dc.png)

---
**2.给父模块pom.xml添加依赖（添加公用的依赖）**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.chq</groupId>
    <artifactId>cloud-03-OpenFegin-parent</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>

        <module>cloud-OpenFegin-eureka-service-1</module>
        <module>cloud-OpenFegin-eureka-service-2</module>
        <module>cloud-OpenFegin-order-service</module>
        <module>cloud-OpenFegin-stock-service</module>
    </modules>

    <!-- 统一管理jar包版本 -->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <junit.version>4.12</junit.version>
        <log4j.version>1.2.17</log4j.version>
        <lombok.version>1.18.22</lombok.version>
        <mysql.version>8.0.24</mysql.version>
        <druid.version>1.2.8</druid.version>
        <mybatis-plus.version>3.0.7.1</mybatis-plus.version>
    </properties>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.2.8</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-jdbc -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>

        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>${mybatis-plus.version}</version>
            <optional>true</optional>
            <exclusions>
                <exclusion>
                    <groupId>com.baomidou</groupId>
                    <artifactId>mybatis-plus-generator</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
    <!-- 子模块继承之后，提供作用：锁定版本+子modlue不用写groupId和version  -->
    <dependencyManagement>
        <dependencies>
            <!--spring boot 2.2.2-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.2.2.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud Hoxton.SR1-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <addResources>true</addResources>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

---
**3.分别对Eureka1和2进相互注册（也可以进行Eureka单机运行此样例）**

**3.1 pom.xml**
```xml
<dependencies>
        <!--Eureka Server 依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
    </dependencies>
```
**3.2 启动类 另一个类更改类名** 

```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, DruidDataSourceAutoConfigure.class})
@EnableEurekaServer
public class OpenFeginEurekaApplication1 {
    public static void main(String[] args) {
        SpringApplication.run(OpenFeginEurekaApplication1.class,args);
    }
}
```
**3.3 编写application.yml文件 进行相互注册 这里就写一个 另一个详细可以看 [Spring Cloud Eureka 服务注册与发现](https://blog.csdn.net/qq_60972641/article/details/143643150?spm=1001.2014.3001.5501)**
```java
server:
  port: 8001
eureka:
  instance:
    hostname: eureka8001.com  #eureka服务端的实例名字
  client:
    #表识不向注册中心注册自己
    register-with-eureka: false
    #表示自己就是注册中心，职责是维护服务实例，并不需要去检索服务
    fetch-registry: false
    service-url:
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
      defaultZone: http://eureka8002.com:8002/eureka/
```
**4.对stock模块进行操作**

**4.1pom.xml文件**

```xml
	<dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
    </dependencies> 
```
**4.2 编写application.yml文件 此处注意 更改端口号集群启动时 instance-id: stock9002也得更改**

```yml
server:
  port: 9002
spring:
  application:
  	#这里最好和Eureka页面应用名称相对于
    name: CLOUD-OPENFEGIN-STOCK-SERVICE
eureka:
  instance:
    instance-id: stock9002
    # Eureka注册中心（服务端）在收到客户端心跳之后，等待下一次心跳的超时时间，如果在这个时间内没有收到下次心跳，则移除该客户端。（默认90秒）
    lease-expiration-duration-in-seconds: 5
    # 客户端向注册中心发送心跳的时间间隔（默认30秒）
    lease-renewal-interval-in-seconds: 2
  client:
    # 向服务端注册
    register-with-eureka: true
    # 需要检索
    fetchRegistry: true
    service-url:
      defaultZone: http://eureka8001.com:8001/eureka, http://eureka8002.com:8002/eureka
    #迅速获取服务注册状态 默认30s
    registry-fetch-interval-seconds: 1
```

**4.3 创建启动类**
```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, DruidDataSourceAutoConfigure.class})
@EnableEurekaServer
public class OpenFeginEurekaApplication1 {
    public static void main(String[] args) {
        SpringApplication.run(OpenFeginEurekaApplication1.class,args);
    }
}
```

**4.4 创建StockController(这里我们让页面更好看 更改之前的代码) 注意这里打开：可多应用启动 [Spring Cloud Eureka详细介绍](https://blog.csdn.net/qq_60972641/article/details/143643150?spm=1001.2014.3001.5501)**
```java
@RestController
@RequestMapping("/stock")
public class StockController {

    private Integer count = 0 ;
	
	//更改端口号 集群启动时 自动获取
    @Value("${server.port}")
    public Integer port;

    @GetMapping("/subStock/{seconds}")
    public R subStock(@PathVariable("seconds") Integer seconds){
    //seconds这里我们一会测试超时使用
        try{
            System.out.println("睡眠"+seconds+"秒");
            TimeUnit.SECONDS.sleep(seconds);
        }catch (Exception e){
            e.printStackTrace();
        }
        HashMap<String, Object> map = new HashMap<>();
        map.put("port",port);
        map.put("count",++count);
        return R.ok(map);
    }
}
```

---

**5.对Order操作（OpenFegin重点）**

**5.1 pom.xml文件**
```xml
	<dependencies>
        <!--添加 OpenFeign 依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>

        <!--Eureka Client 依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
    </dependencies>
```
**5.2 负载均衡规则 rule 还是按照之前写的 每五次更换一次服务**

```java
public class FiveTimesRule extends AbstractLoadBalancerRule {
    //定义访问次数
    private Integer count = 0;
    //定义一个索引位置
    private Integer index = 0;

    //不需要重写 加载配置
    @Override
    public void initWithNiwsConfig(IClientConfig iClientConfig) {

    }

    @Override
    public Server choose(Object key) {
        //通过父类的方法getLoadBalancer 获取负载均衡器
        ILoadBalancer loadBalancer = this.getLoadBalancer();
        //获取所有的服务 获取的是无序的
        List<Server> servers = loadBalancer.getAllServers();
        //排序 升序
        List<Server> result = servers.stream().sorted(Comparator.comparing(server -> server.getPort())).collect(Collectors.toList());

        if (result!=null && result.size()!=0){
            count++;
            if (count <= 5){
                return result.get(index);
            }else {
                index++;
                count = 1;
                //防止outOfBoundsIndex
                if (index >= result.size()){
                    index = 0;
                }
                return result.get(index);
            }
        }
        return null;
    }
}

```

**5.3 application.yml**
```yml
server:
  port: 7001
spring:
  application:
    name: CLOUD-OPENFEGIN-ORDER-SERVICE
eureka:
  instance:
    instance-id: order7001
    # Eureka注册中心（服务端）在收到客户端心跳之后，等待下一次心跳的超时时间，如果在这个时间内没有收到下次心跳，则移除该客户端。（默认90秒）
    lease-expiration-duration-in-seconds: 5
    # 客户端向注册中心发送心跳的时间间隔（默认30秒）
    lease-renewal-interval-in-seconds: 2
  client:
    # 向服务端注册
    register-with-eureka: true
    # 需要检索
    fetchRegistry: true
    service-url:
      defaultZone: http://eureka8001.com:8001/eureka, http://eureka8002.com:8002/eureka
    registry-fetch-interval-seconds: 1

CLOUD-OPENFEGIN-STOCK-SERVICE:
  ribbon:
    ServerListRefreshInterval: 5000  #每五秒拉取一下服务列表
    NFLoadBalancerRuleClassName: com.chq.csdn.rule.FiveTimesRule
```
**5.4 启动类**
```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, DruidDataSourceAutoConfigure.class})
@EnableEurekaClient
@EnableFeignClients
public class OpenFeignOrderApplication {
    public static void main(String[] args) {
        SpringApplication.run(OpenFeignOrderApplication.class,args);
    }
}
```
**5.5 远程调用StockApi接口**

```java
@Component
@FeignClient("CLOUD-OPENFEGIN-STOCK-SERVICE")
public interface StockApi {
    /*
     * ip + port  => ribbon => localhost:9001
     * localhost:9001/stock/subStock
     * 远程调用
     * */
    @GetMapping("/stock/subStock/{seconds}")
    R subStock(@PathVariable("seconds") Integer seconds);
}
```

**5.6 编写OrderController**

```java
@RestController
@RequestMapping("/order")
public class OrderController{

    @Autowired
    private StockApi stockApi;

    @GetMapping("/addOrder/{seconds}")
    public R addOrder(@PathVariable("seconds") Integer seconds){
        return stockApi.subStock(seconds);
    }
}

```

---
**6.启动 启动Eureka集群 order模块 stock9001 和 9002 模块**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6081bcd766404b67a8e8fb8a114f66da.png)

---
**7.测试 多次此时符合我们预期结果 这里默认 休眠时间为0，下文我们会说OpenFeign超时控制问题**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8bccd6769cbf4d0491894fe3bf2af121.png)

---
# 四、OpenFeign 超时控制

**当一个微服务调用另一个微服务时，如果在设定的时间内未得到响应，则OpenFeign会自动停止请求的操作，这就是超时控制。**

**OpenFeign客户端的默认超时时间为 1 秒钟，如果服务端处理请求的时间超过 1 秒就会报错。为了避免这样的情况，我们需要对 OpenFeign 客户端的超时时间进行控制。**


---
**我们访问localhost:7001/order/addOrder/1**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/da16d4cddd224c92913e8eec3c99bf31.png)
**控制台报错**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/27d731c90c9f4461a7bc542d3d09a3ef.png)

---
**超时控制的作用**
- 防止服务雪崩
- 提升用户体验
- 优化资源利用


---
**调整超时时间 向 cloud-OpenFegin-order-service模块的yml文件中添加  ReadTimeout 和 ConnectTimeout**
```yml
CLOUD-OPENFEGIN-STOCK-SERVICE:
  ribbon:
    #指的是建立连接后从服务器读取到可用资源所用的时间
    ReadTimeout: 6000
    #指的是建立连接所用时间，适用于网络状态正常的情况下，两端连接所用的时间
    ConnectTimeout: 6000
    ServerListRefreshInterval: 5000  #每五秒拉取一下服务列表
    NFLoadBalancerRuleClassName: com.chq.csdn.rule.FiveTimesRule
```

此时访问页面localhost:7001/order/addOrder/1 就可以正常访问
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/38c7f9c62ce34301baa5bb9f3284df59.png)

**一但超过6秒未能获取数据 则还会出现以上 超时的错误**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a54b61b948c0424b877dbce03d43b041.png =600x)

---
# 五、OpenFeign日志增强
**OpenFeign 提供了日志打印功能，Feign 为每一个 FeignClient 都提供了一个 feign.Logger 实例，通过它可以对 OpenFeign 服务绑定接口的调用情况进行监控。**

---
**日志级别**
- NONE：默认级别，不显示任何日志。
- BASIC：记录请求方法、URL、响应状态码及执行时间。
- HEADERS：除了BASIC中定义的信息之外，还包括请求和响应的头信息。
- FULL：除了HEADERS中定义的信息之外，还包括请求和响应的正文及元数据。

**使用OpenFeign 日志打印功能**
在order模块的yml文件添加配置
```yml
logging:
  level:
    # feign 日志以什么级别监控哪个接口 
    com.chq.csdn.api.StockApi: debug
```
**测试 观察order模块控制台打印日志信息**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f2db876e8fda4f9aae41b63b82d41980.png)

**创建建FeignConfig类**

```java
@Configuration
public class FeignConfig {
    /**
     * OpenFeign 日志增强
     * 配置 OpenFeign 记录哪些内容
     */
    @Bean
    Logger.Level feignLoggerLevel(){
        return Logger.Level.FULL;
    }
}
```
**再次观察order模块打印的日志信息 此时我们的日志信息 就很清晰明了**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8f064fe65b1f4e9ba39e17b647357e15.png) 

---
**注意事项**
- **日志级别冲突：OpenFeign日志增强打印级别可能和Log4j的logback.xml中打印到控制台的打印级别冲突，导致不打印增强日志。解决方案是统一两者打印级别，至少保证logback向下兼容OpenFeign的打印级别。**
- **性能影响：开启高级别的日志（如FULL级别）可能会对性能产生一定影响，因为需要记录更多的信息。因此，在生产环境中，应根据实际需求选择合适的日志级别。**
