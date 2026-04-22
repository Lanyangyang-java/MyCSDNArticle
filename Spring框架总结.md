@[TOC](Spring框架总结 超详细 易理解 我的学习笔记)

---
# 一、Spring框架简介
## 1.Spring框架介绍
**Spring框架是一个轻量级的Java开发开源框架，它旨在解决企业应用开发的复杂性，并为开发人员提供一个高效、灵活的开发环境。**

**Spring之父：Rod Johnson(罗德.约翰逊) 他是悉尼大学音乐学博士，而计算机仅仅是学士学位。 由于Rod对JAVAEE笨重、臃肿的现状深恶痛绝，以至于他将他在JAVAEE实战中的经历称为噩梦般的经历。他决定改变这种现状，于是就有了Spring。**

---
## 2.Spring体系结构
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6076c403bc99402abcb309454a340cf8.png)


**(1)核心层**
* **Core Container:核心容器，这个模块是Spring最核心的模块，其他的都需要依赖该模块，存放对象**

**(2)AOP层**
* **AOP:面向切面编程，它依赖核心层容器，目的是在不改变原有代码的前提下对其进行功能增强**
* **Aspects:AOP是思想,Aspects是对AOP思想的具体实现**

**(3)数据层**
* **Data Access:数据访问，Spring全家桶中有对数据访问的具体实现技术**
* **Data Integration:数据集成，Spring支持整合其他的数据层解决方案，比如Mybatis**
* **Transactions:事务，Spring中事务管理是Spring AOP的一个具体实现，也是后期学习的重点内容**

**(4)Web层**
* **Web开发**

**(5)Test层**
* **Spring主要整合了Junit来完成单元测试和集成测试**

---
## 3.Spring核心概念
>**DI:依赖注入（Dependency Injection）**
>**AOP:面向切面编程（Aspect Oriented Programming）**
>**IOC容器:Spring创建了一个容器用来存放所创建的对象，这个容器就叫IOC容器**
>**Bean:容器中所存放的一个个对象就叫Bean或Bean对象**


# 二、IOC和DI
## 1.IOC(Inversion of Control)控制反转
### 样例：理解控制反转思想
>**我们先用我们原来的方式写一段代码** 

---
**(1) 先写一个StudentMapper接口和StudentMapper实现类**
```java
public interface StudentMapper {
    void insert();
}
-------------------------------
public class StudentMapperImpl implements StudentMapper {
    @Override
    public void insert() {
        System.out.println("新增一位学生通过Mapper");
    }
}
```

---
**（2）在写一个StudentService接口和Student实现类**
```java
public interface StudentService {
    void save();
}
------------------------------
public class StudentServiceImpl implements StudentService {
    private StudentMapper studentMapper = new StudentMapperImpl();

    @Override
    public void save() {
        System.out.println("新增一位学生通过Service");
        studentMapper.insert();
    }
}
```

---
**(3) 测试**
```java
public class StudentTest1 {
    private static StudentServiceImpl studentService = new StudentServiceImpl();;

    public static void main(String[] args) {
        studentService.save();
    }
}
	/**
     * 控制台打印结果
     * 新增一位学生通过Service
     * 新增一位学生通过Mapper
     */
```

>**这是我们原来的方式 , 开始大家也都是这么去写的对吧 . 那我们现在修改一下**

---
**(4) 把StudentMapper的实现类增加一个**
```java
public class StudentMapperByMysql implements StudentMapper {

    @Override
    public void insert() {
        System.out.println("新增学生添加到MySQL数据库中");
    }
}
```

---
**(5) 紧接着我们要去使用MySql的话 , 我们就需要去service实现类里面修改对应的实现**
```java
public class StudentServiceImpl implements StudentService {
    private StudentMapper studentMapper = new StudentMapperByMysql();

    @Override
    public void save() {
        System.out.println("新增一位学生通过Service");
        studentMapper.insert();
    }
}
```

---
 **(6) 在假设, 我们再增加一个StudentMapper的实现类**
```java
public class StudentMapperByOracle implements StudentMapper {
    @Override
    public void insert() {
        System.out.println("新增学生添加到Oracle数据库中");
    }
}
```
>**那么我们要使用Oracle , 又需要去service实现类里面修改对应的实现 . 假设我们的这种需求非常大 , 这种方式就根本不适用了, 甚至烦人每次变动 , 都需要修改大量代码 . 这种设计的耦合性太高了, 牵一发而动全身**

