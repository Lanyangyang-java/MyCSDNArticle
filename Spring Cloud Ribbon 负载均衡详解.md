@[TOC](Spring Cloud Ribbon 负载均衡)

---
>**Spring Cloud Ribbon 是一套基于 Netflix Ribbon 实现的客户端负载均衡工具。** 

>**本文样例 均使用[Sping Cloud Eureka 笔记中的Eureka集群样例](https://blog.csdn.net/qq_60972641/article/details/143643150?spm=1001.2014.3001.5501) 进行更改使用**
# 一.负载均衡概述
**负载均衡(Load Balance): 将用户的请求通过使用各种算法来确定分配请求的最佳方式，将这些请求分配到多个服务器上运行，从而防止任何一个资源过载或失效而导致应用程序的性能下降或停止响应。**


**在任何一个系统中，负载均衡都是一个十分重要且不得不去实施的内容，它是系统`处理高并发`，`缓解网络压力`和`服务端扩容`的重要手段之一**

---
# 二、服务端负载均衡

**工作原理**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/23638bcd258449de9ceead4479eb1243.png)


**服务端负载均衡是在客户端和服务端之间建立一个独立的负载均衡服务器，该服务器既可以是硬件设备，也可以是软件（例如 Nginx）。这个负载均衡服务器维护了一份可用服务端清单，然后通过心跳机制来删除故障的服务端节点，以保证清单中的所有服务节点都是可以正常访问的。**

**当客户端发送请求时，该请求不会直接发送到服务端进行处理，而是全部交给负载均衡服务器，由负载均衡服务器按照某种算法（轮询、随机等），从其维护的可用服务清单中选择一个服务端，然后进行转发。**


**特点:**
- 需要建立一个独立的负载均衡服务器。
- 负载均衡是在客户端发送请求后进行的，因此客户端并不知道到底是哪个服务端提供的服务。
- 可用服务端清单存储在负载均衡服务器上。
---
# 三、客户端负载均衡
**工作原理**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/42589664063a4900aa464de56c4fff2f.png)

**客户端负载均衡是将负载均衡逻辑以代码的形式封装到客户端上，负载均衡器位于客户端。客户端通过服务注册中心，获取到一份服务端提供的可用服务列表。获取服务列表后，负载均衡器会在客户端发送请求前通过负载均衡算法选择一个服务端实例再进行访问，以达到负载均衡的目的**

**客户端负载均衡也需要心跳机制去维护服务端清单的有效性，这个过程需要配合服务注册中心一起完成**

**特点：**
- 负载均衡器位于客户端，不需要单独搭建一个负载均衡服务器。
- 负载均衡是在客户端发送请求前进行的，因此客户端清楚地知道是哪个服务端提供的服务。
- 客户端都维护了一份可用服务清单，而这份清单都是从服务注册中心获取的。

---
# 四、负载均衡策略

---
## 负载均衡策略简介
**Ribbon的负载均衡规则是一个叫做IRule的接口来定义的，每一个子接口都是一种规则**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d2c4adde29724d49af71f0aa3bbf595b.png)

实现类	|负载均衡策略
-|-
RoundRobinRule|按照线性轮询策略，即按照一定的顺序依次选取服务实例
RandomRule|随机选取一个服务实例
RetryRule|按照 RoundRobinRule（轮询）的策略来获取服务，如果获取的服务实例为 null 或已经失效，则在指定的时间之内不断地进行重试（重试时获取服务的策略还是 RoundRobinRule 中定义的策略），如果超过指定时间依然没获取到服务实例则返回 null 。
WeightedResponseTimeRule|WeightedResponseTimeRule 是 RoundRobinRule 的一个子类，它对 RoundRobinRule 的功能进行了扩展。<br><br>根据平均响应时间，来计算所有服务实例的权重，响应时间越短的服务实例权重越高，被选中的概率越大。刚启动时，如果统计信息不足，则使用线性轮询策略，等信息足够时，再切换到 WeightedResponseTimeRule。
BestAvailableRule|	继承自 ClientConfigEnabledRoundRobinRule。先过滤点故障或失效的服务实例，然后再选择并发量最小的服务实例。
AvailabilityFilteringRule|	先过滤掉故障或失效的服务实例，然后再选择并发量较小的服务实例。
ZoneAvoidanceRule|	默认的负载均衡策略，综合判断服务所在区域（zone）的性能和服务（server）的可用性，来选择服务实例。在没有区域的环境下，该策略与轮询（RandomRule）策略类似。



