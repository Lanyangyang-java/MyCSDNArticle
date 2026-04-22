
@[TOC](Spring Cloud Eureka 服务注册与发现)

---

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/0e70051a2d5cbb2db2a9709685d15e0a.jpeg#pic_center =600x)

# 一、Eureka基础知识概述
## 1.Eureka两个核心组件
- **Eureka Server ：服务注册中心，主要用于提供服务注册功能。**
	当微服务启动时，会将自己的服务注册 到 Eureka Server。Eureka Server 维护了一个可用服务列表，存储了所有注册到 Eureka Server 的可用服务的信息，这些可用服务可以在 Eureka Server 的管理界面中直观看到
- **Eureka Client ：客户端，通常指的是微服务系统中各个微服务，主要用于和 Eureka Server 进行交互。**
	在微服务应用启动后，Eureka Client 会向 Eureka Server 发送心跳（默认周期为 30 秒）。若 Eureka Server 在多个心跳周期内没有接收到某个 Eureka Client 的心跳，Eureka Server 将它从可用服务列表中移除（默认 90 秒）

>**Eureka的心跳机制主要用于确保客户端（服务提供者）与服务器（服务注册中心）之间的连接活性。客户端启动后，会定期向服务器发送心跳数据，以告知服务器自己仍然处于活动状态**

---
## 2.Eureka 服务注册与发现
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/673a77b32f1c4db59aafd5c3bd16680c.png)


- **服务注册中心（Register Service）：** 它是一个 Eureka Server，用于提供服务注册和发现功能。
- **服务提供者（Provider Service）：** 它是一个 Eureka Client，用于提供服务。它将自己提供的服务注册到服务注册中心，以供服务消费者发现。
- **服务消费者（Consumer Service）：** 它是一个 Eureka Client，用于消费服务。它可以从服务注册中心获取服务列表，调用所需的服务。


**Eureka 实现服务注册与发现的流程**

1. 搭建一个Eureka Server作为服务注册中心
2. 服务提供者Eureka Client启动时，会把当前服务器的信息以服务名（spring.application.name）的方式注册到服务注册中心
3. 服务消费者Eureka Client启动时，也会向服务注册中心注册
4. 服务消费者还会获取一份可用路由服务列表，该列表中包含了所有注册到服务注册中心的服务信息（包括服务提供者和自身的信息）
5. 在获得了可用服务列表后，服务消费者通过 HTTP 或消息中间件远程调用服务提供者提供的服务。
6. 服务注册中心（Eureka Server）所扮演的角色十分重要，它是服务提供者和服务消费者之间的桥梁。服务提供者只有将自己的服务注册到服务注册中心才可能被服务消费者调用，而服务消费者也只有通过服务注册中心获取可用服务列表后，才能调用所需的服务。

---
# 二、Eureka单机搭建
1.**构建父模块和三个子模块 分别为注册中心 eureka-service 仓储模块stock-service 订单模块 order-service**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a882830144e945018b8aa38944ad74af.png)


---

2.**父模块pom.xml文件**


```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>cloud-02-Eureka-parent</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>cloud-eureka-service</module>
        <module>cloud-stock-service</module>
        <module>cloud-order-service</module>
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

3.**对cloud-eureka-service注册中心操作**
3.1 **pom.xml文件**
```xml
	<dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-eureka-server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
    </dependencies>
```

3.2 **主启动类**
```java
// 添加数据库和 druid 却未配置
// 则添加 exclude = {DataSourceAutoConfiguration.class, DruidDataSourceAutoConfigure.class}
// 则启动报错
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, DruidDataSourceAutoConfigure.class})
@EnableEurekaServer//告诉服务器我是一个注册中心
public class EurekaServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServiceApplication.class,args);
    }
}
```
3.3 **application.yml**
```yml
server:
  port: 8001
eureka:
  instance:
    hostname: 127.0.0.1  #eureka服务端的实例名字127.0.0.1 或 localhost
  client:
    #表识不向注册中心注册自己
    register-with-eureka: false
    #表示自己就是注册中心，职责是维护服务实例，并不需要去检索服务
    fetch-registry: false
    service-url:
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
      #服务注册位置 http://127.0.0.1:8001/eureka/
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```
---
4.**对仓储模块cloud-stock-service操作**
4.1 **pom.xml**
```xml
	<dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
    </dependencies>