---
**(7) 此时，我们试着加入一个Set方法  不去实现它 , 而是留出一个接口**
```java
public class StudentServiceImpl implements StudentService {
    private StudentMapper studentMapper;

    public void setStudentMapper(StudentMapper studentMapper){
        this.studentMapper = studentMapper;
    }

    @Override
    public void save() {
        System.out.println("新增一位学生通过Service");
        studentMapper.insert();
    }
}
```

---
**(8) 再次进行测试**
```java
public class StudentTest1 {

    private static StudentServiceImpl studentService;

    public static void main(String[] args) {
        studentService = new StudentServiceImpl();
        studentService.setStudentMapper(new StudentMapperByMysql());
        studentService.save();
    }
}
```
>**通过上面的样例我们发现：**
>**程序员是主动创建对象！控制权在程序员手上！**
>**使用了set注入之后，程序不在具有主动性，而是变成了被动的接受对象！**
>**这种思想 , 从本质上解决了问题 , 我们程序员不再去管理对象的创建了 , 更多的去关注业务的实现 . 耦合性大大降低 . 可以更加专注的在业务的实现上，这也就是IOC的原型 !** 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e7f8e3f0b33344aa8bba6fbfe11cab1b.png =600x)
**总的来说：使用对象时，在程序中不主动使用new产生对象，转换为由外部提供对象 !!! 这种实现思想就是Spring的一个核心概念**

---

### 控制反转概述
* **使用对象时，由主动new产生对象转换为由外部提供对象，此过程中对象创建控制权由程序转移到外部，此思想称为控制反转。通俗的讲就是“将new对象的权利交给Spring，我们从Spring中获取对象使用即可**
* **业务层要用数据层的类对象，以前是自己new的**
* **现在自己不new了，交给别人（外部）来创建对象**
* **别人（外部）就反转控制了数据层对象的创建权**
* **这种思想就是控制反转**
---

**Spring和IOC之间的关系**
- **Spring技术对IoC思想进行了实现**
- **Spring提供了一个容器，称为IOC容器，用来充当IoC思想中的“外部”**
- **IOC思想中的别人（外部）指的就是Spring的IOC容器**

---

**IOC容器的作用**
- **IOC容器负责对象的创建、初始化等一系列工作，其中包含了数据层和业务层的类对象**
- **被创建或被管理的对象在IOC容器中统称为`Bean`，IOC容器中放的就是一个个的`Bean`对象**

---
**注意 ! ! !** 
**当IOC容器中创建好service和mapper对象后，程序并不能正确执行，因为service运行需要依赖mapper对象， IOC容器中虽然有service和mapper对象，但是service对象和mapper对象没有任何关系，像这种在容器中建立对象与对象之间的绑定关系就要用到DI（依赖注入）**


## 2.DI(Dependency Injection)依赖注入

### 样例：使用Spring管理绑定对象与对象之间的关系
**(1) 导入Spring依赖**
```xml
	<dependencies>
        <!--导入spring的坐标spring-context，对应版本是5.2.10.RELEASE-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>
    </dependencies>
```
---
**（2）创建StudentMapper接口与实现类**
```java
public interface StudentMapper {
    void insert();
}
-------------------
public class StudentMapperImpl implements StudentMapper {
    @Override
    public void insert() {
        System.out.println("新增一位学生通过Mapper");
    }
}
```
---
**(3) 创建StudentService接口与实现类**
```java
public interface StudentService {
    void save();
}
---------------------------------
public class StudentServiceImpl implements StudentService {
    private StudentMapper studentMapper;

    public void setStudentMapper(StudentMapper studentMapper){
        this.studentMapper = studentMapper;
    }

    @Override
    public void save() {
        System.out.println("新增一位学生通过Service");
        studentMapper.insert();
    }
}
```
---
**(4) 创建Spring配置文件**
>**配置对应类作为Spring管理的bean对象 定义applicationContext.xml配置文件并配置BookServiceImpl**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="studentMapper" class="com.chq.mapper.Impl.StudentMapperImpl"/>
    <bean id="studentService" class="com.chq.service.Impl.StudentServiceImpl">
        <!--配置service与mapper的关系
           property标签：表示配置当前bean的属性
           name属性：表示配置哪一个具体的属性,注意: 这里的name并不是属性 , 而是set方法后面的那部分 , 首字母小写
           ref属性：表示参照哪一个bean,引用另外一个bean , 不是用value 而是用 ref
           ref：引用Spring容器中创建好的对象
           value：具体的值，基本数据类型！
        -->
        <property name="studentMapper" ref="studentMapper"/>
    </bean>

