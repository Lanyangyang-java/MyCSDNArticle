
@[TOC](JavaWeb  我的学习笔记)

---

# 一、动态网页开发
## 1.动态网页
**动态网页介绍** 
动态网页是指能在服务器端运行、使用程序语言设计的交互式网页。它们会根据某种条件（如时间、用户输入、数据库内容等）的变化，返回不同的网页内容。这种变化不是由静态的HTML代码直接实现的，而是依赖于服务器端的程序逻辑和数据库交互。

**例如：** 日常浏览器的搜索页面，根据不同的操作，返回不同的页面

动态网页相对于静态网页来说，页面代码虽然没有变，但是显示的内容却是可以随着时间、环境或者数据库操作的结果而发生改变的。

---
## 2.系统架构
### C/S架构
**Client/Server（客户端/服务器）**
**C/S架构的特点：** 需要安装特定的客户端软件。 

**优点：**
1. 速度快：软件中的大部分数据都是集成到客户端软件当中，很少量的数据从服务器端传送过来，所以C/S结构的系统速度快
2. 体验好：速度又快，界面又炫酷，体验好
3. 界面炫酷：专门的语言去实现界面，更加灵活
4. 服务器压力小:因为大量的数据都是集成在客户端软件当中，所以服务器只需要传送很少的数据量，当然服务器压力小。
5. 安全性高：因为大量的数据都是集成在客户端软件当中，所以服务器虽然只有一个，就算服务器那边地震了，火灾了，服务器受损，问题也不大，因为大量的数据都在多个客户端上有缓存，有存储，所以从这个方面说，C/S结构的系统比较安全。

**缺点：**
升级维护比较差劲，升级维护比较麻烦，成本高，每个客户端软件都需要升级，有一些软件不是那么容易安装的。

----
### B/S架构

**B/S（Browser/Server，浏览器/服务器）** 
**实际上B/S结构的系统还是一个C/S，只不过这C比较特殊，这个Client是一个固定不变浏览器软件。**

**优点：**
1. 升级维护方便，成本比较低（只需要升级服务器端即可。）
2. 不需要安装特定的客户端软件，用户操作及其方便。只需要浏览器，输入网址即可

**缺点：**
1. 速度慢：不是因为宽带低的原因，是因为所有的数据都是在服务器上，用户发送的每应该请求都是需要服务器全身心的响应数据，所有B/S结构的系统在网络中传送的数据量比较大。
2. 体验差：界面不是那么炫酷
3. 不安全：所有数据都在服务器上，只要服务器挂了，最终数据全部丢失。


开发B/S结构的系统，其实就是开发网站，其实就是开发一个Web系统

---
### B/S与C/S的比较
名称|B/S架构|C/S架构
-|-|-
软件安装|浏览器|需要专门的客户端
升级维护|客户端维护方便|客户端需要单独维护升级
平台相关|与操作系统平台关系不大|对客户端操作系统一般有限制
性能安全|在响应速度上和安全成本上需要花费更多的设计成本|能充分发挥客户端处理能力，客户端响应更快

---
## 3.URL通信三要素
1. **IP地址**：电子设备(计算机)在网络中的唯一标识。
2. **端口**：应用程序在计算机中的唯一标识。 0~65536
3. **传输协议**：规定了数据传输的规则,如http, ftp等

---
## 4.Tomcat服务器

**1.Tomcat简介**

**服务器介绍：**
服务器：安装了服务器软件的计算机
服务器软件：接收用户的请求，处理请求，做出响应
在web服务器软件中，可以部署web项目，让用户通过浏览器来访问这些项目
Tomcat就是web服务器的一种

**2. Tomcat的目录结构：**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/86954585cc4b418090cef424452e0247.png)


官方下载地址：[https://tomcat.apache.org/](https://tomcat.apache.org/)

**3. Tomcat基本使用**

1. 进入到Tomcat的目录中bin目录中 进入cmd 输入startup.bat
2. Tomcat服务器的启动文件在bin目录中：startup.bat 就是Tomcat的启动文件。
3. Tomcat服务器的停止文件也在bin目录中：shutdown.bat 就是Tomcat的停止文件
4. 启动Tomcat服务器之后, 能够正常访问 http://localhost:8080



**问题分析解决**
1. **启动一闪而过**
  **原因：** 没有配置环境变量。
  **解决办法：** 配置上JAVA_HOME环境变量
2. **报错: Address already in use : JVM_Bind**
**原因：** 端口被占用
**解决办法:** 找到使用该端口的进程 结束进程

**正确启动关闭tomcat**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/9d1763588dbb47feabe0b543faa08704.png =500x)


**4.IDEA集成Tomcat**
创建Tomcat

---
![1.](https://i-blog.csdnimg.cn/direct/68220453f58b409b9ead16deaf7223eb.png =600x)
配置Tomcat

---
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/da8a6d0be28e4cd092cf9dbe34ee3bb5.png =600x)
tomcat各项功能介绍

---
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e54f24bb8fe34e919ae9640c1fe1bd10.png =600x)

# 二、Servlet
## 1.Servlet简介
**运行在服务器端的小程序**
1. - Servlet就是一个接口，定义了java类被浏览器访问到（tomcat识别到）的规则。
2. Servlet就javaweb三大组件之一，三大组件分别是
 Servlet程序
 Filter过滤器
 Listener监听器
 
Servlet是在服务器端运行的Java程序，可以接收客户端请求并做出响应
Servlet可以动态生成HTML内容对客户端进行响应
Servlet与JSP都可以动态生成HTML内容
## 2.Servlet快速入门

### 入门样例
1.创建javaWEB项目
![请添加图片描述](https://i-blog.csdnimg.cn/direct/edfd8c6a1dfe463e978b2ecadd8f2266.png)
2.导入依赖
```xml
	<dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
    </dependency>
```
3.定义一个类，实现Servlet接口，在service方法里,输出内容
```java
public class ServletTest1 implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }
    @Override
    public ServletConfig getServletConfig() {
        return null;
    }
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("Hello Servlet");
    }
    @Override
    public String getServletInfo() {
        return null;
    }
    @Override
    public void destroy() {

    }
```
4.配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app
    version="4.0"
    xmlns="http://xmlns.jcp.org/xml/ns/javaee"
    xmlns:javaee="http://xmlns.jcp.org/xml/ns/javaee"
    xmlns:xml="http://www.w3.org/XML/1998/namespace"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd">
  <display-name>Archetype Created Web Application</display-name>
  <!--配置Servlet-->
  <!--servlet标签给tomcat配置Servlet程序-->
  <servlet>
    <!--servlet-name标签Servlet程序起一个别名（一般用于类名）-->
    <servlet-name>ServletTest1</servlet-name>

    <!--servlet-class是Servlet程序的全类名-->
    <servlet-class>com.chq.ServletTest1</servlet-class>
  </servlet>

  <!--servlet-mapping标签给Servlet程序配置访问地址-->
  <servlet-mapping>
    <!--servlet-name标签的作用是告诉服务器，
    我们当前配置的地址给那个Servlet程序使用-->
    <servlet-name>ServletTest1</servlet-name>

    <!--url-pattern标签配置访问地址
    / 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路劲
    /hello表示地址为：http://ip:port/工程路劲/hello    -->

    <url-pattern>/hello</url-pattern>
  </servlet-mapping>
</web-app>
```
5.配置启动Tomcat 在浏览器中访问 http://localhost:8080/ServletTest1/hello![请添加图片描述](https://i-blog.csdnimg.cn/direct/b30a1f09e4cb453b80380f48656e52ec.png)

6.观察控制台 打印出Hello Servlet


### 执行原理
1. 当服务器接受到客户端浏览器的请求后，会解析请求URL路劲，获取访问的Servlet的资源路劲
2. 查找web.xml文件，是否有对应的` <url-pattern> `标签内容
3. 如果有，则在找到对应的 `<servlet-class>` 全类名
4. tomcat会将字节码文件加载到内存，并且创建其对象
5. 调用方法

**执行过程**
浏览器——>Tomcat服务器——>我们的应用——>应用中的web.xml——>Servlet类——>响应浏览器

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f80a6bbafca34e3d8bdd23f4a9a45e5e.png)


## 3.Servlet的体系结构
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ddcecb09590544dca392eaffe6c77f7e.png)
GenericServlet——抽象类：将Servlet接口中其他方法做了默认空实现，只将service()方法作为抽象，将来定义servlet类时，可以继承GenericServlet，实现service()方法即可
```java
public class ServletTest2  extends GenericServlet{
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
            System.out.println("Hello GenericServlet");
    }
}
```
HttpServlet——抽象类：对http协议的一种封装，简化操作
1. 定义类继承HttpServlet
2. 复写doGet/doPost方法

```java
public class ServletTest3 extends HttpServlet{
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("get请求");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("post请求");
    }
}
```

**注意：**
1. 继承了HttpServlet，需要重写里面的doGet和doPost方法来接收get方式和post方式的请求。
2. 为了实现代码的可重用性，我们只需要在doGet或者doPost方法中一个里面提供具体功能即可，而另外的那个方法只需要调用提供了功能的方法,不实现功能浏览器会报405错误
## 4.servlet的十大方法
方法名|说明
-|-
init（HTTPServletConfig config）|被servlet容器调用与指明一个servlet被放进服务中，当Servlet第一次被请求的时候Servlet容器会调用这个方法,在后续的请求中不会被再次调用
destroy()|被servletcontainer调用以告知一个servlet它被剔除服务，当销毁Servlet的时候,该方法被调用
service(HttpServletRequest request,HttpServletResponse response）|被servlet容器调用以允许servlet响应一个请求，每当请求Servlet的时候多会调用一次。
doGet(httpServletRequest request,HttpServletResponse response)|被server调用处理客户端的GET请求。
doPost(HttpServletRequest request,HttpServletResponse response)|被server调用处理客户端的POST请求。
doPut（HttpServletRequest request,HttpServletResponse response）|被server调用处理客户端的DELETE请求。
doDelete(HttpServletRequest request,HttpServletResponse response)|被server调用处理客户端的DELETE请求。
getServletInfo()|返回有关Servlet的信息，例如作者、版本、版权。
getServletConfig()|被server调用处理客户端的Head请求.
doHead(HttpServletRequest request,HttpServletResponse response):|被server调用处理客户端的Head请求.

## 5.Servlet生命周期
 服务器启动时，会在web.xml文件中找到servlet文件的配置并创建servlet的实例
1.  init（）|第一次访问servlet的时候调用，全过程仅执行一次（初始化）；
2. service（）|每一次访问servlet的时候调用（处理请求和响应）；
3. destroy（）| 清理或销毁，服务器关闭。

## 6.在web.xml中配置servlet
**1.servlet的名字，包装**
```xml
<servlet>
	<!--为servlet取的名字-->
	<servlet-name>ServletTest1</servlet-name>  
	<!--指明为哪一个servlet类起个名字，名字要写全限定名；-->
	<servlet-class>com.chq.ServletTest1</servlet-class>
