@[TOC](Spring Cloud Gateway API 网关)

---

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c150ed6dd2c54d77a813bd5b7f5a2619.png#pic_center)

# 一、Spring Cloud Gateway
## 1.API 网关
**基本概念**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0ac9ab6506f94adb98e39e7b749c3184.png =800x)
>**网关的作用其实和我们日常生活中的海关差不多，当从美国想到中国的上海，必须经过中国的海关，处理一切非法进入等，像检查个人随身物品啊，有无签证，是否可以入境等。**
>
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/69f92039b3ce4450bede81f6dc06b398.png =800x)

>**而网关的概念是一个搭建在客户端和微服务之间的服务，我们可以在 API 网关中处理一些非业务功能的逻辑，例如权限验证、监控、缓存、请求路由等。**

---
**API 网关就像整个微服务系统的门面一样,客户端会先将请求发送到 API 网关，然后由 API 网关根据请求的标识信息将请求转发到微服务实例。**

**网关的优势 	：**
- 客户端通过 API 网关与微服务交互时，客户端只需要知道 API 网关地址即可，而不需要维护大量的服务地址。
- 客户端直接与 API 网关通信，能够减少客户端与各个服务的交互次数。
- 客户端与后端的服务耦合度降低。
- 节省流量，提高性能，提升用户体验。
- API 网关还提供了安全，流控，过滤，缓存以及监控等 API 管理功能