</beans>
```
1. Spring配置文件就相当于一个容器。此容器中负责创建对象，并实现对象与对象之间的装配。
2. java中每一个类都是一个bean。所以上面的bean标签，就是在容器中创建一个java对象。
3. bean标签中的class属性，就是类名； id属性，就是对象名。
4. property标签，是给bean的属性注入其它对象。name属性，就是对象属性名； ref属性，就是给属性注入的对象。（如果想要注入基本数据类型，那么使用value属性）
5. 给bean的属性注入其它对象，默认使用 get/set 方法注入。也可以使用其它方式注入：构造方法注入、P命名空间注入等。

---
**(5) 初始化IOC容器，通过容器获取Bean对象**
```java
public class StudentTest {
    public static void main(String[] args) {
        //创建IOC容器对象,加载Spring核心配置文件
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //2 从IOC容器中获取Bean对象(studentService对象)
        StudentService studentService = (StudentService)applicationContext.getBean("studentService");
        //3 调用Bean对象(BookService对象)的方法
        studentService.save();
    }
    /**
     * 控制台打印结果
     * 新增一个学生通过Service
     * 新增一个Student
     */
}
```

---
### 依赖注入概述
- 在容器中建立bean与bean之间的依赖关系的整个过程，称为依赖注入
- 绑定对象与对象之间的依赖关系

---
**IOC容器中哪些bean之间要建立依赖关系**

>**根据业务需求提前建立好关系，如业务层需要依赖数据层，service就要和mapper建立依赖关系**

---
**IOC和DI的最终目标就是:充分解耦，具体实现:**
- 使用IOC容器管理bean（IOC)
- 在IOC容器内将有依赖关系的bean进行关系绑定（DI）
- 最终结果为:使用对象时不仅可以直接从IOC容器中获取，并且获取到的bean已经绑定了所有的依赖关系.

## 3.依赖自动装配
>**搭建一个测试环境**
```java
public class Cat {
    public void shout() {
        System.out.println("喵喵");
    }
}
public class Dog {
    public void shout() {
        System.out.println("汪汪");
    }
}
@Data
public class User {
    private Cat cat;
    private Dog dog;
    private String name;
}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dog" class="com.chq.pojo.Dog"/>
    <bean id="cat" class="com.chq.pojo.Cat"/>

    <bean id="user" class="com.chq.pojo.User">
        <property name="cat" ref="cat"/>
        <property name="dog" ref="dog"/>
        <property name="name" value="蔡徐坤"/>
    </bean>
</beans>
```

**测试**
```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user = context.getBean("user", User.class);
        user.getCat().shout();
        user.getDog().shout();
        System.out.println(user.getName());
    }
}
```
---
### byName 自动装配
**当一个bean节点带有 autowire byName的属性时。**
1. 将查找其类中所有的set方法名，例如setCat，获得将set去掉并且首字母小写的字符串，即cat。
2. 去spring容器中寻找是否有此字符串名称id的对象。
3. 如果有，就取出注入；如果没有，就报空指针异常

**修改bean配置，增加一个属性  `autowire="byName"`**
```xml
	<!--
        byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的bean id !
    -->
    <bean id="user" class="com.chq.pojo.User" autowire="byName">
        <property name="name" value="蔡徐坤"/>
    </bean>
    <!--
        结果和之前一样
    -->
```
### byType 自动装配
**autowire byType (按类型自动装配)**
>**使用autowire byType首先需要保证：同一类型的对象，在spring容器中唯一。如果不唯一，会报不唯一的异常。**

**再次修改Bean**

```xml

    <bean class="com.chq.pojo.Dog"/>
    <bean class="com.chq.pojo.Cat"/>

    <!--
        在注册一个cat 的bean对象 报错：NoUniqueBeanDefinitionException
    -->
    <!--<bean id="catt" class="com.chq.pojo.Cat"/>-->

	<bean id="user" class="com.chq.pojo.User" autowire="byType">
     	property name="name" value="蔡徐坤"/>
    </bean>

	<!-- 
       byname的时候，需要保证所有bean的id唯一，并且这个bean需要和自动注入的属性set方法的值一致！
       bytype的时候，需要保证有bean的class唯一，并且这个bean需要和自动注入的属性的类型值一致！
    -->