</servlet>
<!--serlvet一般情况是有用户第 一次访问的时候才初始化，
如果需要应用程序一启动就初始化，需要配置load-on-startup-->
<servlet>
	<servlet-name>ServletTest1</servlet-name>
	<servlet-class>com.chq.ServletTest1</servlet-class>
	<!--当值为0或者大于0时，表示容器在应用启动时就加载这个servlet,正数的值越小，启动该servlet的优先级越高-->
	<load-on-startup>1</load-on-startup>
	
</servlet>

```

**2.映射、访问的地址（url）**
```xml
<!--用于映射一个已注册的servlet的一个对外访问路径-->
<servlet-mapping>
	<!--与上面的servlet名字要完全一样,指明为哪一个servlet类配置对外访问路径-->
	<servlet-name>ServletTest1</servlet-name>
	<!--运行时地址栏显示的文件名,指明对外访问资源路径-->
	<url-pattern>/hello</url-pattern> 
</servlet-mapping>
```
同一个Servlet可以被映射到多个URL上，即多个 `<servlet-mapping>` 的`<servlet-name>` 的值，可以是同一个Servlet

**3.配置默认首页:** `<welcome-file-list>`


**4.session有效期的设置**
```xml
<session-config>
	<session-timeout>300</session-timeout>
</session-config>
```


**5.servlet全局参数的配置**
```xml
<context-param>
	<param-name>encoding</param-name>
	<param-value>UTF-8</param-value>
</vontext-param>
```

## 6.servlet的注解方式。
通过web.xml方式配置servlet是web3.0版本以前常用的方式，那么在3.0版本后，使用注解的方式代替配置文件。
**支持注解配置，可以不需要web.xml了**

**servlet注解方式**
**注解：**  @WebServlet(value="/myServlet1",name = “MyServlet”)
参数名称|说明
-|-
 String name() default ”“|名字
 String[] value() default {}| 获取参数值
 String[] urlPatterns() default {}| 访问路径
 int loadOnStartup() default -1| 启动时机
 WebInitParam[] initParams() default {}| 初始化参数
 loadOnStartup|servlet初始化时机，默认是第一次访问servlet的时候初始化 这个时候loadOnStartup值为-1.

```java
@WebServlet("/test2")
public class ServletTest2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("get方式于注解测试");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("post方法注解测试");
    }
}
//控制台打印：get方式于注解测试
```
**控制台出现中文乱码：**
在VM options上加上`-Dfile.encoding=utf-8`
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/99aa5bf1f45e4d44ac062aee3975a232.png)

**Servlet的映射方式**
1. 具体名称的方式。访问的资源路径必须和映射配置完全相同。(常用) 如上述例子
2. / 开头 + 通配符的方式。只要符合目录结构即可，不用考虑结尾是什么
3. 通配符 + 固定格式结尾的方式。只要符合固定结尾格式即可，不用考虑前面的路径
```java
//只要请求符合这个目录结构/ServletTest/, 后面无论是什么都能匹配到这个  
@WebServlet("/ServletTest/*")
//匹配所有, 任何请求都能匹配到这个
@WebServlet("/*")
//匹配所有以.cxk结尾的请求
@WebServlet("*.cxk")
```


**Servlet的多路径映射**
我们可以给一个 Servlet 配置多个访问映射，从而可以让不同的请求路径来访问相同的Servlet。
```java
//@WebServlet里面可以写多个路径, 使用{}包裹,用逗号分隔
@WebServlet({"/test3","/test4","/test5"})
```
# 三 Request(请求)
## 1.Request继承体系
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b496063c3736461b959edf791af096f9.png)
## 2.Request的功能
1.  获取请求行（getContextPath、getServletPath、getRequestURI，getQueryString） 包含HTTP方法、请求URL和HTTP版本。
 2. 获取请求头（getHeader） 包含多个键值对，用于传递额外的信息给服务器。
 3. 获取请求体（getReader / getInputStream） 用于POST、PUT等包含数据的请求方法，包含要发送给服务器的数据。


## 3.请求消息数据
**请求行**
关于请求方式：HTTP协议有7种请求方式，常用的有两种：
  1. GET：请求参数在请求行中(url中)；请求的url有长度限制；不太安全
  2. POST：请求参数在请求体中；请求的url没有长度限制；相对安全
  3. 请求方式 请求url 请求协议/版本

**请求头**
 1. 键值对。是客户端浏览器告诉服务器的信息。
 2. 关于几个重要的请求头键/值：
 	 User-Agent：浏览器告诉服务器"我是哪个版本的浏览器"。这样服务器才能"对症下药"，解决浏览器兼容性问题
 	 Referer：告诉服务器"我的请求的来源"。这被用于 防盗链 和 统计工作。

**请求空行:** 就是个空行。分割了 请求头 和 请求体

**请求体:** POST方式有请求体，GET方式没有。流的形式获取。
## 4.Request获取请求数据
1. **请求行**

方法|描述
-|-
String  getMethod()|获取请求方式：GET
String  getContextPath()|获取虚拟目录 （重要）
String  getServletPath()|获取Servlet路劲
String  getQueryString()|获取get方式请求参数
String  getRequestURI()|获取请求URI （重要）
StringBuffer getRequestURL()|获取get方式请求参数，获取请求URL
String  getProtocol()|获取协议版本：HTTP/1.1
String getRemoteAddr()|获取客户机的IP地址

**样例：**
```java
  @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse resp){
        // 1. String  getMethod()获取请求方式
        String method = request.getMethod();
        System.out.println("method = " + method);
        //2. String  getContextPath()获取虚拟目录
        String contextPath = request.getContextPath();
        System.out.println("contextPath = " + contextPath);
        //3. String  getServletPath()获取Servlet路劲
        String servletPath = request.getServletPath();
        System.out.println("servletPath = " + servletPath);
        //4. String  getQueryString()获取get方式请求参数
        String queryString = request.getQueryString();
        System.out.println("queryString = " + queryString);
        //5. String  getRequestURL()获取请求URL
        String requestURI = request.getRequestURI();
        System.out.println("requestURI = " + requestURI);
        //6. StringBuffer getRequestURL()获取get方式请求参数
        StringBuffer requestURL = request.getRequestURL();
        System.out.println("requestURL = " + requestURL);
        //7. String  getProtocol()获取协议版本
        String protocol = request.getProtocol();
        System.out.println("protocol = " + protocol);
        //8. String getRemoteAddr()获取客户机的IP地址
        String remoteAddr = request.getRemoteAddr();
        System.out.println("remoteAddr = " + remoteAddr);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
```

2. **请求头**

方法名|说明
-|-
String getHeader（String name）|根据请求头获取一个值

常见请求头
名称|说明
-|-
Host|指定了请求的目标域名和端口号。
User-Agent|包含了客户端（如浏览器）的类型、版本、操作系统等信息。
Accept|指定客户端能够处理的MIME类型，如text/html、application/json等。
Authorization|包含认证信息，如令牌（Token）或基本认证（Base64编码的用户名和密码）。
Cookie|包含客户端存储的Cookie信息，用于会话管理。

```java
 @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
        // 获取请求头：user-agent:浏览器版本信息
        String agent = req.getHeader("User-Agent");
        System.out.println(agent);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)  {

    }