```
4.2 **主启动类**

```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, DruidDataSourceAutoConfigure.class})
@EnableEurekaClient//需要使用Eureka注册中心添加此注解
public class StockServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(StockServiceApplication.class,args);
    }
}

```
4.3 **application.yml**

```yml
server:
  port: 9001
spring:
  application:
    name: cloud-stock-service
eureka:
  client:
    # 向服务端注册
    register-with-eureka: true
    # 需要检索
    fetchRegistry: true
    service-url:
      defaultZone: http://localhost:8001/eureka
```
4.4 StockController
```java
@RestController
@RequestMapping("/stock")
public class StockController {
    @GetMapping("/subStock")
    public String subStock(){
        System.out.println("库存减1");
        return "库存减1";
    }
}
```

---
5.**对订单模块cloud-order-service操作**
5.1 **pom.xml文件**

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
    </dependencies>
```

5.2 **主启动类**

```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, DruidDataSourceAutoConfigure.class})
@EnableEurekaClient
public class OrderServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class,args);
    }
}

```

5.3 **application.yml**

```yml
server:
  port: 7001
spring:
  application:
    name: cloud-order-servce
eureka:
  client:
    # 向服务端注册
    register-with-eureka: true
    # 需要检索
    fetchRegistry: true
    service-url:
      defaultZone: http://localhost:8001/eureka/
```

5.4 **OrderController**

```java
@RestController
@RequestMapping("/order")
public class OrderController {
	//这里特别提醒 `http://` 千万别忘加 本人因为这个错误找了很长时间 。。。。
	//CLOUD-STOCK-SERVICE 这里是eureka注册中心的名称
    private static final String HOST = "http://CLOUD-STOCK-SERVICE";

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("/addOrder")
    public String addOrder(){
        System.out.println("订单已完成");
        return restTemplate.getForObject(HOST + "/stock/subStock",String.class);
    }
}
```

5.5 **RestTemplate配置类**

```java
@Configuration
public class ApplicationConfig {
    @Bean
    @LoadBalanced//负载均衡器
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```
---
6.**启动设置**

 **我用的是idea 2021 ,多应用启动设置在如下位置**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2bd43466640a4041bcdf46f08ca77de6.png =800x)
**7.启动**
7.1 **先启动服务注册中心cloud-eureka-service  访问localhost:8001** 


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d7f3e3604a2f49649090a77628c39351.png)

7.2 **在以端口号为9001和 9002 分别启动 cloud-stock-service**
在yml文件 server.port更改 启动完成9001 更改端口号为9002 在启动一次 再次访问**localhost:8001**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/30a95d2d76b94af399959f1ae2406c36.png)
7.3 **在以端口号为7001和 7002 分别启动 cloud-order-service ，和 7.2更改方式一样 访问 localhost:8001**


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/91a38b0a8a6d47f0859da34afaaad66d.png)
8. **测试 ** 
8.1 **访问 localhost:7001/order/addOrder**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1dfef2990f824f0e9a9e21e391fbcd61.png)
8.2 **观察控制台，当我们多少刷新访问 localhost:7001/order/addOrder 则会出现9001和9002交替处理请求（轮循），缓解服务器压力，选择服务规则和 @LoadBalanced 负载均衡器有关**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/bfde9ecbbfba4bd39cf5320d67e4076d.png)

**试着访问localhost:7002,结果相同**


---
# 三、Eureka集群搭建
>在微服务架构中，一个系统往往由十几甚至几十个服务组成，若将这些服务全部注册到同一个 Eureka Server 中，就极有可能导致 Eureka Server 因不堪重负而崩溃，最终导致整个系统瘫痪。解决这个问题最直接的办法就是部署 Eureka Server 集群。

1.**新建cloud-eureka-server-2和cloud-eureka-server-3**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/16bfc5708f534c26b1068033d811f11c.png)
2.**修改映射配置**

打开 C:\Windows\System32\drivers\etc 目录下的hosts文件 修改映射配置添加进hosts文件
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c9a2564544334b1caa5e1b2e11462b45.png =800x)
3.**更改yml文件**
3.1 **cloud-eureka-service-2的yml文件**
```yml
server:
  port: 8002
eureka:
  instance:
    hostname: eureka8002.com  #eureka服务端的实例名字
  client:
    #表识不向注册中心注册自己
    register-with-eureka: false
    #表示自己就是注册中心，职责是维护服务实例，并不需要去检索服务
    fetch-registry: false
    service-url:
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
      defaultZone: http://eureka8003.com:8003/eureka/
```
---
3.2 **cloud-eureka-service-2的yml文件**

```yml
server:
  port: 8003
eureka:
  instance:
    hostname: eureka8003.com  #eureka服务端的实例名字
  client:
    #表识不向注册中心注册自己
    register-with-eureka: false
    #表示自己就是注册中心，职责是维护服务实例，并不需要去检索服务
    fetch-registry: false
    service-url:
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
      defaultZone: http://eureka8002.com:8002/eureka/
```
3.3 **cloud-order-service的yml文件**

```yml
server:
  port: 7001
spring:
  application:
    name: cloud-order-servce
eureka:
  client:
    # 向服务端注册
    register-with-eureka: true
    # 需要检索
    fetchRegistry: true
    service-url:
      defaultZone: http://localhost:8002/eureka,http://localhost:8003/eureka
```
----
3.3 **cloud-stock-service的yml文件**

```yml
server:
  port: 9001
spring:
  application:
    name: cloud-stock-service
eureka:
  client:
    # 向服务端注册
    register-with-eureka: true
    # 需要检索
    fetchRegistry: true
    service-url:
      defaultZone: http://localhost:8002/eureka,http://localhost:8003/eureka

```
---
4.**分别为cloud-eureka-service-2和cloud-eureka-service-3添加启动类**
```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, DruidDataSourceAutoConfigure.class})
@EnableEurekaServer//告诉服务器我是一个注册中心
public class EurekaServiceApplication2 {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServiceApplication2.class,args);
    }
}
-----------------------------------------------------------------------------------------------------------------

