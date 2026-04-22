@[TOC](Spring Cloud Hystrix 豪猪哥 服务容错与保护组件)



---
# 一、Spring Cloud Hystrix概述
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ee36565535d745968ad7f7b3cf461d36.png)

## 1.简介
**Spring Cloud Hystrix是一个用于处理分布式系统延迟和容错的库，它提供了熔断机制、服务降级、服务隔离等关键特性，以提高分布式系统的弹性和可用性**

**在微服务系统中，Hystrix 能够帮助我们实现以下目标：**
- 保护线程资源：防止单个服务的故障耗尽系统中的所有线程资源。
- 快速失败机制：当某个服务发生了故障，不让服务调用方一直等待，而是直接返回请求失败。
- 提供降级（FallBack）方案：在请求失败后，提供一个设计好的降级方案，通常是一个兜底方法，当请求失
- 败后即调用该方法。
- 防止故障扩散：使用熔断机制，防止故障扩散到其他服务。
- 监控功能：提供熔断器故障监控组件 Hystrix Dashboard，随时监控熔断器的状态。
## 2.核心功能
- **服务降级:** 在请求失败后，提供一个设计好的降级方案，通常是一个兜底方法，当请求失败后即调用该方法。
- **服务熔断:** 当服务调用失败率达到一定阈值时，Hystrix会自动熔断该服务的调用，阻止进一步的请求，从而避免雪崩效应
- **请求合并:** 在一个时间节点，访问同一个节点，等几秒，将相同节点的请求合并去请求同一个节点
- **请求缓存:** 可以缓存相同请求的响应结果，从而避免重复调用远程服务
- **隔离:** 隔离分为线程池隔离和信号量隔离。通过判断线程池或信号量是否已满，超出容量的请求直接降级，从而
达到限流的作用。
## 3.雪崩效应
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0d75ebd9ec86471dac9ada9b946e440b.png)
> **在所有服务都处于可用状态时，请求 1 需要调用服务1，2，3完成，请求 2 需要调用服务4，5，2，6完成，请求 3 需要调用服务 2，3完成。**

**当服务2发生故障或网络延迟：**
- 即使其他所有服务都可用，由于服务2的不可用，那么用户请求 1、2、3 都会处于阻塞状态，等待服务2的响应。在高并发的场景下，会导致整个服务器的线程资源在短时间内迅速消耗殆尽。
- 所有依赖于服务2的其他服务，服务1，5也都会处于线程阻塞状态，等待服务2的响应，导致这些服务的不可用。
- 所有依赖服务5的服务，服务5处于线程阻塞状态，导致服务4不可用。
- 最终导致雪崩效应，然后微服务架构引入了熔断器系列服务容错和保护机制。

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/eb66d7f2ca166dc9251dbe2ea2740132.jpeg =500x)

**雪崩原因**
- 服务提供者不可用: 硬件故障，程序BUG,并发请求过大，缓存击穿等。
- 不断的请求重试，代码重试等。
- 无服务调用者不可用:同步请求造成了资源耗尽。

**雪崩效应最终的结果就是:服务链条中的某一个服务不可用,导致一系列的服务不可用,最终造成服务逻辑
崩溃。这种问题造成的后果，往往是无法预料的。**
                                                                                                                                                                        
# 二、Hystrix服务降级
## 1.服务降级简介
**服务降级是指，当请求超时、资源不足等情况发生时进行服务降级处理，不调用真实服务逻辑，而是使用
快速失败(fallback)方式直接返回一个托底数据，保证服务链条的完整，避免服务雪崩。**

**服务降级常用两种情况：**
- **服务器压力剧增：**
	根据实际业务情况及流量，对一些不重要、不紧急的服务进行有策略地不处理或简单处
理，从而释放服务器资源以保证核心服务正常运作。
- **某些服务不可用：**
	当某些服务不可用时，为了避免长时间等待造成服务卡顿或雪崩效应，而主动执行备用的降级逻辑立刻返回一