```
3. **请求体**
用于POST、PUT等包含数据的请求方法，包含要发送给服务器的数据。
请求体不是每个HTTP请求都必需的，它主要用于POST、PUT等包含数据的请求方法。请求体包含了要发送给服务器的数据，数据的格式可以是文本、表单数据、JSON等。
```html
<!--前端表格-->
<form action="/myServlet5" method="post">
     <input type="text" placeholder="请输入用户名" name="username"><br>
     <input type="text" placeholder="请输入密码" name="password"><br>
     <input type="submit" value="注册">
 </form>
```

```java
@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
    //获取post  请求体  请求参数
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        //1.获取字符流输入流
        BufferedReader reader = req.getReader();
        //2.读取数据
        String line = null;
        while ((line = reader.readLine()) != null){
            System.out.println(line);
        }
        reader.close();
    }
```
	
## 5.request其他功能
1. 请求转发（request.getRequestDispatcher("/另一个资源路径").forward(req,resp)）
一种在服务器内部的资源跳转方式   

请求转发的特点：
浏览器地址栏路径不发生变化
只能转发到当前服务器的内部资源
一次请求，可以在转发的资源间使用request共享数据

名称|描述
-|-
RequestDispatcher getRequestDispatcher（String name）| 获取请求调度对象
void forward(HttpServletRequest req, HttpServletResponse resp)  |实现转发

2. 共享数据（setAttribute / getAttribute）
 请求转发资源间共享数据：使用Request对象

名称|描述
-|-
void  setAttribute(String  name,Object obj)|存储数据到request域中
Object  getAttitude(String  name)|根据key，获取值
void  removeAttribute(String  name) |根据key，删除该键值对

3. 获取ServletContext对象（getServletContext）这个在后面会说


# 四、Response（响应）
## 1.Response组成部分
1. **响应行：**
包含HTTP版本（例如HTTP/1.1）、状态码（如200）和状态消息（如OK）三个要素。
状态码是一个三位数字，用于表示请求的处理结果。
**状态码**

状态码类型|说明
-|-
1xx|信息性状态码，表示请求已被接收，需要继续处理。
2xx|成功状态码，表示请求已成功被服务器接收、理解并接受。
3xx|重定向状态码，表示需要客户端采取进一步的操作才能完成请求。
4xx|客户端错误状态码，表示请求包含语法错误或无法完成请求。
5xx|服务器错误状态码，表示服务器在处理请求的过程中发生了错误。

2. **响应头：**
包含多个键值对，提供关于响应的元数据。
常见的响应头包括Content-Type（内容类型）、Content-Length（内容长度）、Set-Cookie（设置Cookie）等。
**常见的响应头**

名称|说明
-|-
Cache-Control|用于控制缓存策略，如缓存时间、是否允许缓存等。
Content-Type|表示响应内容的MIME类型，如text/html、application/json等。
Content-Length|表示响应内容的长度（字节数）。
Content-Encoding|表示响应内容的压缩算法，如gzip。
Set-Cookie|用于设置与页面关联的Cookie。
Server|表示服务器的类型或软件信息。
Date|表示响应生成的GMT时间。
3. **空行：**
响应头和响应体之间有一个空行（回车符和换行符），用于分隔它们。
4. **响应体：**
服务器返回给客户端的实际数据。响应体的内容类型由Content-Type头指定，可以是纯文本、HTML页面、JSON数据等。如果请求的是HTML页面，那么响应体就是该页面的HTML代码；如果请求的是JSON数据，那么响应体就是序列化后的JSON字符串。
## 2.Response响应字符数据
**使用**
1.通过Response对象获取字符输出流
```java
PrintWriter writer = response.getWriter();
```
2.写数据
```java
writer.write("hello");
```
**样例**
```java
@WebServlet("/myResponse1")
public class MyResponse1  extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.获取字符输出流
        PrintWriter writer = response.getWriter();
        //告诉浏览器，服务器发送的消息数据的编码，建议浏览器使用该编码解码
        //response.setHeader("content-type","text/html;charset=utf-8");
        //简化版
        response.setContentType("text/html;charset=utf-8");
        //2.输出数据
        writer.write("hello");
        writer.write("<h1>hello</h1>");
        String json = "{" +
                "\"name\":\"zhangsan\"," +
                "\"age\":18" +
                "}";
        writer.write(json);
        //注意：writer流不要关闭，因为这个流随着response获取出来的，这一次请求响应完毕之后，
        //response对象会被销毁，销毁的时候这个流会自动的关闭掉，所以我们不需要手动关闭。
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
        this.doGet(request, response);
    }
}
```
## 3.Response响应字节数据
**使用**
1.通过Response对象获取字输出流
```java
ServletOutputStream os = response.getOutputStream();
```
2.写数据
```java
os.write(字节数据);
```
**样例**
```java
@WebServlet("/myResponse2")
public class MyResponse2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String path = "S:\\JavaProjectPackages\\JavaWeb\\Servlet_Demo\\src\\main\\webapp\\img\\1.jpg";
        //1.读取文件
        //获取字节输入流
        FileInputStream fis = new FileInputStream(path);
        //2.获取response字节输出流
        ServletOutputStream os = response.getOutputStream();
        //3.完成流的copy
        byte[] bytes = new byte[1024];
        int len = 0;
        while ((len = fis.read(bytes))!= -1){
            os.write(bytes, 0, len);
        }
        fis.close();
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}

```
## 4.重定向
1. **重定向的定义**
简单来说就是一种资源跳转方式
**定义：** 重定向是指通过各种方法将网络请求（如浏览器请求）重新定个方向转到其它位置，如网页重定向、域名的重定向、数据报文重定向等。在网站建设中，重定向常用于处理URL更改、页面跳转、访问权限限制等场景

2. **重定向的类型**
 常见的重定向状态码有
 
 状态码|说明
 -|-
 301|表示永久性转移（Moved Permanently），即请求的资源已经被永久移动到了由Location头部指定的URL上。<br>搜索引擎会根据该响应修正链接，以后所有的请求都应该被重定向到新的URL。
302|表示暂时性转移（Moved Temporarily，或者Found），即请求的资源被暂时移动到了由Location头部指定的URL上。<br>浏览器会重定向到这个URL，但是搜索引擎不会对该资源的链接进行更新，仍然会保留原URL的记录。
303|通常用于临时重定向，并提示客户端使用GET方法重新访问被重定向的URL。这有助于在资源访问时提供临时的展示页。
307|与302类似，但要求客户端在重定向时保持原有的请求方法不变。
308|表示永久重定向，但与301不同的是，它要求客户端在重定向时使用原有的请求方法。

3. **重定向特点**
	浏览器地址栏路径发生变化
	可以重定向到任意位置的资源（服务器内部、外部均可)
	两次请求，不能在多个资源使用request共享数据

4. **重定向和请求转发的对比**

![请添加图片描述](https://i-blog.csdnimg.cn/direct/3981cd96c26a4b0ebf42a67ecfa45343.png)
区别 |重定向（Redirect）| 请求转发（forword）
-|-|-
浏览器地址栏路径|发生变化|不发生变化
访问范围|可以重定向到任意位置的资源|只能转发到当前服务器的内部资源
请求次数|两次请求|一次请求
共享数据|不能在多个资源使用request共享数据|可以在转发资源间使用request共享数据

**样例**
```java
//访问外部资源
@WebServlet("/myResponse3")
public class MyResponse3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("Hello MyResponse3");
        response.sendRedirect("https://www.baidu.com");
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
//访问我的服务器资源
public class MyResponse4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("Hello MyResponse4");
        String contextPath = request.getContextPath();
        response.sendRedirect(contextPath+"/myResponse4");
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```
# 五、ServletContext
## 1.ServletContext简介
ServletContext 是应用上下文对象。每一个应用中只有一个 ServletContext 对象，代表整个web应用，可以和程序的容器(服务器)来通信。
**ServletContext特性**
特性|说明
-|-
全局唯一性|在一个 Web 应用程序中，ServletContext只有一个
生命周期|应用一加载则创建，应用被停止则销毁。
数据共享| 允许不同的 Servlet 之间共享数据，可以在 ServletContext 中存储和检索对象。
访问资源|提供了访问 Web 应用资源的方法
初始化参数|可以读取在 web.xml 文件中配置的初始化参数
请求转发|可以获取 RequestDispatcher对象，该对象允许将请求转发到另一个资源
监听器|支持监听器，可以监听 Web 应用的各种事件

**不过随着javaEE的发展，spring框架的提供了更加灵活和强大的方式来管理 Web 应用的上下文和共享数据。**
## 2.ServletContext获取
1. 通过request对象获取
```java
request.getServletContext();
```

2. 通过HttpServlet获取
```java
this.getServletContext();
```

**样例**
```java
@WebServlet("/servletContext1")
public class ServletContext1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response){
        // 1.通过request对象获取
        ServletContext context1 = request.getServletContext();
        // 2.通过HttpServlet获取
        ServletContext context2 = this.getServletContext();
        System.out.println(context1);
        System.out.println(context2);
        System.out.println(context1 == context2); //true
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
        this.doGet(request, response);
    }
}
```

## 3.ServletContext作用

**1.作为域对象共享数据**
域对象指的是对象有作用域。也就是有作用范围。域对象可以实现数据的共享。不同作用范围的域对象，共享数据的能力也不一样。在 Servlet 规范中，一共有 4 个域对象。ServletContext 就是其中的一个。它也是 web 应用中最大的作用域，也叫 application 域。它可以实现整个应用之间的数据共享！

**实现方法**
方法名称|说明
-|-
void setAttribute(String name,Object value)|向域对象储存数据，并设置数据名称
Object getAttribute(String name)|通过名称获取域对象的数据
void removeAttribute(String name)|通过名称移除域对象的数据

**样例**
```java