@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, DruidDataSourceAutoConfigure.class})
@EnableEurekaServer//告诉服务器我是一个注册中心
public class EurekaServiceApplication3 {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServiceApplication3.class,args);
    }
}
```
---
5.**启动**
5.1 **启动cloud-eureka-service-2和cloud-eureka-service-3**
5.1.1 **访问localhost:8002**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/9794a4c935294b44a503bfdb8ea1899b.png =800x)
5.1.2 **访问localhost:8003**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ddbd652f71d44fe195c9b47b58eaef5b.png =800x)

---

5.2 **启动cloud-order-service和 端口号为9001和9002的cloud-stock-service集群服**

5.2.1 **访问localhost:8002**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a7530345b04b420c8e2092528db907c5.png =800x)


5.2.2 **访问localhost:8003**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4ed6c2c06d98456e98d61851c722fbd2.png =800x)

---
6.**测试 多次访问localhost:7001 观察控制台**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/dd61a11a596d43ea89ea6060ca96b81c.png)

---
**以上方式可以形成一组互相注册的 Eureka Server 集群，当服务提供者发送注册请求到 Eureka Server 时，Eureka Server 会将请求转发给集群中所有与之相连的 Eureka Server 上，以实现 Eureka Server 之间的服务同步。**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5a45c1adf3564490a7f9ffe88ed8da00.png)


**通过服务同步，服务消费者可以在集群中的任意一台 Eureka Server 上获取服务提供者提供的服务。这样，即使集群中的某个服务注册中心发生故障，服务消费者仍然可以从集群中的其他 Eureka Server 中获取服务信息并调用，而不会导致系统的整体瘫痪，这就是 Eureka Server 集群的高可用性。**

---



# 四、心跳续约
>**“心跳”指的是一段定时发送的自定义信息，让对方知道自己“存活”，以确保连接的有效性。大部分 CS 架构的应用程序都采用了心跳机制，服务端和客户端都可以发心跳。通常情况下是客户端向服务器端发送心跳包，服务端用于判断客户端是否在线**

>**心跳续约是指服务实例（Eureka客户端）定期向Eureka服务器发送心跳包，以证明其仍然在线并愿意继续提供服务。Eureka服务器会根据这些心跳包来更新服务实例的活跃状态，并维护一个可用的服务实例列表。**

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/061fd50b8a78f75a0614d65d24467034.webp?x-oss-process=image/format,png#pic_center)
**功能介绍**

Spring Cloud A 和 Spring Cloud B可以看作两个服务的提供者 
Spring Cloud C 相当于 服务的消费者 定时（默认30秒）获取服务列表
registry服务注册列表 里面存储的是 各个注册服务的 名称 ip 端口号
readWriteCacheMap 会实时同步registry注册列表中的数据
readOnlyCacheMap 默认每30秒去同步一次readWriterCacheMap对象中的数据

---

**详细流程**

- A和B 服务 向Eureka Service 服务注册列表中注册服务 将自己的服务名称 ip + 端口号注册到 registry服务注册列表。
- A和B每隔30秒 (默认30秒) 发送 一次心跳任务告诉Eureka 我还活着， registry服务注册列表 同步数据到readWriteCacheMap
- C 服务 通过readOnlyCacheMap 每30秒拉去一次注册表数据，这个数据不是同步的而是30秒（默认30秒）更新一次，这也是有时我们会在UI界面上看到服务注册成功，调用时却出现错误的原因。同理，服务下线也存在同样的问题，服务已经下线了，但是还是有客户端在调用已经下线的服务，这时就会出现连接拒绝的错误。
- 我们在Eureka UI页面上看到的注册信息，实际上并没有走readOnlyCacheMap，而是直接通过registry服务注册列表获取，所以我们能够在Eureka UI页面实时的看到注册的新服务。
- 服务续约默认是30秒 ，定时请理60秒清理一次超过90秒未续约的服务，也就是说在连续3次丢失心跳后会被Eureka Server的evict线程清理，最极端的情况，服务下线后，可能需要延迟180s之后，Eureka Server中的registry对象才会被更新。

综上分析，在非手动清除的情况下，缓存需要180秒才能感知下线的服务，这种情况在生产环境中非常严重。

---

上述问题中，我们可以更改默认值，来解决感知下线服务时间过长问题

**Eureka注册中心**
```yml
eureka:
  server:
    #清理无效服务间隔 (默认60秒)
    eviction-interval-timer-in-ms: 1000
    #同步readWrite到readOnly间隔(默认30秒)
    response-cache-update-interval-ms: 10000
    #Client直接从readWriteCacheMap更新服
    #use-read-only-response-cache: false