---
## 2.Gateway 核心概念
>**Spring Cloud Gateway 
>中文官方文档 ：[https://springdoc.cn/spring-cloud-gateway/](https://springdoc.cn/spring-cloud-gateway/)**
>**官方文档：[https://docs.spring.io/spring-cloud-gateway/reference/spring-cloud-gateway-server-mvc/filters/requestsize.html](https://docs.spring.io/spring-cloud-gateway/reference/spring-cloud-gateway-server-mvc/filters/requestsize.html)**

---
**Spring Cloud GateWay 三个核心概念:**

核心概念|描述
-|-
**Route（路由）**   |Route 是网关的基础元素，由 ID、目标 URI、断言、过滤器组成。当请求到达网关时，<br>由 Gateway Handler Mapping 通过断言进行路由匹配（Mapping），当断言为真时，匹配到路由。
**Predicate 断言）**|路由转发的判断条件，我们可以通过 Predicate 对 HTTP 请求进行匹配，
**Filter（过滤器）**|Filter 是 Gateway 中的过滤器，可以在请求发出前后进行一些业务上的处理

---
## 3.工作流程
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/fffecab21b144563bed01e20355443f7.png#pic_center =500x)
1. **客户端将请求发送到Gateway**
2. **Gateway 通过 Gateway Handler Mapping 找到与请求相匹配的路由，将其发送给 Gateway Web Handler。**
3. **Gateway Web Handler 通过指定的过滤器链（Filter Chain），将请求转发到实际的服务节点中，执行业务逻辑返回响应结果。**
4. **过滤器之间用虚线分开是因为过滤器可能会在转发请求之前或之后执行业务逻辑。**
5. **过滤器可以在请求被转发到服务端前，对请求进行拦截和修改**
6. **过滤器可以在响应返回客户端之前，对响应进行拦截和再处理**
7. **响应原路返回给客户端**

---
# 二、Gateway 路由
## 1.断言 Predicate
### (1) 定义与作用

**什么是断言：**
- **官方术语：** 断言是一种在程序中的一阶逻辑（如：一个结果为真或假的逻辑判断式），目的是为了表示与验证软件开发者预期的结果。当程序执行到断言的位置时，对应的断言应该为真。若断言不为真时，程序会中止执行，并给出错误信息。
- **实际就是一种逻辑判断条件 符合我们的规定（条件）就放行 不符合就拦截**

**定义：**
- **Gateway断言是指在API网关中配置的断言（Predicate），用于对路由请求进行条件判断。当请求满足某个条件时，才会被路由到指定的目标服务。**

**作用：**
- **过滤请求：** 通过定义一系列条件，只允许满足条件的请求通过网关转发到服务。
- **增强安全性：** 可以通过断言来阻止不符合安全策略的请求，提高系统的安全性。
- **灵活路由：** 可以根据不同的条件将请求路由到不同的服务，实现灵活的路由策略。

**使用断言注意事项：**
- 路由与断言的对应关系为“一对多”，一个路由可以包含多个不同断言
- 一个请求想要转发到指定的路由上，就必须同时匹配路由上的所有断言
- 当一个请求同时满足多个路由的断言条件时，请求只会被首个成功匹配的路由转发。

---
### (2) Route Predicate 工厂
**定义：**
> **Route Predicate工厂是一组用于创建路由断言的工厂类。在Spring Cloud Gateway中，Predicate提供了路由规则的匹配机制。当请求到达网关时，网关会根据配置的Predicate对请求进行匹配，如果匹配成功，则请求会被路由到目标服务。**

---
**编写路由配置注意：**
 - routes属性下可以配置多个路由，所以书写时使用 `-` 符号。
 - id属性：自定义路由id，保证唯一即可。
 - uri属性：要访问的目标服务器地址，即Gateway的API网关地址。
 - predicates属性：使用断言判定路由条件。

---
**常见的路由工厂包括**
- **Path Route Predicate Factory:** 请求路径路由断言工厂，用于匹配特定路径的请求

**样例**
```yml
# path 断言 提供的服务隐藏起来，不暴露给客户端，只给客户端暴露 API 网关的地址 10000
# 也就是说当我们访问localhost:10000 和 访问 localhost:7001是一样的效果
# 在服务端口7001嵌套了一个10000端口
server:
  port: 10000
spring:
  cloud:
    gateway: #网关路由配置
      routes:
      - id: path_route #路由 id,没有固定规则，但唯一，建议与服务名对应
        uri: http://127.0.0.1/7001 #匹配后提供服务的路由地址
        #以下为断言条件，必选全部符合条件
        #路径为 /a/order/b,/a/order,/order,/order/b 则该路由匹配
        predicates:
        - Path=/*/order/**
```

---
- **Before Route Predicate Factory:** 指定日期之前路由断言工厂，用于匹配在指定日期时间之前的请求。
- **After Route Predicate Factory:** 指定日期之后路由断言工厂，用于匹配在指定日期时间之后的请求。
- **Between Route Predicate Factory:** 指定日期之间路由断言工厂，用于匹配在两个指定日期时间之间的请求。

**样例**

```yml
spring:
  cloud:
    gateway: #网关路由配置
      routes:
      # 匹配在(时区按照亚洲上海的时区)2024年11月27日之后的任何请求
      - id: after_route #路由 id,没有固定规则，但唯一，建议与服务名对应
        uri: http://127.0.0.1/7001 #匹配后提供服务的路由地址
        predicates:
        - After=2024-11-27T17:42:47.789-07:00[Asia/Shanghai]
      # (时区按照亚洲上海的时区)2025年11月27日之前的任何请求
      - id: before_route
        uri: http://127.0.0.1/7002
        predicates:
        - Before=2025-11-27T17:42:47.789-07:00[Asia/Shanghai]
      # 匹配在(时区按照亚洲上海的时区)2024年11月27日之后 2025年11月27日之前的任何请求
      - id: between_route
        uri: http://127.0.0.1/7003
        predicates:
        - Between=2024-11-27T17:42:47.789-07:00[Asia/Shanghai], 2025-11-27T17:42:47.789-07:00[Asia/Shanghai]
```
---
- **Cookie Route Predicate Factory:** Cookie路由断言工厂，用于匹配请求中包含特定Cookie键值对的请求。

**样例**
```yml
spring:
  cloud:
    gateway: #网关路由配置
      routes:
      - id: cookie_route #路由 id,没有固定规则，但唯一，建议与服务名对应
        uri: http://127.0.0.1/7001 #匹配后提供服务的路由地址
        predicates:
        # 两个参数 一个Cookie名称 另一个Cookie的值
        # 只有当请求中包含名为username且值为chq的Cookie时，路由会被匹配
        - Cookie=username,chq
```

---
- **Header Route Predicate Factory:** 请求头路由断言工厂，用于匹配请求头中包含特定字段和值的请求。

**样例**
```yml
spring:
  cloud:
    gateway: #网关路由配置
      routes:
      - id: header_route #路由 id,没有固定规则，但唯一，建议与服务名对应
        uri: http://127.0.0.1/7001 #匹配后提供服务的路由地址
        predicates:
  		# 两个参数 一个为请求头名称 另一个为正则表达式
        # 当请求中包含名为 X-Request-Id 且其值匹配正则表达式 \d+（它的值是一个或多个数字）时,路由会被匹配
        - Header=X-Request-Id, \d+
```

---
- **Method Route Predicate Factory:** 请求方法路由断言工厂，用于匹配特定HTTP方法的请求，如GET、POST等。

**样例**
```yml
spring:
  cloud:
    gateway:
      routes: #网关路由配置
      - id: method_route #路由 id,没有固定规则，但唯一，建议与服务名对应
        uri: http://127.0.0.1/7001  #匹配后提供服务的路由地址
        predicates:
        # Method 路由谓词工厂接受一个 methods 参数，它是一个或多个参数：要匹配的HTTP方法
        # 如果请求方式是 GET 或 POST，则该路由会匹配。
        - Method=GET,POST
```
----
**这些断言规则也可以复合使用：**

**样例**
```yml
server:
  port: 10000
spring:
  cloud:
    gateway: #网关路由配置
      routes:
      - id: path_route #路由 id,没有固定规则，但唯一，建议与服务名对应
        uri: http://127.0.0.1/7001 #匹配后提供服务的路由地址
        # 以下为断言条件，必选全部符合条件
        # 1.在(时区按照亚洲上海的时区)2024年11月27日之后 2025年11月27日之前
        # 2.只能是POST请求方式
        # 3.且路径满足Path规则
        # 同时满足1,2,3三个条件的请求才会被此路由匹配
        predicates:
        - Path=/*/order/**
        - Method=POST
        - Between=2024-11-27T17:42:47.789-07:00[Asia/Shanghai], 2025-11-27T17:42:47.789-07:00[Asia/Shanghai]
```
## 2.动态路由

**动态路由是指路由器能够根据路由器之间的交换的特定路由信息自动地建立自己的路由表，并且能够根据链路和节点的变化适时地进行自动调整。**

**Spring Cloud Gateway 会根据服务注册中心中维护的服务列表，以服务名作为路径创建动态路由进行转发，从而实现动态路由功能**

**样例**
```yml
server:
  port: 8000
spring:
  application:
    name: cloud-gateway-service
  cloud:
    gateway: #网关路由配置
      routes:
        #将 cloud-provider-payment-7001 提供的服务隐藏起来，不暴露给客户端，只给客户端暴露 API 网关的地址 8000
        - id: provider_payment_routh  #路由 id,没有固定规则，但唯一，建议与服务名对应
          # uri: http://localhost:7001          #匹配后提供服务的路由地址
          # uri 的协议，表示开启 Spring Cloud Gateway 的负载均衡功能,
          # CLOUD-OPENFEGIN-ORDER-SERVICE服务名，Spring Cloud Gateway 会根据它获取到具体的微服务地址。
          uri: lb://CLOUD-OPENFEGIN-ORDER-SERVICE 
          predicates:
            #以下是断言条件，必选全部符合条件
          - Path=/*/order/**
          - Method=GET #只能时 GET 请求时，才能访问

# 将网关注册到Eureka 服务注册中心
eureka:
  instance:
    instance-id: cloud-gateway-service
    hostname: cloud-gateway-service
  client:
    fetch-registry: true
    register-with-eureka: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/
```
---
# 三、Gateway 过滤器

>**GatewayFilter 是 Spring Cloud Gateway 网关中提供的一种应用在单个或一组路由上的过滤器。它可以对单个路由或者一组路由上传入的请求和传出响应进行拦截，并实现一些与业务无关的功能**
## 1.局部过滤器

**GatewayFilter接口:** 应用在单个路由或者一组路由上的过滤器

### (1) GatewayFilter 工厂

>GatewayFilter工厂的主要作用是读取配置文件中的参数，并基于这些参数创建定制化的GatewayFilter对象。这些GatewayFilter对象可以对进入网关的请求和微服务返回的响应进行各种处理，如添加请求头、添加请求参数、修改响应头、限流等。

**常见的GatewayFilter 工厂包括：**

- **PrefixPath:** 拦截传入的请求，并在请求路径增加一个指定的前缀。

**样例**
```yml
spring:
  cloud:
    gateway:
      routes:
      - id: add_request_header_route
        uri: lb://CLOUD-OPENFEGIN-ORDER-SERVICE
        #以下是断言条件，必选全部符合条件
        predicates:
       	- Path=/*/order/**
        - Method=GET
        filters:
        # 拦截传入的请求，并在请求路径增加一个指定的前缀。
        # 一个参数 prefix：需要增加的路径前缀。
        - PrefixPath=/admin
```
---
- **PreserveHostHeader:** 转发请求时，保持客户端的 Host 信息不变，然后将它传递到提供具体服务的微服务中。

**样例**
```yml
spring:
  cloud:
    gateway:
      routes:
      - id: add_request_header_route
        uri: lb://CLOUD-OPENFEGIN-ORDER-SERVICE
        #以下是断言条件，必选全部符合条件
        predicates:
       	- Path=/*/order/**
        - Method=GET
        filters:
        - PreserveHostHeader
```

---
- **AddRequestHeader:** 拦截传入的请求，并在请求上添加一个指定的请求头参数。
- **AddRequestParameter:** 拦截传入的请求，并在请求上添加一个指定的请求参数。
- **AddResponseHeader:** 拦截响应，并在响应上添加一个指定的响应头参数。

**样例**
```yml
spring:
  cloud:
    gateway:
      routes:
      - id: add_request_header_route
        uri: lb://CLOUD-OPENFEGIN-ORDER-SERVICE
        #以下是断言条件，必选全部符合条件
        predicates:
       	- Path=/*/order/**
        - Method=GET
        filters:
        # 拦截传入的请求，并在请求上添加一个指定的请求头参数。
        # 两个参数 name：需要添加的请求头参数的 key,value：需要添加的请求头参数的 value。
        - AddRequestHeader=my-request-header,2024
        # 拦截传入的请求，并在请求上添加一个指定的请求参数。
        # 两个参数 name：需要添加的请求参数的 key,value：需要添加的请求参数的 value。
        - AddRequestParameter=name,chq
        # 拦截响应，并在响应上添加一个指定的响应头参数。
        # 两个参数 name：需要添加的响应头的 key,value：需要添加的响应头的 value。
        - AddResponseHeader=my-response-header,2024
```
---
- **RemoveRequestHeader:**  移除请求头中指定的参数。
- **RemoveResponseHeader:**  移除响应头中指定的参数。
- **RemoveRequestParameter:**  移除指定的请求参数。

**样例**
```yml
spring:
  cloud:
    gateway:
      routes:
      - id: add_request_header_route
        uri: lb://CLOUD-OPENFEGIN-ORDER-SERVICE
        #以下是断言条件，必选全部符合条件
        predicates:
       	- Path=/*/order/**
        - Method=GET
        filters:
        # name：需要移除的请求头的 key
        - RemoveRequestHeader=my-request-header
        # name：需要移除的响应头
        - RemoveResponseHeader=my-response-header
        # name：需要移除的请求参数
        - RemoveRequestParameter=name
```
---
- **RequestSize:** 配置请求体的大小，当请求体过大时，将会返回 413 Payload Too Large。

**样例**
```yml
spring:
  cloud:
    gateway:
      routes:
      - id: add_request_header_route
        uri: lb://CLOUD-OPENFEGIN-ORDER-SERVICE
        #以下是断言条件，必选全部符合条件
        predicates:
       	- Path=/*/order/**
        - Method=GET
        filters:
        # maxSize：请求体的大小,默认值为 B
        - name: RequestSize
            args:
              maxSize: 5000000
```
---
## 2.全局过滤器
**GlobalFilter接口:** 应用在所有的路由上的过滤器

>作用于所有路由，不需要单独配置。开发者可以通过全局过滤器实现一些共通功能，并且全局过滤器也是开发者使用比较多的过滤器

**特点：**
- 无需在配置文件中单独配置，即可对所有路由生效。
- 可以拦截并处理所有通过网关的请求和响应，实现统一的安全检查、日志记录、请求修改等功能。
- 可以与其他过滤器（如路由过滤器）组合使用，形成完整的过滤器链。

**自定义全局网关过滤器（GlobalFilter）:** 

**样例**
```java
// 使用@Component注解将过滤器放入Spring容器中。
@Component
// 日志注解
@Slf4j
public class AuthFilter implements GlobalFilter {

	/**
	 * 执行过滤器中的业务逻辑。
	 * ServerWebExchange：相当于请求响应的上下文。
	 * GatewayFilterChain：网关过滤的链表，用于过滤器的链式调用。
	 */
	@Override
	public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
		log.info("进入自定义的全局过滤器 MyGlobalFilter" + new Date());
		//获取请求参数
		String token = exchange.getRequest().getQueryParams().getFirst("token");
		if(token==null) {
			//响应HTTP状态码（401：没有访问权限）
			exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
			//请求结束
			return exchange.getResponse().setComplete();
		}
		//继续执行过滤器链中的下一个资源
		return chain.filter(exchange);
	}
}
```