个友好的提示，以保障主体业务不受影响。

## 2.样例准备
使用 我的另一篇文章 [Spring Cloud OpenFeign 声明式服务调用与负载均衡组件 文章中 OpenFeign实现服务远程调用](https://blog.csdn.net/qq_60972641/article/details/143721489?spm=1001.2014.3001.5502) 的样例 实现负载均衡

## 3.服务提供者降级
对于stock模块
**1.更改启动类 添加注解 @EnableCircuitBreaker**
```java
@EnableEurekaClient
@EnableCircuitBreaker //激活服务降级 服务提供者使用
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, DruidDataSourceAutoConfigure.class})
public class OpenFeignStockApplication {
    public static void main(String[] args) {
        SpringApplication.run(OpenFeignStockApplication.class,args);
    }
}
```
**2.填写兜底方法 及使用兜底方法 对与StockController进行更改**
```java
@RestController
@RequestMapping("/stock")
public class StockController {

    private Integer count = 0 ;

    @Value("${server.port}")
    public Integer port;

    @GetMapping("/subStock/{seconds}")
    //该注解走该节点方法 发生异常 走降级方法
    @HystrixCommand(fallbackMethod = "subStockFallBack",
            commandProperties = {
                //在6秒钟还没有返回 就执行兜底方法
                //execution.isolation.thread.timeoutInMilliseconds这个为官方文档的默认值 
                //value默认为1000 即是超时1秒走兜底方法 subStockFallBack
                @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "6000")
    })
    public R subStock(@PathVariable("seconds") Integer seconds){
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

    //降级方法 参数用不用都要写
    public R subStockFallBack(Integer seconds){
        return R.failed("subStock 的兜底方法");
    }
}
```
**3.测试** 

**Tip:** 如果我们order 设置的openfegin的超时时间小于6秒那么我们适当调大一些 避免直接走openfegin的超时异常而非我们的降级方法 更改order模块的yml文件

```yml
CLOUD-OPENFEGIN-STOCK-SERVICE:
  ribbon:
  	#将ReadTimeout和ConnectTimeout调大一些方便测试
    #指的是建立连接后从服务器读取到可用资源所用的时间
    ReadTimeout: 20000
    #指的是建立连接所用时间，适用于网络状态正常的情况下，两端连接所用的时间
    ConnectTimeout: 20000
    ServerListRefreshInterval: 5000  #每五秒拉取一下服务列表
    NFLoadBalancerRuleClassName: com.chq.csdn.rule.FiveTimesRule

```
**访问localhost:7001/order/addOrder/6**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/74e1e8e18fc84775b52f6b1b08e3c77d.png)
我们可以发现 超时不在报异常了 而是直接走了我们的降级方法


## 4.服务消费者降级
**1.更改启动类**

```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, DruidDataSourceAutoConfigure.class})
@EnableEurekaClient
@EnableFeignClients
@EnableHystrix //服务消费方 使用这个注解
public class OpenFeignOrderApplication {
    public static void main(String[] args) {
        SpringApplication.run(OpenFeignOrderApplication.class,args);
    }
}
```
**2.更改OrderController**
```java
@RestController
@RequestMapping("/order")
@DefaultProperties(defaultFallback = "globalFallBack")
public class OrderController{

    @Autowired
    private StockApi stockApi;

    @GetMapping("/addOrder/{seconds}")
    @HystrixCommand(fallbackMethod = "addOrderFallBack",
            commandProperties = {
                    //在6秒钟还没有返回 就执行兜底方法
                    @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "6000")
            })
    public R addOrder(@PathVariable("seconds") Integer seconds){
        return stockApi.subStock(seconds);
    }

    public R addOrderFallBack(Integer seconds){
        return R.failed("订单系统繁忙,请稍后再试");
    }
}
```
**3.测试 重启order 访问localhost:7001/order/addOrder/6**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/095d37b5d1b54517970d4db2b2166daf.png)
这里我们可以得出结论 当服务消费者和服务提供者同时返回降级方法时，会走服务消费者的降级方法