```
### 使用注解实现自动装配
**xml文件开启支持注解配置**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--开启注解的支持-->
    <context:annotation-config/>

    <bean id="dog" class="com.chq.pojo.Dog"/>
    <bean id="cat" class="com.chq.pojo.Cat"/>
    <bean id="user" class="com.chq.pojo.User"/>

</beans>
```
**@Autowired**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/de8e319362d44715a2fa03a37e5a0dcb.png =800x)



**User.class**
```java
@Data
public class User {
    @Autowired
    private Cat cat;
    @Autowired
    private Dog dog;
    @Value("蔡徐坤")
    private String name;
}
//再次测试结果与之前相同
```



>如果@Autowired自动装配环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候，我们可以使用@Qualifier(value="xxx")去配置@Autowired的使用，指定一个唯一的bean对象注入！
```java
@Data
public class User {
    @Autowired
    @Qualifier(value = "cat2")
    private Cat cat;
    @Autowired
    private Dog dog;
    @Value("蔡徐坤")
    private String name;
}
```
---

**组合注解@Resource注解**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/23e8aa71d232439ab1c67fe6ae6846d6.png)


```java
@Data
public class User {
    @Resource(name = "cat2")
    private Cat cat;
    @Resource
    private Dog dog;
    @Value("蔡徐坤")
    private String name;
}
```
**@Resource和@Autowired区别**
- 都是用来自动装配的，都可以放在属性字段上
- @Autowired通过byType的方式实现，而且必须要求这个对象存在！【常用】
- @Resource默认通过byname的方式实现，如果找不到名字，则通过byType实现！如果两个都找不到的情况下，就报错！
- 执行顺序不同：@Autowired通过byType的方式实现，@Resource默认通过byname的方式实现。


---
# 三、Bean的基础配置和实例化
## 1.Bean的基础配置
**定义Spring核心容器管理的对象**

**格式**
```xml
<beans>
	<bean/>
	
	<bean></bean>
</beans>
```
**属性列表**
- **id：** bean的id，使用容器可通过id获取对应bean，在一个容器中id值唯一
- **class：** bean的类型，即配置bean的全限定类名

**示例**
```xml
 <bean id="studentMapper" class="com.chq.mapper.Impl.StudentMapperImpl"/>
 
 <bean id="studentService" class="com.chq.service.Impl.StudentServiceImpl"> </bean>
```
## 2.Bean别名配置
定义bean的别名，可以定义多个，使用`，` 或`;`或者空格

**实列**
```xml
<bean id="studentMapper" name="a,b,c" class="com.chq.mapper.Impl.StudentMapperImpl"/>
```
```java
public class StudentTest {
    public static void main(String[] args) {
        //创建IOC容器对象,加载Spring核心配置文件
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //2 从IOC容器中获取Bean对象(studentService对象)
        StudentMapper studentMapper = (StudentMapper)applicationContext.getBean("a");
        //3 调用Bean对象(BookService对象)的方法
        studentMapper.insert();
    }

    /**
     * 控制台打印结果
     * 新增一位学生通过Mapper
     */
}
```
## 3.Bean作用范围配置
**定义bean的作用范围**
- singleton：单列（默认）
- prototype：非单列

>**scope的取值不仅仅只有singleton和prototype，还有request、session、application、 websocket ，表示创建出的对象放置在web容器(tomcat)对应的位置。比如：request表示保存到request域中。**

**示列**
```xml
<bean id="studentMapper" class="com.chq.mapper.Impl.StudentMapperImpl" scope="prototype"/>
```

```java
public class StudentTest {
    public static void main(String[] args) {
        //创建IOC容器对象,加载Spring核心配置文件
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //2 从IOC容器中获取Bean对象(studentService对象)
        StudentMapper studentMapper1 = (StudentMapper)applicationContext.getBean("studentMapper");
        StudentMapper studentMapper2 = (StudentMapper)applicationContext.getBean("studentMapper");

        //3 两个Bean对象地址不同 则非单列的
        System.out.println(studentMapper1 == studentMapper2);
    }

    /**
     * 控制台打印结果
     * false
     */
}
```
## 4.实例化Bean的三种方式
**bean本质上就是对象，创建bean使用构造方法完成**
1. 构造方法方式
**xml配置**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--构造方法实例化bean-->
    <bean id="studentMapper" class="com.chq.mapper.Impl.StudentMapperImpl"/>

    <!--无参构造方法如果不存在，将抛出异常BeanCreationException-->
    
</beans>
```

**测试**
```java
public class StudentTest2 {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext2.xml");

        StudentMapper studentMapper = (StudentMapper) applicationContext.getBean("studentMapper");

        studentMapper.insert();
    }
}