@WebServlet("/servletContext2")
public class ServletContext2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
        //1.通过HttpServlet获取
        ServletContext context = this.getServletContext();
        //2.存储数据
        context.setAttribute("msg","hello ServletContext");
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
        this.doGet(request, response);
    }
}

//共享数据
@WebServlet("/servletContext3")
public class ServletContext3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
        //1.通过HttpServlet获取
        ServletContext context = this.getServletContext();
        //2.获取数据
        Object msg = context.getAttribute("msg");
        System.out.println("msg = " + msg);
        //删除数据
        context.removeAttribute("msg");
        System.out.println("msg = " + msg);
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
        this.doGet(request, response);
    }
}
```

**2.获取文件的真实路径**

**实现方法**
方法名称|说明
-|-
String getRealPath(String path)|获取文件的真实路径

```java
//web目录下资源访问
String b = context.getRealPath("/a.txt")
//WEB-INF目录下的资源访问
String c = context.getRealPath("/WEB-INF/c.txt");
//src目录下的资源访问
String a = context.getRealPath("/WEB-INF/classes/c.txt");
```
**3.获取MIME类型**
MIME (Multipurpose Internet Mail Extensions) 是描述消息内容类型的标准，用来表示文档、文件或字节流的性质和格式。
MIME 消息能包含文本、图像、音频、视频以及其他应用程序专用的数据

**常见MIME类型**
类型名称|说明|常见格式
-|-|-
text|表明文件是普通文本|text/html, text/css, text/javascript
image|表明是某种图像。不包括视频，但是动态图（比如动态gif）也使用image类型|image/gif, image/png, image/jpeg
audio|表明是某种音频文件|audio/midi
vidio|表明是某种视频文件|video/mp4
application|表明是某种二进制数据|application/xml

**获取方法**
方法名称|说明
-|-
String getMimeType(String file)|根据文件名称获取该文件的MIME类型

**样例**
```java
@WebServlet("/servletContext4")
public class ServletContext4  extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
        //ServletContext获取MIME类型
        //1.通过HttpServlet获取
        ServletContext context = this.getServletContext();
        //2.定义文件名称
        String filename = "a.mp4";
        //3.获取MIME类型
        String mimeType = context.getMimeType(filename);
        System.out.println("mimeType = " + mimeType);//image/jpeg
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
        this.doGet(request, response);
    }
}
```
# 六、JSP
## 1.JSP简介
**定义：** JSP是运行在Java服务器中的页面，即JavaWeb中的动态页面。其本质是一个Servlet，不能脱离服务器运行。
**构成：** JSP页面由静态内容和动态内容构成。静态内容即HTML元素，动态内容即JSP元素，包括指令元素、脚本元素、动作元素、注释等。

## 2.语法声明
**1.JSP的注释**
 **作用：** 注释Java脚本代码

 **语法：** 
```java
<%– 注释 –%>
```


**2.JSP的Java脚本表达式**
**作用：** 输出数据到页面上

**语法：**
```java
 <%= %>   <%-(实际上就是调用输出流打印到页面上) out.print(表达式);-%>
 //样例
 <%="hello jsp"%>
```

**3.JSP中的Java脚本片段**
 实际开发中，应做到JSP中不能出现一行Java脚本片段
 作用：书写Java代码逻辑，可以直接编写Java代码
 语法：
```java
<% %>
//样例
<%
   System.out.println("hello,jsp~");