## 5.全局降级方法
**1.对于服务消费者order我们更改OrderController**
```java
@RestController
@RequestMapping("/order")
@DefaultProperties(defaultFallback = "globalFallBack") //全局默认兜底方法 使用globalFallBack方法
public class OrderController{

    @Autowired
    private StockApi stockApi;

    @GetMapping("/addOrder/{seconds}")
    @HystrixCommand(fallbackMethod = "addOrderFallBack",
            commandProperties = {
                    //在6秒钟还没有返回 就执行兜底方法
                    @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "6000")
            })
    public R addOrder(@PathVariable("seconds") Integer seconds){
        return stockApi.subStock(seconds);
    }

	//这里我们再写一个 
    @GetMapping("/insertOrder/{seconds}")
    @HystrixCommand//如不配置超时时间 这里默认超时时间为一秒 超过一秒未响应则执行全局兜底方法
    public R insertOrder(@PathVariable("seconds") Integer seconds){
        return stockApi.subStock(seconds);
    }

    public R addOrderFallBack(Integer seconds){
        return R.failed("订单系统繁忙,请稍后再试");
    }

    public  R globalFallBack(){
        return R.failed("全局的兜底方法");
    }
}
```
**2.测试**
**访问http://localhost:7001/order/addOrder/6** 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ef48e0898b564185b9b7e45be8807dcb.png)

**当全局降级方法 和 局部降级方法都存在时 会走局部降级方法**

---
**访问http://localhost:7001/order/insertOrder/1** 
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/875d0135c2db4998825df143574d501e.png)
**默认超时时长为1 走全局降级方法**

## 6.对API接口降级方法
**1.开启feign 支持 hystrix 在order模块的yml文件中**
```yml
feign:
  hystrix:
    #开启feign 支持 hystrix 一定要开启 默认为关闭状态
    enabled: true
```
**2.更改OrderController 注意各个注解和findOrder方法**

```java
@RestController
@RequestMapping("/order")
//@DefaultProperties(defaultFallback = "globalFallBack")
public class OrderController{

    @Autowired
    private StockApi stockApi;

    @GetMapping("/addOrder/{seconds}")
    @HystrixCommand(fallbackMethod = "addOrderFallBack",
            commandProperties = {
                    //在6秒钟还没有返回 就执行兜底方法
                    @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "6000")
            })
    public R addOrder(@PathVariable("seconds") Integer seconds){
        return stockApi.subStock(seconds);
    }


    @GetMapping("/insertOrder/{seconds}")
    @HystrixCommand
    public R insertOrder(@PathVariable("seconds") Integer seconds){
        return stockApi.subStock(seconds);
    }

    @GetMapping("/findOrder/{id}")
    public R findOrder(@PathVariable("id") Integer id){
        return stockApi.findStock(id);
    }

    public R addOrderFallBack(Integer seconds){
        return R.failed("订单系统繁忙,请稍后再试");
    }

    public  R globalFallBack(){
        return R.failed("全局的兜底方法");
    }
}
```
**3.对api接口包进行操作**

```java
@Component
@FeignClient(value = "CLOUD-OPENFEGIN-STOCK-SERVICE",fallback = StockApiFallBackService.class)
public interface StockApi {
    /*
     * ip + port  => ribbon => localhost:8091
     * localhost:8091/stock/subStock
     * 远程调用
     * */
    @GetMapping("/stock/subStock/{seconds}")
    R subStock(@PathVariable("seconds") Integer seconds);

    @GetMapping("/stock/findStock/{id}")
    R findStock(@PathVariable("id") Integer id);
}
```
**4.StockApiFallBackService类实现接口StockApi 在此写我们的降级方法**

```java
@Component
public class StockApiFallBackService implements StockApi{
    @Override
    public R subStock(Integer seconds) {
        return R.failed("Api 下的subStock 兜底方法");
    }

    @Override
    public R findStock(Integer id) {
        return R.failed("Api 下的findStock 兜底方法");
    }
}
```

