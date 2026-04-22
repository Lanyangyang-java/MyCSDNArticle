@[TOC](微服务是什么 SpringCloud是什么)

---
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/5054dd7beae559c081563f147f565ae6.jpeg#pic_center =300x)

# 一、微服务概述
>**微服务（MicroServices）最初是由 Martin Fowler 于 2014 年发表的论文 [MicroServices](https://martinfowler.com/articles/microservices.html)提到的，有兴趣的朋友可去看一看**

微服务从字面意思上看，即是微小的服务

**服务：**  实际的是项目中的功能模块，可以解决某一个或一组问题，在开发过程中表现为项目中的一个工程或 Moudle。
**微小：** 强调的是单个服务的大小，微服务体积小，复杂度低，单个微服务通常只专注于做 **单个业务** 的功能的服务

--- 
# 二、微服务架构
**微服务架构概述:**
- 微服务架构是一种系统架构的设计风格，与传统的单体式架构不同，微服务架构提单一的应用程序拆分成多个小型服务，这些小型服务都在各自独立的进程中运行，服务之前通常使用HTTP 或 API 等轻量级通信机制 进行通讯
- 这些小型服务通常都是围绕着某个特定的业务进行构建的，每一个服务只专注于做一件事，**专业的事交给专业的人来办**
- 每个服务都能够独立地部署到各种环境中
- 每个服务都能独立启动或销毁而不会对其他服务造成影响
- 服务可以使用不同数据存储技术，甚至使用不同的编程语言。

**微服务的架构特征**
- 单一职责：微服务拆分粒度小，每一个服务都对应唯一的业务能力
- 独立：团队独立、技术独立、数据独立，独立部署
- 面向服务：服务提供统一标准的接口，与语言和技术无关
- 隔离性强：服务调用做好隔离、容错、降级，避免出现级联问题

![请添加图片描述](https://i-blog.csdnimg.cn/direct/bd1e25a1b3014fe08a9d622bfafd78b5.png =800x330)

---
# 三、单体架构
单体架构是微服务架构出现之前业界最经典的软件架构类型，许多早期的项目采用的也都是单体架构。单体架构将应用程序中所有业务逻辑都编写在同一个工程中，最终经过编译、打包，部署在一台服务器上运行**

**将业务的所有功能集中在一个项目中开发，打成一个包部署**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/593381e065494aa9a225cd0eb35a1efb.png =800x300)
**特点：**
- 单体架构：简单方便，高度耦合，扩展性差，适合小型项目。例如：学生管理系统

**优点：**
- 架构简单
- 部署成本低

**缺点：**
- 耦合度高（维护困难、升级困难）

**为什么单体架构向微服务架构转型：**
- 随着业务复杂度的提高，采用单体架构的应用程序的代码量也越来越大，导致代码的可读性、可维护性以及扩展性下降。
- 用户越来越多，程序所承受的并发越来越高，而单体应用处理高并发的能力有限
- 单体应用将所有的业务都集中在同一个工程中，修改或增加业务都可能会对其他业务造成一定的影响，导致测试难度增加。

---
# 四、分布式架构
根据业务功能对系统做拆分，每个业务功能模块作为独立项目开发，称为一个服务。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/726530b027fc44829557d2c243371124.png =800x)
**特点**
- 松耦合，扩展性好，但架构复杂，难度大。适合大型互联网项目，例如：京东、淘宝

**优点：**
- 降低服务耦合
- 有利于服务升级和拓展
	
**缺点：**
- 服务调用关系错综复杂