%>
```
## 3.jsp指令
**1.标准指令**
设定JSP网页的整体配置信息
**特点:**
1. 并不向客户端产生任何输出，
2. 指令在JSP整个文件范围内有效
3. 为翻译阶段提供了全局信息

**指令的语法格式:**
```java
<%@ 指令名称 属性名=“属性值” 属性名=“属性值” …%>
```
**2.page指令**	
表示JSP页面相关的配置信息

**常用属性**
属性名称|说明
-|-
 language| 表示在JSP中编写的脚本的语言.(只能是java)
 contentType| 表示JSP输出的MIME类型和编码.等价于 response.setContentType(“text/html;charset=utf-8”);
 pageEncoding| 表示JSP输出的编码;等价于response.setCharacterEncoding(“utf-8”);
 import| 用于导入JSP脚本中使用到的类,等价于Java代码中的: import 类的全限定名;****注意：一个import属性可以导入多个包，用逗号分隔。
 errorPage| 指示当前页面出错后转向（转发）的页面。目标页面如果以"/"（当前应用）就是绝对路径。


**指令的语法格式:**
```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
```

**3.include指令**
用于在JSP页面中包含一个文件，该文件可以是HTML网页、文本文件或一段Java代码。使用include指令可以简化页面代码，提高代码的重用性。

**指令的语法格式:**
```java
<%@include file="" %>
```

**静态包含**
 1. 使用JSP的include指令 <% @include file="被包含的页面文件" %>
 2. **特点:** 在翻译阶段就已经把多个JSP,合并在一起,最终翻译得到一个java文件

**动态包含**
1.  使用JSP的动作指令 <jsp: include page="被包含的页面文件" />
2. **特点:** 把每一个JSP都翻译成Servlet类,在运行时期,动态的合并在一起,最终得到多个java文件

**4.taglib(标签库)指令**
该指令用于引入外部标签库。html标签和jsp标签不用引入。

**指令的语法格式:**
```java
<%taglib uri="" prefix=""%>
```
**属性：**
1. uri：外部标签的URI地址。
2. prefix：使用标签时的前缀 自定义。
# 七、EL表达式
## 1.EL表达式简介
1. EL（全称Expression Language ）表达式语言，用于简化 JSP 页面内的 Java 代码。
2. EL 表达式的主要作用是 获取数据。其实就是从域对象中获取数据，然后将数据展示在页面上。
3. 而 EL 表达式的语法也比较简单，`${expression}` 。例如：`${brands}` 就是获取域中存储的 `key` 为 `brands` 的数据。

## 2.EL表达式的作用
1. 让 jsp 书写起来更加的方便。简化在 jsp 中获取作用域或者请求数据的写法。也会搭配 Jstl 来进行使用。只起到获取作用域和请求中的数据。
2. **语法结构：** `${expression}`，提供`.`和`[ ]`(通常获取下标)两种运算符来存取数据。
注意：自带响应功能
## 3.EL表达式运算符
**1.算数运算符**
运算符|作用|示例
-|-|-
+|加|`${1+2}`
-|减|`${2-1}`
*|乘|`${2*3}`
/或者div|除|`${5 / 2}`或者`${5 div 2}`
%或者mod|取余|`${5 % 2} `或者`${5 mod 2}`

**2.关系运算符**
运算符|作用|示例
-|-|-
==或eq|等于|${1 == 1} 或 ${1 eq 1}
!=或 ne|不等于|${1 != 2} 或 ${1 ne 2}
<或lt|小于|${2 < 3} 或 ${2 lt 3}
`>`或gt|大于|${3 > 2} 或 ${3 gt 2}
`<=`或le|小于等于|${2 <= 3} 或 ${2 le 3}
`>=`或 ge|大于等于|${3 >= 2} 或 ${3 ge 2}

**3.逻辑运算符**
运算符|作用|示例
-|-|-
&&或 and|并且|${A && B} 或 ${A and B}
`‖`或or|或者|${A || B} 或 ${A or B}
`!` 或 not|取反|${!A }或 ${ not A }

**4.其他运算符**  
运算符|作用|示例
-|-|-
empty|判断对象是否为null<br>2.判断字符串是否为空字符串<br>3.判断容器元素长度是否为0|${empty str}<br>
条件?表达式1:表达式2|三元运算符| ${1 > 2? 0 : 1}

## 4.域对象
JavaWeb中有四大域对象，分别是：
1. page：当前页面有效
2. request：当前请求有效
3. session：当前会话有效
4. application：当前应用有效

el 表达式获取数据，会依次从这4个域中寻找,按照page → request → session → application的作用域顺序依次查找，找到即返回，最终找不到返回null
## 5.使用EL表达式获取请求数据
**获取数据**
1. 普通字符串数据 ${键名}
2. 对象数据 ${键名.属性名}

**集合数据**
1. list集合 ${键名[角标]}
2. Map集合 ${map集合作用域存储的键名.map集合存储的数据的键名}

**指定作用域获取数据**
1. ${pageScope.键名} 指明获取pageContext作用域中的数据
2. ${requestScope.键名} 指明获取request作用域中的数据
3. ${sessionScope.键名} 指明获取session作用域中的数据
4. ${applicationScope.键名} 指明获取application作用域中的数据

**获取请求实体中同键不同值的数据**
1. ${param.键名} 获取请求实体中一个键一个值的数据
2. ${paramValues.键名} 获取请求实体中同键不同值的数据，返回的是String数组，可以使用角标直接获取

**获取请求头数据**
1. ${header} 返回所有的请求头数据，键值对形式
2. ${header["键名"]} 返回指定的键的请求头数据
3. ${headerValues["键名"]}

**获取Cookie数据**
1. ${cookie} 获取所有的Cookie对象 键值对
2. ${cookie.Cookie对象的键名} 获取存储了指定Cookie数据的Cookie对象
3. ${cookie.Cookie对象的键名.name} 获取存储了指定Cookie数据的Cookie对象的存储的键
4. ${cookie.Cookie对象的键名.value}获取存储了指定Cookie数据的Cookie对象的存储的值



# 八、JSTL
## 1.JSTL简介
**概述：** JSTL（JavaServer Pages Standard Tag Library，JSP标准标签库）是一个不断完善的开放源代码的JSP标签库，它提供了一系列标准标签，用于简化JSP页面的开发，提高程序的可读性和可维护性。

**作用：** 用于简化和替换jsp页面上的java代码

**使用步骤：**
1. 导入jstl相关jar包
2. 引入标签库
3. 使用标签库
## 2.JSTL核心标签库
JSTL包含四大库，但其中FMT,SQL和XML库已经过时，不再推荐使用。目前主要使用的是核心库(core).
**核心库（core）：** 提供了基本的控制结构、迭代、条件判断等标签

**核心库（core）常用标签**
标签名称|功能分类|作用
-|-|-
`<标签名:if>`|流程控制|用于条件判断
`<标签名:choose>`<br>`<标签名:when>`<br>`<标签名:otherwise>`|流程控制|用于多条件判断
`<标签名:forEach>`|迭代遍历|用于循环遍历
## 3.JSTL使用

**if语句**
相当于java代码的if语句

**属性：**
test 必须属性，接受boolean表达式        
如果表达式为true，则显示if标签体内容，如果为false，则不显示标签体内容        
一般情况下，test属性值会结合el表达式一起使用

**注意：**
 c:if标签没有else情况，想要else情况，则可以在定义一个c:if标签

**样例**

```html
<%
   List list = new ArrayList<>();
   list.add("a");
   list.add("b");
   list.add("c");
   request.setAttribute("list",list);
%>

<c:if test="${empty list}">
   <h1>空</h1>
</c:if>

<c:if test="${not empty list}">
   <h1>不为空</h1>
</c:if>
```


**choose语句**
choose:相当于java代码的switch语句        
1. 使用choose标签声明，相当于switch声明       
2. 使用when标签做判断，相当于case       
3. 使用otherwise标签做其他情况的声明，相当于default

```html
<%
    request.setAttribute("num",3);
%>

<c:choose>
    <c:when test="${num==1}">周一</c:when>
    <c:when test="${num==2}">周二</c:when>
    <c:when test="${num==3}">周三</c:when>
    <c:when test="${num==4}">周四</c:when>
    <c:when test="${num==5}">周五</c:when>
    <c:when test="${num==6}">周六</c:when>
    <c:when test="${num==7}">周日</c:when>
    <c:otherwise>数字有误!</c:otherwise>
</c:choose>
```

**foreache语句：**
foreach:相当于java代码的for语句
属性值|介绍
-|-
begin|开始值    
end|结束值    
var|临时变量    
step|步长  
varStatus|循环状态对象   
index|容器中元素的索引，从0开始     
count|循环次数，从1开始

**样例：**
```html
<c:forEach begin="1" end="10" var="i" step="1">
    ${i}
</c:forEach>
 <c:forEach begin="1" end="10" var="i" step="3" varStatus="s">
   ${i} <span>${s.index}</span> <span>${s.count}</span><br>
</c:forEach>
```
# 九、Cookie
## 1.客户端会话跟踪技术概述
**会话:**
1. 用户打开浏览器，访问web服务器的资源，会话建立，直到有一方断开连接，会话结束。在一次会话中可以包含多次请求和响应。
2. 从浏览器发出请求到服务端响应数据给前端之后，一次会话(在浏览器和服务器之间)就被建立了
3. 会话被建立后，如果浏览器或服务端都没有被关闭，则会话就会持续建立着
4. 浏览器和服务器就可以继续使用该会话进行请求发送和响应，上述的整个过程就被称之为会话。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/282f5668410c4a6e84bd8b45ad45f075.png)


**会话跟踪:**
1. 一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于同一浏览器，以便在同一次会话的多次请求间共享数据。
2. 服务器会收到多个请求，这多个请求可能来自多个浏览器
3. 服务器需要用来识别请求是否来自同一个浏览器
4. 服务器用来识别浏览器的过程，这个过程就是会话跟踪
5. 服务器识别浏览器后就可以在同一个会话中多次请求之间来共享数据


**为什么现在浏览器和服务器不支持数据共享：**

1. 浏览器和服务器之间使用的是HTTP请求来进行数据传输
2. HTTP协议是无状态的，每次浏览器向服务器请求时，服务器都会将该请求视为新的请求
3. HTTP协议设计成无状态的目的是让每次请求之间相互独立，互不影响
4. 请求与请求之间独立后，就无法实现多次请求之间的数据共享

**会话跟踪技术的实现：**
1. 客户端会话跟踪技术：Cookie
2. 服务端会话跟踪技术：Session

**这两个技术都可以实现会话跟踪，它们之间最大的区别:Cookie是存储在浏览器端而Session是存储在服务器端**
## 2.Cookie的基本使用
### 概念
Cookie：客户端会话技术，将数据保存到客户端，以后每次请求都携带Cookie数据进行访问。
**对于Cookie的操作主要分两大类**
1. 发送Cookie 
2. 获取Cookie
### 工作流程

![请添加图片描述](https://i-blog.csdnimg.cn/direct/7e2c28ce37884d1abd4ce14f2cc1dac5.png)



1. 服务端提供了两个Servlet，分别是ServletA和ServletB
2. 浏览器发送HTTP请求1给服务端，服务端ServletA接收请求并进行业务处理
3. 服务端ServletA在处理的过程中可以创建一个Cookie对象并将name=cxk的数据存入Cookie
4. 服务端ServletA在响应数据的时候，会把Cookie对象响应给浏览器
5. 浏览器接收到响应数据，会把Cookie对象中的数据存储在浏览器内存中，此时浏览器和服务端就建立了一次会话
6. 在同一次会话中浏览器再次发送HTTP请求2给服务端ServletB，浏览器会携带Cookie对象中的所有数据
7. ServletB接收到请求和数据后，就可以获取到存储在Cookie对象中的数据，这样同一个会话中的多次请求之间就实现了数据共享

### Cookie使用
 **发送Cookie**
创建Cookie对象，并设置数据
```java
Cookie c=new Cookie(String name, String value);
```
发送Cookie到客户端：使用response对象
```java
resp.addCookie(cookie)
```

**样例:**
```java
@WebServlet("/myCookie1")
public class MyCookie1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //发送Cookie
        //1.创建Cookie对象  绑定数据key:value
        Cookie cookie = new Cookie("msg","HelloCookie");
        //2.通过response发送cookie对象
        resp.addCookie(cookie);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}