**5.对StockController进行更改**

```java
@RestController
@RequestMapping("/stock")
public class StockController {

    private Integer count = 0 ;

    @Value("${server.port}")
    public Integer port;

    @GetMapping("/subStock/{seconds}")
    //掉用此请求 走降级方法
    @HystrixCommand(fallbackMethod = "subStockFallBack",
            commandProperties = {
                //在6秒钟还没有返回 就执行兜底方法
                @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "6000")
    })
    public R subStock(@PathVariable("seconds") Integer seconds){
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

    @GetMapping("/findStock/{id}")
    //掉用此请求 走降级方法
    @HystrixCommand(fallbackMethod = "findStockFallBack",
            commandProperties = {
                    //在6秒钟还没有返回 就执行兜底方法
                    @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "6000")
            })
    public R findStock(@PathVariable("id") Integer id){
        if (id < 0){
            //异常除法 findStockFallBack 兜底方法
            throw new RuntimeException("id 不能为负数");
        }
        try {
            System.out.println("睡眠"+id+"秒");
            TimeUnit.SECONDS.sleep(id);
        }catch (Exception e){
            e.printStackTrace();
        }
        String info = "当前id:" + id + "的库存有:" + new Random().nextInt(100);
        HashMap<String, Object> map = new HashMap<>();
        map.put("port",port);
        map.put("info",info);
        return R.ok(map);
    }
    //降级方法 参数用不用都要写
    public R subStockFallBack(Integer seconds){
        return R.failed("subStock 的兜底方法");
    }

    //降级方法 参数用不用都要写
    public R findStockFallBack(Integer seconds){
        return R.failed("findStock 的兜底方法");
    }
}

```
**6.测试** 
**访问http://localhost:7001/order/findOrder/-1**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/074f3b0b21704e6cb48564ddaf29ef2f.png)
**访问http://localhost:7001/order/findOrder/6**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/46a0b574211a48dc9b8c77e14ced96d9.png)

---
 **总结**
- **方法一:** 直接在被兜底方法上添加@HystrixCommand注解，并且在括号内标明兜底方法的方法名以及
commandProperties属性。
**注意:** 该方法中兜底方法与被兜底方法必须在同一个类下。该方法可以通过修改
value值等属性对兜底方法进行相应控制。

- **方法二:** 只用在被兜底方法上添加@HystrixCommand注解，不需要在@HystrixCommand注解中标明任何属
性。并在所在类上添加: @DefaultProperties(defaultFallback = "globalOrderFallBack")
该方法大大避免了代码的冗余，同一个兜底方法可以被多次调用，只用在需要被兜底的方法上添加
@HystrixCommand注解即可。
**注意:** 全局兜底方法没有参数。

- **方法三:** 新创建一个类(api包下)，该类实现客服端服务的接口。当然想要让被调用的服务知道该类中是与之
对应的兜底方法，还需要在客服端服务接口上的@FeignClient注解中添加上fallback属性
**注意:** 该方法(第三种方法)只有在运行超时(例如服务端出现高并发，宕机)，兜底方法才会执行，出现运行时
异常则不会执行兜底方法。如果需要兼顾到运行时异常需要采用第1，2种方法。

---
# 三、Hystrix服务熔断
## 1.服务熔断机制简介

**熔断机制是为了应对雪崩效应而出现的一种微服务链路保护机制。**

**当微服务系统中的某个微服务不可用或响应时间太长时，为了保护系统的整体可用性，熔断器会暂时切断请求对该服务的调用，并快速返回一个友好的错误响应。这种熔断状态不是永久的，在经历了一定的时间后，熔断器会再次检测该微服务是否恢复正常，若服务恢复正常则恢复其调用链路。**
## 2.熔断工作流程
**三种熔断状态：**
- **熔断关闭状态：** 当服务访问正常时，熔断器处于关闭状态，服务调用方可以正常地对服务进行调用。