```

**服务提供者**
```yml
eureka:
  instance:
    # Eureka注册中心（服务端）在收到客户端心跳之后，
    #等待下一次心跳的超时时间，如果在这个时间内没有收到下次心跳，则移除该客户端。（默认90秒）
    lease-expiration-duration-in-seconds: 5
    # 客户端向注册中心发送心跳的时间间隔（默认30秒）
    lease-renewal-interval-in-seconds: 2
```

# 五、Eureka自我保护机制

>**Eureka的自我保护机制是一种应对网络异常的安全保护措施，宁可同时保留所有微服务（健康的服务和不健康的服务都会保留）也不盲目移除任何健康的服务。**
>

当我们在本地调试基于 Eureka 的程序时，Eureka 服务注册中心很有可能会出现红色警告。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/16900e88b2de45d5a477d7038cbefeb7.png)

- 实际上，这个警告是触发了 Eureka 的自我保护机制而出现的。默认情况下，如果 Eureka Server 在一段时间内没有接收到某个服务提供者的心跳，就会将这个服务提供者提供的服务从服务注册表中移除。 这样服务消费者就再也无法从服务注册中心中获取到这个服务了，更无法调用该服务。

- 但在实际的分布式微服务系统中，健康的服务也有可能会由于网络故障（例如网络延迟、卡顿等原因）而无法与 Eureka Server正常通讯。若此时 Eureka Server因为没有接收心跳而误将健康的服务从服务列表中移除，这显然是不合理的。而 Eureka 的自我保护机制就是来解决此问题的。

- 所谓 “Eureka 的自我保护机制”，其中心思想就是“好死不如赖活着”。如果 Eureka Server 在一段时间内没有接收到 Eureka Client 的心跳，那么 Eureka Server 就会开启自我保护模式，将所有的 Eureka Client 的注册信息保护起来，而不是直接从服务注册表中移除。一旦网络恢复，这些 Eureka Client 提供的服务还可以继续被服务消费者消费。

---
默认情况下，Eureka 的自我保护机制是开启的，如果想要关闭，则需要在配置文件中添加以下配置
```yml
eureka:
  server:
  	# false 关闭 Eureka 的自我保护机制，默认是开启
    enable-self-preservation: false 
```
---
**需要注意的是：**

**Eureka 的自我保护机制也存在弊端。如果在 Eureka 自我保护机制触发期间，服务提供者提供的服务出现问题，那么服务消费者就很容易获取到已经不存在的服务进而出现调用失败的情况。此时，我们可以通过客户端的容错机制来解决此问题**