```

**浏览器中查看**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d666cfd82b974ebab64aaf2c853aa74a.png =600x) 

---
 **获取Cookie**
1. 获取客户端携带的所有Cookie，使用request对象
```java
Cookie[] cookies = request.getCookies();
```
2. 遍历数组，获取每一个Cookie对象
3. 使用Cookie对象方法获取数据
```java
 getName() //获取对象名称
 getValue() //获取对象值
```

**样例**
```java
@WebServlet("/myCookie2")
public class MyCookie2 extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie[] cookies = req.getCookies();
        for (Cookie cookie : cookies) {
            String name = cookie.getName();
            String value = cookie.getValue();
            System.out.println(name + "->" + value);
        }
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}
```

控制台信息

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7cf33541d0174a5aa644361461243aea.png)

**JSESSIONID:** 当用户第一次访问服务器时，服务器会调用request.getSession()方法创建会话，并生成一个唯一的JSESSIONID。这个ID会被作为Cookie的一部分发送给客户端浏览器。浏览器在接收到JSESSIONID后，会将其保存在内存中（作为内存Cookie）。在后续的请求中，浏览器会自动将这个Cookie包含在请求头中发送给服务器。服务器在接收到请求后，会根据请求头中的JSESSIONID来识别用户并检索相应的会话信息。

## 3.Cookie的原理分析

![请添加图片描述](https://i-blog.csdnimg.cn/direct/f51012422f9a49959d6186bcdca9be12.png)

1. ServletA给前端发送Cookie,ServletB从request中获取Cookie的功能
2. 对于ServletA响应数据的时候，Tomcat服务器都是基于HTTP协议来响应数据
3. 当Tomcat发现后端要返回的是一个Cookie对象之后，Tomcat就会在响应头中添加一行数据 **Set-Cookie:username=cxk**
4. 浏览器获取到响应结果后，从响应头中就可以获取到**Set-Cookie**对应值**username=cxk**,并将数据存储在浏览器的内存中
5. 浏览器再次发送请求给ServletB的时候，浏览器会自动在请求头中添加**Cookie: username=cxk**发送给服务端ServletB
6. Request对象会把请求头中cookie对应的值封装成一个个Cookie对象，最终形成一个数组
7. ServletB通过Request对象获取到Cookie[]后，就可以从中获取自己需要的数据
## 4.Cookie使用细节
**Cookie的存活时间:**
正常情况，Cookie存储在浏览器内存中，浏览器关闭，内存释放，则Cookie被销毁，现在想将Cookie持久化存储，则用到API
```java
setMaxAge(int seconds)
```
**参数值为**
1. 正数：将Cookie写入浏览器所在电脑的硬盘，持久化存储。到时间自动删除
2. 负数：默认值，Cookie在当前浏览器内存中，当浏览器关闭，则Cookie被销毁
3. 零：删除对应Cookie

**Cookie存储中文:**
Cookie不能直接存储中文

**储存中文实现思路为：**
1. 在ServletB中对中文进行URL编码，采用URLEncoder.encode()，将编码后的值存入Cookie中
2. 在ServletB中获取Cookie中的值,获取的值为URL编码后的值
3. 将获取的值在进行URL解码,采用URLDecoder.decode()，就可以获取到对应的中文值

**样例：**
```java
		//发送Cookie
        String value = "我家哥哥";
        //对中文进行URL编码
        value = URLEncoder.encode(value, "UTF-8");
        //将编码后的值存入Cookie中
        Cookie cookie = new Cookie("name",value);
        //设置存活时间   ，1周 7天
        cookie.setMaxAge(60*60*24*7);
        //2. 发送Cookie，response
        response.addCookie(cookie);

//----------------------------------------------------------------------------------------
		//获取Cookie        
		//1. 获取Cookie数组        
		Cookie[] cookies = request.getCookies();
		//2. 遍历数组        
		for (Cookie cookie : cookies) {
			//3. 获取数据
			String name = cookie.getName();
			if("name".equals(name)){
				//获取URL编码后的值       
				String value = cookie.getValue();
				//URL解码                
				value = URLDecoder.decode(value,"UTF-8");
				System.out.println(name+":"+value);  
			}
		}          		            	
```
## 5.cookie中常用属性
属性名称|说明
-|-
Name|这个是cookie的名字
Value|这个是cooke的值
Path|这个定义了Web站点上可以访问该Cookie的目录
Expires|这个值表示cookie的过期时间，也就是有效值，cookie在这个值之前都有效。
Size|这个表示cookie的大小
# 十、Session
## 1.服务端会话跟踪技术概述
基本在Cookie中介绍过了，最大的区别就是Cookie是存储在浏览器端而Session是存储在服务器端

session就是一个存储于服务器的特殊对象，通过session可以实现数据共享，session有一个JSESSIONID，这个是session的唯一标识，使用它可以查找到session。session是会话级别的，对于每一个客户端来说是独享它所拥有的session的，我们使用session在进行页面跳转时，服务端可以利用session进行数据共享。session由服务器进行控制。session的创建和销毁都是服务器进行管理的。服务器会为每一个客户端创建一个session。

## 2.Session的基本使用
### 概念
服务端会话跟踪技术：将数据保存到服务端。

1. Session是存储在服务端而Cookie是存储在客户端
2. 存储在客户端的数据容易被窃取和截获，存在很多不安全的因素
3. 存储在服务端的数据相比于客户端来说就更安全


### 工作流程

![请添加图片描述](https://i-blog.csdnimg.cn/direct/73236b073fcf4122933acb77346fff95.png)
1. 在服务端的ServletA获取一个Session对象，把数据存入其中
2. 在服务端的ServletB获取到相同的Session对象，从中取出数据
3. 就可以实现一次会话中多次请求之间的数据共享了
4. 现在最大的问题是如何保证ServletA和ServletB使用的是同一个Session对象

### Session使用
**获取Session对象**
```java
HttpSession session = req.getSession();
```
**Session对象提供的功能**
方法名|说明
-|-
void setAttribute(String name, Object o)|存储数据到 session 域中
Object getAttribute(String name)|根据 key，获取值
void removeAttribute(String name)|根据 key，删除该键值对

**样例：**
```java
//储存数据到session域中
@WebServlet("/mySession1")
public class MySession1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
        //存储到Session中
        //1. 获取Session对象
        HttpSession session = request.getSession();
        //2. 存储数据
        session.setAttribute("name","cxk");
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
        this.doGet(request, response);
    }
}

//获取数据
@WebServlet("/mySession2")
public class MySession2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
        //获取数据，从session中
        //1. 获取Session对象
        HttpSession session = request.getSession();
        //2. 获取数据
        Object name = session.getAttribute("name");
        System.out.println("name ->" +  name);
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
        this.doGet(request, response);
    }
}
//打开浏览器先访问mySession1在访问mySession2然后观察控制台
//name ->cxk
```
## 3.Session的原理分析

![请添加图片描述](https://i-blog.csdnimg.cn/direct/1f643f67e3284949aa7ef7f60df55021.png)


1. ServletA在第一次获取session对象的时候，session对象会有一个唯一的标识，假如是id:10
2. ServletB在session中存入其他数据并处理完成所有业务后，需要通过Tomcat服务器响应结果给浏览器
3. Tomcat服务器发现业务处理中使用了session对象，就会把session的唯一标识id:10当做一个cookie，添加Set-Cookie:JESSIONID=10到响应头中，并响应给浏览器
4. 浏览器接收到响应结果后，会把响应头中的coookie数据存储到浏览器的内存中
5. 浏览器在同一会话中访问ServletB的时候，会把cookie中的数据按照cookie: JESSIONID=10的格式添加到请求头中并发送给服务器Tomcat
6. ServletB获取到请求后，从请求头中就读取cookie中的JSESSIONID值为10，然后就会到服务器内存中寻找id:10的session对象，如果找到了，就直接返回该对象，如果没有则新创建一个session对象
7. 关闭打开浏览器后，因为浏览器的cookie已被销毁，所以就没有JESSIONID的数据，服务端获取到的session就是一个全新的session对象
## 4.Session的使用细节
**钝化**
**Session钝化是指将内存中的Session对象序列化到磁盘上的过程**。这个过程通常发生在以下情况：
1. 服务器正常关闭：当服务器正常关闭时，为了保留当前用户的会话状态，会将还未超时的Session对象钝化到磁盘上。这样，当服务器再次启动时，可以从磁盘上读取这些Session对象并恢复到内存中，从而保持用户的登录状态和数据不丢失。
2. 内存压力：当服务器内存压力较大时，为了释放内存资源，可以将一部分不活跃的Session对象钝化到磁盘上。这样，当需要这些Session对象时，再从磁盘上加载到内存中。

**活化**
**Session活化是指将磁盘上的Session对象反序列化到内存中的过程**。这个过程通常发生在服务器启动时或需要访问某个已钝化的Session对象时。
1. 服务器启动：当服务器启动时，会读取磁盘上保存的Session对象，并将其反序列化到内存中。这样，用户再次访问应用时，可以保持之前的会话状态。
2. 访问已钝化的Session：当需要访问某个已钝化的Session对象时，服务器会将其从磁盘上加载到内存中，并进行反序列化操作。

---
**Session的钝化与活化样例解析**
```java
//MySession3 
@WebServlet("/mySession3")
public class MySession3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取Session对象
        HttpSession session = req.getSession();
        //存储数据
        session.setAttribute("name", "cxk");
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doPost(req, resp);
    }
}