```

---
2.  静态工厂方式


**静态工厂创建对象**
```java
public class StudentMapperFactory {
    public static StudentMapper getStudentMapper(){
        System.out.println("factory created");
        return new StudentMapperImpl();
    }
}
```
**xml配置**
```xml
<!--使用静态工厂实例化bean-->
    <bean id="studentMapper" class="com.chq.test.StudentMapperFactory" factory-method="getStudentMapper"/>
```
**测试**
```java
public class StudentTest2 {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext2.xml");

        StudentMapper studentMapper = (StudentMapper) applicationContext.getBean("studentMapper");

        studentMapper.insert();
    }
    /**
     * 控制台打印
     * factory created
     * 新增一位学生通过Mapper
     */
}
```

---
3.  实例工厂方式

**实例工厂创建对象**
```java
public class StudentMapperFactory {
    public StudentMapper getStudentMapper(){
        System.out.println("factory created");
        return new StudentMapperImpl();
    }
}
```
**xml配置**
```xml
	<!--实例工厂创建对象-->
    <bean id="studentFactory" class="com.chq.test.StudentMapperFactory"/>

    <bean id="studentMapper" factory-method="getStudentMapper" factory-bean="studentFactory"/>
```

**测试**
```java
public class StudentTest2 {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext2.xml");

        StudentMapper studentMapper = (StudentMapper) applicationContext.getBean("studentMapper");

        studentMapper.insert();
    }
    /**
     * 控制台打印
     * factory created
     * 新增一位学生通过Mapper
     */
}
```
---
# 四、Spring注解开发
**注解，就是替代了在配置文件当中配置步骤而已！更加的方便快捷！** 

>**基本使用**

**1.开启Spring注解包扫描**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!--开启注解的支持-->
    <context:annotation-config/>
    <!--扫描包及其子包下的类中注解-->
    <context:component-scan base-package="com.chq.pojo"/>
</beans>
```
**2. 创建Course类并用@Component注解定义Bean。**
```java
/**
 * 组件 @Component定义 bean
 * 等价于 <bean id="course" class="com.chq.pojo.Course"/>
 */
@Component
public class Course {
    public String name = "蔡徐坤世界巨星";
}
```

**3.测试**

```java
public class CourseTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext3.xml");

        Course course = context.getBean("course", Course.class);

        System.out.println(course.name);
    }
    /**
     * 控制台打印
     * 蔡徐坤世界巨星
     */
}
```

---
>**属性注入**

**Course 类中**
```java
/**
 * 组件 @Component定义 bean
 * 等价于 <bean id="course" class="com.chq.pojo.Course"/>
 */
@Component
public class Course {
    // 相当于配置文件中 <property name="name" value="蔡徐坤世界巨星"/>
    @Value("蔡徐坤世界巨星")
    public String name;
}
```

---
**如果有set方法**
```java
/**
 * 组件 @Component定义 bean
 * 等价于 <bean id="course" class="com.chq.pojo.Course"/>
 */
@Component
public class Course {

    public String name;

    // 相当于配置文件中 <property name="name" value="蔡徐坤世界巨星"/>
    @Value("蔡徐坤世界巨星")
    public void setName(String name){
        this.name = name;
    }
}
```

>**衍生的注解**

**@Component 有几个衍生注解，我们在web开发中，会按照mvc三次架构分层**

- **@Controller：用于表现层bean定义** 
- **@Service：用于业务层bean定义**
- **@Repository：用于数据层bean定义**

 **写上这些注解，就相当于将这个类交给Spring管理装配了。 这四个注解功能都是一样的，都是代表将某个类注册到Spring中，装配Bean。**

---
>**自动装配**

**在Bean的自动装配中有详细解释**
- @Autowired：自动装配通过类型，名字。 如果@Autowired不能唯一自动装配上属性，则需要通过@Qualifier(value="xxx")
- @Nullable：字段标记了这个注解，说明这个字段可以为null；
- @Resource：自动装配通过名字，类型。


---
>**注解作用域**

**@Scope() 注解：设置Bean的作用域。**
- **singleton：默认的，Spring会采用单例模式创建这个对象。一个Bean中该Bean的实例只有一个**
- **prototype：多例模式。每次从容器中获取Bean时，都会创建一个新的实例，即Bean是多实例的**