---
# 五、SpringCloud概述
> **Spring Cloud是目前国内使用最广泛的微服务框架。官网地址：[https://spring.io/projects/spring-cloud](https://spring.io/projects/spring-cloud)**

- **SpringCloud是一款基于 Spring Boot 实现的微服务框架**
- **SpringCloud集成了各种微服务功能组件，并基于SpringBoot实现了这些组件的自动装配，从而提供了良好的开箱即用体验。**
- **SpringCloud被称为构建分布式微服务系统的“全家桶”，它并不是某一门技术，而是一系列微服务解决方案或框架的有序集合**
- **提供了一套简单易懂、易部署和易维护的分布式系统开发工具包。将市面上成熟的、经过验证的微服务框架整合起来，并通过 Spring Boot 的思想进行再封装，屏蔽调其中复杂的配置和实现原理**


**特点：**
- **微服务：**一种良好的分布式架构方案
- **优点：**拆分粒度更小、服务更独立、耦合度更低
- **缺点：**架构非常复杂，运维、监控、部署难度提高



**SpringCloud目前有两代实现：**
- 第一代实现：Spring Cloud Netflix 
- 第二代实现：Spring Cloud Alibaba



**常见的组件包括**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e88441ba71004720b49a36c4de724b9c.png =900x400)
Spring Cloud 组件|描述
-|-
<font color= red>**Spring Cloud Eureka** | 服务注册中心、服务注册与发现
<font color= red>**Spring Cloud Ribbon**| 服务调用，客户端负载均衡组件。
<font color= red>**Spring Cloud Netflix Feign**|基于 Ribbon 和 Hystrix 的声明式服务调用组件。
<font color= red>**Spring Cloud Hystrix**|**豪猪哥**，容错管理组件，为服务中出现的延迟和故障提供强大的容错能力。
<font color= red>**Zuul/Spring Cloud Gateway**|API网关。Zuul和Spring Cloud Gateway都负责请求路由、过滤、安全验证等，作为微服务架构中的入口，统一处理外部请求。其中，**Spring Cloud Gateway**是Spring Cloud官方推出的第二代网关，性能比Zuul更好。
<font color= red>**Spring Cloud Alibaba Nacos**|服务注册与发现，配置管理，服务管理中心
<font color= red>**SpringCloud Alibaba Sentinel**|流量控制，熔断机制，实时监控
<font color= red>**Spring Cloud Alibaba Seata**|分布式事务管理，事务协调，分布式锁，多种存储方式
<font color= red>**Spring Cloud Alibaba Dubbo**|高性能的Java RPC框架，用于实现微服务之间的远程调用，丰富的配置选项和扩展点，支持多种协议和序列化方式，提供了服务治理功能
Spring Cloud Config|Spring Cloud 的配置管理工具，支持使用 Git 存储配置内容，实现应用配置的外部化存储，并支持在客户端对配置进行刷新、加密、解密等操作。
Spring Cloud Bus|Spring Cloud 的事件和消息总线，主要用于在集群中传播事件或状态变化，以触发后续的处理，例如动态刷新配置。
Spring Cloud Stream|Spring Cloud 的消息中间件组件，它集成了 Apache Kafka 和 RabbitMQ 等消息中间件，并通过定义绑定器作为中间层，完美地实现了应用程序与消息中间件之间的隔离。通过向应用程序暴露统一的 Channel 通道，使得应用程序不需要再考虑各种不同的消息中间件实现，就能轻松地发送和接收消息。
Spring Cloud Sleuth|Spring Cloud 分布式链路跟踪组件，能够完美的整合 Twitter 的 Zipkin。

---
# 六、SpringBoot和 SpringCloud的区别与联系
1. **分工不同**
- SpringBoot主要关注单体应用的快速开发，提供了一些基础功能和开发工具；而SpringCloud则是一套完整的微服务解决方案，提供了更多的分布式开发工具和组件
- SpringBoot适用于开发独立的、单体的应用程序；而SpringCloud则适用于构建复杂的、分布式的微服务系统。
- SpringBoot主要依赖于Spring框架的核心功能；而SpringCloud则内部集成了众多分布式开发工具和组件，更加适用于微服务架构。

2. **Spring Cloud 是基于 Spring Boot 实现的**
3. **Spring Cloud 不能脱离 Spring Boot 单独运行**
4. **Spring Boot 能够用于开发单个微服务，但它并不具备管理和协调微服务的能力，因此它只能算是一个微服务快速开发框架，而非微服务框架**
---
# 七、SpringCloud版本选择
在使用 Spring Boot + Spring Cloud 进行微服务开发时，我们需要根据项目中 Spring Boot 的版本来决定 Spring Cloud 版本，否则会出现许多意想不到的错误。

**Spring Boot 与 Spring Cloud 的版本对应关系 2024SpringCloud官网发布**
具体可参考[https://github.com/spring-cloud/spring-cloud-release/wiki/Supported-Versions#supported-releases](https://github.com/spring-cloud/spring-cloud-release/wiki/Supported-Versions#supported-releases)

Spring Cloud |Spring Boot
-|-
2023.0.x （Leyton）	|3.3.x, 3.2.x
2022.0.x （Kilburn）	|3.0.x, 3.1.x (Starting with 2022.0.3)
2021.0.x （Jubilee） |	2.6.x, 2.7.x (Starting with 2021.0.3)
2020.0.x （Ilford	）|2.4.x, 2.5.x (Starting with 2020.0.3)
Hoxton	|2.2.x, 2.3.x (Starting with SR5)
Greenwich|	2.1.x
Finchley|	2.0.x
Edgware|	1.5.x
Dalston|	1.5.x

**Spring Cloud 官方已经停止对 Dalston、Edgware、Finchley 和 Greenwich 的版本更新。**