//MySession4 
@WebServlet("/mySession4")
public class MySession4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取到username 输出到浏览器
        Object name = req.getSession().getAttribute("name");
        System.out.println(name);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doPost(req, resp);
    }
}
```
1. 访问mySession4在访问mySession4可以获取到到name的值，然后正常关闭Tomcat服务器，非正常关闭或Session超时浏览器会报500的错误

2. 让服务器正常关闭，在Tomcat 中的work 目录下，会多出一个`SESSIONS.ser` 文件
 ![请添加图片描述](https://i-blog.csdnimg.cn/direct/7b9ea4c7104e4284b4a809eb08bab200.png)
 3. 里面存储着还未过期的Session 信息，这就是Session的钝化，以文件的形式保存在本地磁盘中
 4. 当再次启动Tomcat服务器时 直接访问mySession4 也会获取到name的值cxk ，同时数据加载到Session中后，路径中的SESSIONS.ser文件会被删除掉，这就是Session的活化

**销毁**
## 5.Cookie和Session的关系和区别
Cookie 和 Session 都是来完成一次会话内多次请求间数据共享的。

区别|说明
-|- 
存储位置|Cookie 是将数据存储在客户端，Session 将数据存储在服务端
安全性|Cookie不安全，Session安全
数据大小|Cookie最大4KB，Session无大小限制
存储时间|Cookie可以通过setMaxAge()长期存储，Session默认30分钟
服务器性能|Cookie不占服务器资源，Session占用服务器资源

Cookie是用来保证用户在未登录情况下的身份识别
Session是用来保存用户登录后的数据


# 十一、Filter(过滤器)
## 1.Filter简介
过滤器，顾名思义就是对事物进行过滤的，在Web中的过滤器，当然就是对请求进行过滤，我们使用过滤器，就可以对请求进行拦截，然后做相应的处理，实现许多特殊功能。如登录控制，权限管理，过滤敏感词汇等。

## 2.Filter生命周期
### 生命周期简介
**初始化：** 服务器启动时，它会创建Filter的实例对象，并调用其init方法进行初始化。这个方法只会执行一次。
**拦截请求：** 服务器运行期间，每次有请求到达时，都会调用Filter的doFilter方法对请求进行拦截和处理。
**销毁：** 服务器关闭时，它会销毁Filter的实例对象，并调用其destroy方法进行资源清理。这个方法也只会执行一次。


### Filter生命周期接口
接口名称|说明
-|-
init(FilterConfig filterConfig)|在Filter被初始化时调用，用于进行一些初始化操作。例如，读取配置参数、创建必要的资源等。
doFilter(ServletRequest req，ServletResponse resp，FilterChain chain)|对HTTP请求和响应进行拦截和处理的核心方法。当一个 Filter 对象能够拦截访问请求时，Servlet 容器将调用 Filter 对象的 doFilter 方法。
destroy()|在Filter被销毁时调用，用于进行一些清理操作。例如，释放资源、关闭文件等。



## 3.Filter的基本工作原理

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/96974922cf9547adab759d2ecede3995.png)


1. 拦截请求：当Web服务器接收到一个HTTP请求时，它会检查是否有Filter对该请求进行拦截。如果有，那么Web服务器会先调用Filter的doFilter方法，而不是直接调用Servlet的service方法。
2. 处理请求：在doFilter方法中，开发人员可以编写代码来对请求进行处理。这包括检查请求参数、修改请求头、执行权限验证等操作。
3. 继续处理：处理完请求后，Filter会将请求传递给下一个Filter（如果有多个Filter）或Servlet。这通过调用FilterChain对象的doFilter方法来实现。
4. 拦截响应：同样地，当Servlet生成响应并准备发送给客户端时，Filter也可以对响应进行拦截和处理。这包括修改响应内容、设置响应头等操作。
5. 发送响应：处理完响应后，Filter会将响应发送给客户端。
## 4.Filter链(FilterChain)
1. 一个 Web 应用程序中可以注册多个 Filter 程序，每个 Filter 程序都可以对一个或一组 Servlet 程序进行拦截。如果有多个 Filter 程序都可以对某个 Servlet 程序的访问过程进行拦截，当针对该 Servlet 的访问请求到达时，Web 容器将把这多个 Filter 程序组合成一个 Filter 链（也叫过滤器链）。

2. Filter 链中的各个 Filter 的拦截顺序与它们在 web.xml 文件中的映射顺序一致，上一个 Filter.doFilter 方法中调用 FilterChain.doFilter 方法将激活下一个 Filter的doFilter 方法，最后一个 Filter.doFilter 方法中调用的 FilterChain.doFilter 方法将激活目标 Servlet的service 方法。

3. 只要 Filter 链中任意一个 Filter 没有调用 FilterChain.doFilter 方法，则目标 Servlet 的 service 方法都不会被执行
## 5.如何使用Filter
**(1) 添加依赖**
```xml
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>4.0.1</version>
  <scope>provided</scope>
</dependency>
```
**(2) 创建过滤器**
```java
public class MyFilter1 implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("MyFilter1正在创建");
    }
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("MyFilter1正在执行过滤请求方法");
    }
    @Override
    public void destroy() {
        System.out.println("MyFilter1正在销毁");
    }
}
```
**(3) 配置拦截路径（web.xml方式）**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml version="1.0" encoding="UTF-8"?>
<web-app
        version="4.0"
        xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:javaee="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:xml="http://www.w3.org/XML/1998/namespace"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd">
    <display-name>Archetype Created Web Application</display-name>

    <display-name>Filter</display-name>

    <filter>
        <filter-name>myFilter1</filter-name>
        <filter-class>com.chq.filter.MyFilter1</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>myFilter1</filter-name>
        <!--针对于所有的请求都过滤-->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```
**(4) 使用注解方式（@WebFilter）**
```java
@WebFilter("/*")
public class MyFilter2 implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("正在初始化MyFilter2");
    }
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("MyFilter2正在执行过滤方法");
        //放行请求
        filterChain.doFilter(servletRequest,servletResponse);
		
		System.out.println("MyFilter2正在执行响应方法");
    }
    @Override
    public void destroy() {
        System.out.println("正在销毁MyFilter2");
    }
}
```