实现CourseService和他的实现类
```java
public interface CourseService {
}

@Service
@Scope("prototype")
public class CourseServiceImpl implements CourseService {
}
```
**测试**
```java
public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext3.xml");

        Course course1 = context.getBean("course", Course.class);
        Course course2 = context.getBean("course", Course.class);

        System.out.println(course1 == course2);//false
    }
```
# 五、代理模式
## 1. 静态代理
**静态代理的好处:**
 - **可以使得我们的真实角色更加纯粹，不再去关注一些公共的事情 .**
 - **公共的业务由代理来完成，实现了业务的分工 ！**
 - **公共业务发生扩展的时候，方便集中管理 ！**

**缺点 :**
 - **类多了 , 多了代理类 , 工作量变大了 . 开发效率降低**
 - **一个真实角色就会产生一个代理角色；代码量就会翻倍，开发效率会变低**

**样例**

**例如小明要租房子 他直接接触的是中介而不是房东 但是小小明依旧能够租到房子 而真正出租房屋的人确实房东 此时中介起到了代理的作用** 
## 2. 动态代理

>动态代理是一种常用的设计模式，广泛应用于框架中，Spring框架的AOP特性就是应用动态代理实现的。

**动态代理的好处**

 - 可以使得我们的真实角色更加纯粹 . 不再去关注一些公共的事情 .
 - 公共的业务由代理来完成 . 实现了业务的分工 ,
 - 公共业务发生扩展时变得更加集中和方便 .
 - 一个动态代理 , 一般代理某一类业务
 - 一个动态代理可以代理多个类，代理的是接口

**实现动态代理有两种形式**:
- jdk动态代理：根据目标类接口获取代理类实现规则，生成代理对象。这个代理对象，也是目标类接口的一个实现类。
-  cglib动态代理：根据目标类本身获取代理类实现规则，生成代理对象。这个代理对象，也是目标类的一个子类。 （如果目标类为final，则不能使用CGLib实现动态代理）

**SpringAOP可以自动在jdk动态代理和CGLib动态代理之间进行切换，规则如下：**
- 如果目标对象实现了接口，采用jdk动态代理实现aop。
- 如果目标对象没有实现接口，采用CGLib动态代理实现aop。
-  如果目标对象实现了接口，但仍然想要使用CGLIB实现aop，可以手动进行配置

**总结**
 - 静态代理代理的接口是写死的，只能代理该类接口的实现类，实现其对应的方法，真实角色都属于同一类，
-  动态代理通过反射获取被代理对象的接口类型，所以代理的对象可以是任意的，代理类都可以通过反射获取接口类型，被代理对象的方法也可以通过反射获取，
 - 所以动态代理相比静态代理最大的区别就是能够代理的类型可以是任意（Object）的，而静态代理只能代理一类对象（实现相同的接口），静态代理实现了和被代理类相同的接口，而动态代理实现的是InvocationHandler ，可以通过反射实现任意类型的接口，更具灵活性。
 - AOP的底层机制就是动态代理！
# 六、AOP
## 1.AOP简介
>**把我们程序重复的代码抽取出来，在不改变原始设计的基础上为其进行功能增强。简单的说就是在不改变方法源代码的基础上对方法进行功能增强**

**AOP中的核心概念**
1. 连接点（JoinPoint）：正在执行的方法，例如：update()、delete()、select()等都是连接点。
2. 切入点（Pointcut）：进行功能增强了的方法，在SpringAOP中，一个切入点可以只描述一个具体方法，也可以匹配多个方法
	* 一个具体方法：com.chq.mapper包下的StudentMapper接口中的无形参无返回值的save方法
 	* 匹配多个方法：所有的save方法，所有的get开头的方法，所有以Mapper结尾的接口中的任意方法，所有带有一个参数的方法
3.  通知（Advice）：在切入点前后执行的操作，也就是增强的共性功能
	* 在SpringAOP中，功能最终以方法的形式呈现
4. 通知类：通知方法所在的类叫做通知类
5. 切面（Aspect）：描述通知与切入点的对应关系，也就是哪些通知方法对应哪些切入点方法。
## 2.样例注解方式实现AOP
**1. 添加aspectj依赖**
```xml
<dependencies>
        <!--spring核心依赖，会将spring-aop传递进来-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>
        <!--切入点表达式依赖，目的是找到切入点方法，也就是找到要增强的方法-->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.4</version>
        </dependency>
    </dependencies>
```

**2. 定义CourseMapper接口与实现类**
```java
public interface CourseService {
    void save();
    void update();
}

@Repository
public class CourseMapperImpl implements CourseMapper {
    @Override
    public void save() {
        System.out.println(System.currentTimeMillis());
        System.out.println("我是Course Mapper Save");
    }

    @Override
    public void update() {
        System.out.println("我是Course Mapper Update");
    }
}

```