-  **熔断开启状态：** 默认情况下，在固定时间内接口调用出错比率达到一个阈值（例如100条数据10条出错），熔断器会进入熔断开启状态。进入熔断状态后，后续对该服务的调用都会被切断，熔断器会执行本地的降级方法。

- **`半熔断状态`** **在半熔断状态下，熔断器会尝试恢复服务调用方对服务的调用，允许部分请求调用该服务，并监控其调用成功率。如果成功率达到预期，则说明服务已恢复正常，熔断器进入关闭状态；如果成功率仍旧很低，则重新进入熔断开启状态。**



![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/85042832c65846d89a187e38fe04fa84.png)
**Hystrix 实现熔断机制：**
1. 当服务的调用出错率达到或超过 Hystix 规定的峰值后，熔断器进入熔断开启状态。
2. 熔断器进入熔断开启状态后，Hystrix 会启动一个休眠时间窗，在这个时间窗内，调用服务会优先走降级方法
3. 当有请求再次调用该服务时，会直接调用降级方法快速地返回失败响应，以避免系统雪崩。
4. 当休眠时间窗到期后，Hystrix 会进入半熔断转态，允许部分请求对服务原来的主业务逻辑进行调用，并监控其调用成功率。
5. 如果调用成功率达到预期，则说明服务已恢复正常，Hystrix 进入熔断关闭状态，服务原来的主业务逻辑恢复；否则 Hystrix 重新进入熔断开启状态，休眠时间窗口重新计时，继续重复第 2 到第 5 步。
# 四、服务监控 hystrix Dashboard
> **Hystrix 提供了调用监控功能，Hystrix 会持续地记录所有通过 Hystrix 发起的请求的执行信息，并以统计报表的形式展示给用户，包括每秒执行请求的数量、成功请求的数量和失败请求的数量等。**

**实现服务监控 hystrix Dashboard**

**1.在原有的代码基础上新建一个模块 cloud-hystrix-dashboard-service**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/76c634c9d42644c9b0230d74c18f29d8.png)
**2.创建主启动类**
```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, DruidDataSourceAutoConfigure.class})
@EnableHystrixDashboard
public class DashBoardApplication {
    public static void main(String[] args) {
        SpringApplication.run(DashBoardApplication.class,args);
    }
}
```
**3.yml文件**
```java
server:
  port: 9999
```
**4.对order模块的启动类下**
```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, DruidDataSourceAutoConfigure.class})
@EnableEurekaClient
@EnableFeignClients
@EnableHystrix //服务消费方 使用这个注解
public class OpenFeignOrderApplication {

    public static void main(String[] args) {
        SpringApplication.run(OpenFeignOrderApplication.class,args);
    }

    /*
     * 此配置是为了服务监控而已，与服务认错本身无关，springcloud升级后发现的坑
     * ServletRegistrationBean因为springboot的默认路劲不是 /hystrix.stream
     * 只要在自己的项目李配置上下面deServlet就可以了
     * */
    @Bean
    public ServletRegistrationBean getServlet(){
        HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
        registrationBean.setLoadOnStartup(1);
        registrationBean.addUrlMappings("/hystrix.stream");
        registrationBean.setName("HystrixMetricsStreamServlet");
        return registrationBean;
    }
}
```
**5.启动项目 访问 localhost:9999/hystrix**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a3110280677248bc8210665e78929838.png =900x)

**6.进入Hystrix监控页面**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/14ff314e560d4ab896c72d10f68fb61c.png)

**7.访问 http://localhost:7001/order/findOrder/0 观察监控页面变化**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/69db1c2720934179b3401d2c87e4fdd3.png)
通过Hystrix可以观察到我们服务的变化，不过Spring Cloud Alibaba出版的Sentinel会更加整洁，更加好用，但是他们的原理差不多，理解Hystrix可以学习Sentinel会事半功倍