**关于@WebFilter** 
属性名称|说明
-|-
filterName|过滤器的名称，是一个可选属性。它可以用于在配置文件或其他地方引用该过滤器。
urlPatterns|过滤器要过滤的URL模式，是一个必需属性。可以是一个字符串数组，表示多个URL模式。例如，“/*”表示过滤所有请求。
value|urlPatterns的别名属性，可以用来指定过滤器要过滤的URL模式。
description|过滤器的描述信息，是一个可选属性。
dispatcherTypes|过滤器要过滤的请求类型，是一个可选属性。默认为REQUEST，还可以是其他类型，如FORWARD、INCLUDE、ERROR、ASYNC等。
asyncSupported|是否支持异步操作，是一个可选属性。默认为false。
initParams|过滤器的初始化参数，是一个可选属性。以`@WebInitParam`注解的数组形式提供。
servletNames|指定过滤器拦截的Servlet名称，可以是一个字符串数组，表示多个Servlet名称

## 6.FilterConfig和FilterChain
**FilterConfig 接口**

1. 与普通的 Servlet 程序一样，Filter 程序也很可能需要访问 Servlet 容器。Servlet 规范将代表 ServletContext 对象和 Filter 的配置参数信息都封装到一个称为 FilterConfig 的对象中。
 2. FilterConfig 接口则用于定义 FilterConfig 对象应该对外提供的方法，以便在 Filter 程序中可以调用这些方法来获取 ServletContext 对象，以及获取在 web.xml 文件中为 Filter 设置的友好名称和初始化参数。
3.  FilterConfig接口定义的各个方法：
 
 方法名|说明
 -|-
  getFilterName()|返回` <filter-name> `元素的设置值。
  getServletContext ()|返回 FilterConfig 对象中所包装的 ServletContext 对象的引用。
  getInitParameter()|用于返回在` web.xml 文件中为 Filter 所设置的某个名称的初始化的参数值。
  getInitParameterNames()|返回一个 Enumeration 集合对象。

**FilterChain 接口**

1. 该接口用于定义一个 Filter 链的对象应该对外提供的方法，这个接口只定义了一个 doFilter 方法。
2. FilterChain 接口的 doFilter 方法用于通知 Web 容器把请求交给 Filter 链中的下一个 Filter 去处理，如果当前调用此方法的 Filter 对象是Filter 链中的最后一个 Filter，那么将把请求交给目标 Servlet 程序去处理。
## 7.Filter 的注册与映射(了解)
**注册 Filter**
 一个 `<filter>` 元素用于注册一个 Filter。其中，`<filter-name> `元素是必需的，`<filter-class> `元素也是必需的，`<init-param>` 元素是可选的，可以有多个 `< init-param>` 元素。
```xml
 <filter>
    <filter-name>myFilter1</filter-name>
    <filter-class>com.chq.filter.MyFilter1</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
```

**映射Filter**
`<filter-mapping>` 元素用于设置一个 Filter 所负责拦截的资源。一个 Filter 拦截的资源可以通过两种方式来指定：资源的访问请求路径和 Servlet 名称。

**指定资源的访问路径**
```xml
<filter-mapping>
    <filter-name>myFilter1</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```
1. `<url-pattern> `元素中的访问路径的设置方式遵循 Servlet 的 URL 映射规范。
2. `/*`：表示拦截所有的访问请求。
3. `/filter/*`：表示拦截 filter 目录下的所有访问请求，如：http://localhost:080/Filter/filter/xxxxxx
4. `/test.html`：表示拦截根目录下以 test.html 为资源名的访问请求，访问链接只会是：http://localhost:8080/test.html。

**指定Servlet的名称**
```xml
<filter-mapping>
    <filter-name>myFilter1</filter-name>
    <!--对客户端请求的静态资源交由默认的servlet进行处理-->
    <servlet-name>default</servlet-name>
    <dispatcher>INCLUDE</dispatcher>
    <dispatcher>REQUEST</dispatcher>
</filter-mapping>
```
1. `<servlet-name> `元素与 `<url-pattern>` 元素是二选一的关系，其值是某个 Servlet 在 web.xml 文件中的注册名称。
2. `<dispatcher>` 元素的设置值有4种:REQUEST、INCLUDE、FORWARD、ERROR，分别对应Servlet容器调用资源的4种方式：
 通过正常的访问请求调用；
 通过 RequestDispatcher.include 方法调用；
 通过 RequestDispatcher.forward 方法调用；
 作为错误响应资源调用。
3. `<dispatcher>`元素的值决定了过滤器的应用范围。其中，REQUEST是`<dispatcher>`元素的一个可能值，它表示过滤器应该拦截客户端直接发起的请求


**样例设置字符编码：**
```java
@WebFilter(filterName = "MyFilter3 ", urlPatterns = "/*")  
public class MyFilter3 implements Filter {    
    @Override  
    public void init(FilterConfig filterConfig) throws ServletException {  
    }  
    @Override  
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)  
            throws IOException, ServletException {  
        // 设置请求和响应的字符编码  
        request.setCharacterEncoding("UTF-8");  
        response.setCharacterEncoding("UTF-8");  
        response.setContentType("text/html;charset=UTF-8");  
        // 将请求和响应传递给下一个过滤器或目标资源  
        chain.doFilter(request, response);  
    }
    @Override  
    public void destroy() {   
    }  
}
```
如果不用注解@WebFilter  ，那么web.xml配置文件如下
```xml
<filter>  
    <filter-name>MyFilter3 </filter-name>  
    <filter-class>com.chq.filter.MyFilter3 </filter-class>  
    <init-param>  
        <param-name>encoding</param-name>  
        <param-value>UTF-8</param-value>  
    </init-param>  
</filter>  
<filter-mapping>  
    <filter-name>MyFilter3 </filter-name>  
    <url-pattern>/*</url-pattern>  
</filter-mapping>
```
# 十二、Listener（监听器）
## 1.Listener简介
Listener是Java Servlet规范中的一部分，它提供了一种机制，使得开发者能够在不修改源代码的情况下，通过监听器对现有应用程序进行扩展或增强。Listener通常以接口的形式存在，当特定事件发生时，Listener会接收到通知并执行相应的操作。

简单来说，**当被监视的对象发生情况时，监听器会立即采取相应的行动。**

监听器功能|说明
-|-
监听器|监听事件对象
事件对象|被监听的对象
注册监听器|将监听器与事件进行绑定
响应行为|监听器监听到事件源的状态变化时所涉及的功能代码


## 2.Listener的类型
**常见的监听器类型**
类型名称|说明
-|-
ServletContextListener|用于监听Web应用程序的启动和关闭事件。
HttpSessionListener|用于监听会话的创建和销毁事件。
ServletRequestListener| 用于监听请求的创建和销毁事件。

## 3.Listener的生命周期

1. **ServletContextListener**
contextInitialized(ServletContextEvent sce)|在应用程序启动时，会调用contextInitialized方法进行初始化操作；
contextDestroyed(ServletContextEvent sce)|在应用程序关闭时，会调用contextDestroyed方法进行清理操作。
2. **HttpSessionListener**
sessionCreated(HttpSessionEvent se)|在会话创建时，会调用sessionCreated方法进行初始化操作；
sessionDestroyed(HttpSessionEvent se)|在会话销毁时，会调用sessionDestroyed方法进行清理操作。
3. **ServletRequestListener**
requestInitialized(ServletRequestEvent sre)|在请求创建时，会调用requestInitialized方法进行初始化操作；
requestDestroyed(ServletRequestEvent sre)|在请求销毁时，会调用requestDestroyed方法进行清理操作。

## 4.监听器的配置与注册
在Java Web应用程序中，监听器需要通过在web.xml配置文件中声明来启用。开发者需要在web.xml文件中添加相应的监听器声明，告诉Web容器要监听哪些事件，并指定相应的监听器类。此外，从Servlet 3.0规范开始，还可以使用注解的方式来配置监听器，例如使用@WebListener注解来标注一个类为监听器类。

**使用web.xml进行配置**
```xml
<listener>
   <!--注册监听器-->
   <listener-class>com.chq.listener.MyListener1</listener-class>
</listener>
```
**使用注解进行配置**
```java
@WebListener
```

## 5.如何使用Listener
这里会用三个例子分别介绍HttpSessionListener，ServletContextListener，ServletRequestListener的使用

**1. HttpSessionListener**
**统计网站在线人数**
```java
//使用注解方式进行配置
@WebListener
public class MyListener1 implements HttpSessionListener {
    @Override
    public void sessionCreated(HttpSessionEvent se) {
        System.out.println("hello sessionCreated");
        ServletContext sc = se.getSession().getServletContext();
        Integer onlineCount = (Integer) sc.getAttribute("OnlineCount");
        if (onlineCount == null) {
            onlineCount = 1;
        } else {
            onlineCount = onlineCount.intValue() + 1;
        }
        System.out.println(onlineCount);
        sc.setAttribute("OnlineCount", onlineCount);
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        System.out.println("hello sessionDestroyed");
        ServletContext sc = se.getSession().getServletContext();
        Integer onlineCount = (Integer) sc.getAttribute("OnlineCount");
        //这里一定不为空所有不用判断
        onlineCount = onlineCount.intValue() - 1;
        sc.setAttribute("OnlineCount", onlineCount);
    }
}
```
**2. ServletContextListener**
**服务启动时加载数据库数据到内存**

```java
@WebListener
public class MyListener2 implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("hello contextInitialized");
        ServletContext sc = sce.getServletContext();
        Map<Integer, String> students = new HashMap<>();
        PreparedStatement pstm = null;
        // 假设有一个数据库连接工具类ConnectTool 这里可以参考JDBC的那篇内容
        try (Connection connection = ConnectTool.getConnection()) {
            String sql = "select name,age from students";
            pstm = connection.prepareStatement(sql);
            ResultSet rs = pstm.executeQuery();
            while (rs.next()) {
                students.put(rs.getInt(1), rs.getString(2));
            }
            // 将所取到的值存放到一个属性键值对中
            sc.setAttribute("students", students);
            // 关闭资源
            connection.close();
            pstm.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("hello contextDestroyed");
    }
}
```
**3. ServletRequestListener**
**Request对象的创建和销毁日志记录**
```java
@WebListener
public class MyListener3 implements ServletRequestListener{
    @Override
    public void requestInitialized(ServletRequestEvent sre) {
        System.out.println("hello requestInitialized");
        ServletRequest servletRequest = sre.getServletRequest();
        System.out.println(servletRequest + " 被创建");
    }

    @Override
    public void requestDestroyed(ServletRequestEvent sre) {
        System.out.println("hello requestDestroyed");
        System.out.println(sre.getServletRequest() + " 被销毁");
    }
}
```