**3. 定义通知类，制作通知方法 ，定义切入点表达式、配置切面(绑定切入点与通知关系)** 
```java
//通知类必须配置成Spring管理的bean
@Component
//设置当前类为切面类类
@Aspect
public class MyAdvice {
    @Pointcut("execution(void com.chq.mapper.CourseMapper.update())")
    private void pt(){

    }

    //设置在切入点pt()的前面运行当前操作(前置通知)
    @Before("pt()")
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

**4. 在配置类中进行Spring注解包扫描和开启AOP功能**
```java
@Configuration
@ComponentScan("com.chq")
//开启注解开发AOP功能
@EnableAspectJAutoProxy
public class SpringConfig {
}
```
**5. 测试**
```java
public class CourseTest3 {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(SpringConfig.class);
        CourseMapper courseMapper = applicationContext.getBean(CourseMapper.class);
        courseMapper.update();
    }

    /**
     * 控制台打印
     * 1730711515074
     * 我是Course Mapper Update
     */
}
```
## 3.AOP工作流程
**1. Spring容器启动**
**2. 读取所有切面配置中的切入点**
**3. 初始化bean，判定bean对应的类中的方法是否匹配到任意切入点**
**4. 获取bean执行方法**

## 4.AOP切入点表达式
>**execution() 是一个Aspect表达式，语法为：execution(返回值类型 包名.类名.方法名 (参数) 异常)** 

 例如: `execution (* com.chq.mapper.impl..*. *(..))`·
 
**整个表达式可以分为五个部分：**
1、execution(): 表达式主体。
2、第一个`*`号：表示返回类型， `*`号表示所有的类型。
3、包名：表示需要拦截的包名，后面的两个句点表示当前包和当前包的所有子包， com.chq.mapper.impl 包、子孙包下所有类的方法。
4、第二个`*`号：表示类名，号表示所有的类。
5、`(..)`：最后这个星号表示方法名，号表示所有的方法，后面括弧里面表示方法的参数，两个句点表示任何参数
## 5.AOP通知类型
**AOP通知共分为5种类型**
- 前置通知：在切入点方法执行之前执行
- 后置通知：在切入点方法执行之后执行，无论切入点方法内部是否出现异常，后置通知都会执行。
- 环绕通知(重点)：手动调用切入点方法并对其进行增强的通知方式。
- 返回后通知(了解)：在切入点方法执行之后执行，如果切入点方法内部出现异常将不会执行。
- 抛出异常后通知(了解)：在切入点方法执行之后执行，只有当切入点方法内部出现异常之后才执行。

**AOP通知示例**
**先构建一个架子**
```java
public interface CourseMapper {
    void save();
    void update();
    void delete();
}

@Repository
public class CourseMapperImpl implements CourseMapper {
    @Override
    public void save() {
        System.out.println("我是Course Mapper Save");
    }

    @Override
    public void update() {
        System.out.println("我是Course Mapper Update");
    }

    @Override
    public void delete() {
        System.out.println("我是Course Mapper Delete");
    }
}
```
```java
//通知类必须配置成Spring管理的bean
@Component
//设置当前类为切面类
@Aspect
public class MyAdvice {

    //设置切入点，@Pointcut注解要求配置在方法上方
    @Pointcut("execution(* com.chq.mapper.Impl.CourseMapperImpl.*(..))")
    public void pt(){
    }
}
```
```java
@Configuration
@ComponentScan("com.chq")
//开启注解开发AOP功能
@EnableAspectJAutoProxy
public class SpringConfig {
}

```
```java
public class AopTest {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(SpringConfig.class);
        CourseMapper courseMapper = applicationContext.getBean(CourseMapper.class);
        courseMapper.save();
    }
}
```
---
**前置通知**
- 名称：@Before
- 类型：方法注解
- 位置：通知方法定义上方
- 作用：设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法前运行
```java
@Before("pt()")
public void before() {
    System.out.println("before advice ...");
}
	/**
     * before advice ...
     * 我是Course Mapper Save
     */
```
---
**后置通知**
- 名称：@After
- 类型：方法注解
- 位置：通知方法定义上方
- 作用：设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法后运行

```java
@After("pt()")
public void after() {
    System.out.println("after advice ...");
}
	/**
	 * before advice ...
	 * 我是Course Mapper Save	
	 * after advice ...
	 */