---
## 切换负载均衡策略

**Ribbon默认使用轮询策略选取服务。**

将默认的轮询负载均衡切换为 随机负载均衡
还是使用[Spring Cloud Eureka](https://blog.csdn.net/qq_60972641/article/details/143643150?spm=1001.2014.3001.5501)实例 更改cloud-order-service中的 ApplicationConfig 

```java
@Configuration
public class ApplicationConfig {

    @Bean
    @LoadBalanced//负载均衡器
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }

    @Bean
    public IRule iRule(){
       return new RandomRule();
    }
}
```
**重启cloud-order-service服务** **多次访问 localhost:7001/order/addorder** 

观察控制台 我们发现每次访问 不在轮询访问 9001和9002 而是随机访问

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/65e3508b22c9448f85c1ff8e059d223b.png)

---
## 自定义负载均衡策略
有时候 我们所需的负载均衡策略 Ribbon未能提供 那么我们可以自定义 负载均衡策略
1.**在cloud-order-service 下创建一个规则 每个服务访问五次更换下一个服务 实现`AbstractLoadBalancerRule`**

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
2.更改ApplicationConfig
```java
@Configuration
public class ApplicationConfig {

    @Bean
    @LoadBalanced//负载均衡器
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }

    @Bean
    public IRule iRule(){
       return new FiveTimesRule();
    }
}
```

3.启动类添加`@RibbonClients`注解

```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, DruidDataSourceAutoConfigure.class})
@EnableEurekaClient
@RibbonClients({
		//自定义 Ribbon 负载均衡策略在主启动类上使用 RibbonClient 注解，在该微服务启动时，就能自动去加载我们自定义的 Ribbon 配置类，从而是配置生效
		// name 为需要定制负载均衡策略的微服务名称（application name）
		// configuration 为定制的负载均衡策略的配置类，
		//// 且官方文档中明确提出，该配置类不能在 ComponentScan 注解（SpringBootApplication 注解中包含了该注解）下的包或其子包中
        @RibbonClient(value = "CLOUD-PAYMENT-SERVICE",configuration = ApplicationConfig.class),
})
public class OrderServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class,args);
    }
}
```
4.我们再更改一下cloud-stock-service模块的controller，

```java
@RestController
@RequestMapping("/stock")
public class StockController {

    private SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

    @GetMapping("/subStock")
    public String subStock(){
        System.out.println(sdf.format(new Date()) + "9001=>库存减1");
        return sdf.format(new Date()) + "=>9001=>库存减1";
    }
}
```

5.**集群启动两个仓储模块 启动一个订单模块 将Eureka2和Eureka3启动 多次访问localhost:7001/order/addOrder**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c6c4a59829354f7f8a64082e6518b572.png)

---
# 五、负载均衡算法
名称|描述
-|-
轮询法|轮询法是挨个轮询服务器处理，也可以设置权重。
随机法|没有配置权重的话，所有的服务器被访问到的概率都是相同的。如果配置权重的话，权重越高的服务器被访问的概率就越大
两次随机法|在随机法的基础上多增加了一次随机，多选出一个服务器。随后再根据两台服务器的负载等情况，从其中选择出一个最合适的服务器
哈希法|将请求的参数信息通过哈希函数转换成一个哈希值，然后根据哈希值来决定请求被哪一台服务器处理
一致性 Hash 法|让相同参数的请求总是发到同一台服务器处理。不过，它解决了哈希法存在的一些问题
最小连接法|当有新的请求出现时，遍历服务器节点列表并选取其中连接数最小的一台服务器来响应当前请求
最少活跃法|最少活跃法以活动连接数为标准，活动连接数可以理解为当前正在处理的请求数。活跃数越低，说明处理能力越强，这样就可以使处理能力强的服务器处理更多请求
最快响应时间法|客户端会维持每个服务器的响应时间，每次请求挑选响应时间最短的