```
---
**环绕通知**
- 名称：@Around（重点，常用）
- 类型：方法注解
- 位置：通知方法定义上方
- 作用：设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法前后运行

```java
@Around("pt()")
    public Object around(ProceedingJoinPoint pjp) throws Throwable {
        //1.环绕通知方法形参必须是ProceedingJoinPoint，表示正在执行的连接点，
        //  使用该对象的proceed()方法表示对原始对象方法进行调用，返回值为原始对象方法的返回值。
        //2.环绕通知方法的返回值建议写成Object类型，用于将原始对象方法的返回值进行返回
        //  哪里使用代理对象就返回到哪里。
        System.out.println("around before advice ...");
        Object ret = pjp.proceed();
        System.out.println("around after advice ...");
        return ret;
    }
    /**
     * around before advice ...
     * before advice ...
     * 我是Course Mapper Save
     * after advice ...
     * around after advice ...
     */
```
---
**返回后通知**
- 名称：@AfterReturning（了解）
- 类型：方法注解
- 位置：通知方法定义上方
- 作用：设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法正常执行完毕后运行
```java
@AfterReturning("pt()")
    public void afterReturning() {
        System.out.println("afterReturning advice ...");
    }
    /**
     * around before advice ...
     * before advice ...
     * 我是Course Mapper Save
     * afterReturning advice ...
     * after advice ...
     * around after advice ...
     */
```
---
**抛出异常后通知**
- 名称：@AfterThrowing（了解）
- 类型：方法注解
- 位置：通知方法定义上方
- 作用：设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法运行抛出异常后执行

更改save方法
```java
@Override
    public void save() {
        int a = 1/0;
        System.out.println("我是Course Mapper Save");
    }
```
```java
@AfterThrowing("pt()")
public void afterThrowing() {
    System.out.println("afterThrowing advice ...");
}
    /**
     * around before advice ...
     * before advice ...
     * afterThrowing advice ...
     * after advice ...
     * Exception in thread "main" java.lang.ArithmeticException: / by zero.....
     */

```

# 七、声明式事务

**事务四个属性ACID原则：**
* 原子性（atomicity）：事务是原子性操作，由一系列动作组成，事务的原子性确保动作要么全部完成，要么完全不起作用
* 一致性（consistency）：一旦所有事务动作完成，事务就要被提交。数据和资源处于一种满足业务规则的一致性状态中
* 隔离性（isolation）：可能多个事务会同时处理相同的数据，因此每个事务都应该与其他事务隔离开来，防止数据损坏
* 持久性（durability）：事务一旦完成，无论系统发生什么错误，结果都不会受到影响。通常情况下，事务的结果被写到持久化存储器中

>**Spring为了支持事务管理，专门封装了事务管理对象。我们只要在Spring容器中配置这个对象，即可使用。在Spring容器中添加事务管理的配置**
```xml
<!-- 配置Spring提供的事务管理器 -->
<bean id="transactionManager"
  class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
  <property name="dataSource" ref="dataSource"></property>
</bean>
<!-- 开启注解事务 -->
<tx:annotation-driven transaction-manager="transactionManager" />
```


>**在Service组件中，使用@Transactional注解，就可以给业务方法添加事务管理。**
```java
@Transactional
public int transferAccounts(Emp emp1,Emp emp2) {
    //需要事务管理的业务
}
//注意：
//1. 需要事务管理的service，在方法上加上@Transactional 注解即可。
//2. 必须为public方法才行,不要捕捉异常，要让异常自动抛出，否则不能进行事务回滚。

```

**事务传播行为**
**@Transactional 注解中的 propagation 属性，可以设置事务传播行为。属性值为：**
1. REQUIRED：如果当前没有事务，就新建一个事务，如果已经存在一个事务中，就加入到这个事务中。这是最常见的选择。
2. SUPPORTS：支持当前事务，如果当前没有事务，就以非事务方式执行。
3. MANDATORY：使用当前的事务，如果当前没有事务，就抛出异常。
4. REQUIRES_NEW：新建事务，如果当前存在事务，把当前事务挂起。
5. NOT_SUPPORTED：以非事务方式执行操作，如果存在事务，就把当前事务挂起。
6. NEVER：以非事务方式执行，如果当前存在事务，则抛出异常。



---

**Spring整合Mybatis可以看我之前写的文章 [https://blog.csdn.net/qq_60972641/article/details/143271487?spm=1001.2014.3001.5501](https://blog.csdn.net/qq_60972641/article/details/143271487?spm=1001.2014.3001.5501)**
