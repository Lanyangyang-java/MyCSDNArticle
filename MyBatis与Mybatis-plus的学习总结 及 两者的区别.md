
@[TOC](MyBatis与Mybatis-plus的学习总结及两者的区别 超详细 样例很多  我的学习笔记)

---
# 一、MyBatis
## 1.MyBatis简介
**什么是MyBatis？**
>MyBatis 是一款优秀的持久层框架，用于简化 JDBC 开发
官网：[https://mybatis.net.cn/index.html](https://mybatis.net.cn/index.html)

**持久层**
>负责将数据到保存到数据库的那一层代码
>JavaEE三层架构：表现层、业务层、持久层

**框架**
>框架就是一个半成品软件，是一套可重用的、通用的、软件基础代码模型
在框架的基础之上构建软件编写更加高效、规范、通用、可扩展


**MyBatis免除了几乎所有的JDBC代码以及设置参数和获取结果集的工作**
Mybatis将JDBC的硬编=>配置文件 繁琐的操作=>自动完成
**来看之前的JDBC编码**
```java
public class JDBCTest {
    public static void main(String[] args) throws Exception{
        //1.注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.获取连接
        String url="jdbc:mysql://127.0.0.1:3306/test";
        String username="root";
        String password="1234";
        Connection conn = DriverManager.getConnection(url, username, password);
        //3.定义sql
        String sql="select * from student";
        //4.获取执行sql的对象Statement
        Statement stmt = conn.createStatement();
        //5.执行sql语句
        ResultSet resultSet = stmt.executeQuery(sql);
        //6.处理结果
        while (resultSet.next()) {
            System.out.println(resultSet.getInt("id")+","+resultSet.getString("name")+","+resultSet.getInt("age"));
        }
        //7.释放资源
        resultSet.close();
        stmt.close();
        conn.close();
    }
}
```

---

**使用Mybatis之后 注册驱动获取链接**
```xml
<dataSource type="POOLED">
      <!--数据库连接信息-->
      <property name="driver" value="com.mysql.jdbc.Driver"/>
      <property name="url" value="jdbc:mysql:///test?useSSL=false"/>
      <property name="username" value="root"/>
      <property name="password" value="1234"/>
</dataSource>
```
**sql语句**
```xml
<select id="selectAll" resultMap="Student">
    select * from student;
</select>
```
**手动封装结果集=>自动完成**
```java
List<Student> students = studentMapper.selectAll();
```
## 2.MybatisX插件
**安装**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/327b0fabb4814be5b89f92eceb600be2.png =600x)
**搜索插件**
![请添加图片描述](https://i-blog.csdnimg.cn/direct/991c33633d1d4b9b8f174093e57cd12b.png =600x)
**效果** **Mapper接口与xml自动跳转功能**
![请添加图片描述](https://i-blog.csdnimg.cn/direct/936ef9173c754460a2e9d89814eaa57f.png =600x)
![请添加图片描述](https://i-blog.csdnimg.cn/direct/f239c7dc1cee4c1a9103ff8430db0310.png =600x)

## 3.Mapper 代理开发
**目的:** 解决原生方式中的硬编码简化后期执行SQL

**使用Mapper代理方式**
1. 定义与SQL映射文件同名的Mapper接口，并且将Mapper接口和SQL映射文件放置在同一目录下

![请添加图片描述](https://i-blog.csdnimg.cn/direct/6959ac6192984d469c7b8a28fd183414.png =600x)

2. 设置SQL映射文件的namespace属性为Mapper接口全限定名   所谓全限定名 = 包名 + 类型名

![请添加图片描述](https://i-blog.csdnimg.cn/direct/0fb50b85455a4240a3d5d5dae758908a.png =600x)


3. 在 Mapper 接口中定义方法，方法名就是SQL映射文件中sql语句的id，并保持参数类型和返回值类型一致
![请添加图片描述](https://i-blog.csdnimg.cn/direct/a8beef90e4414ecc973aca5bd31c51b5.png =600x)
4. 配置mybaits
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <typeAliases>
        <package name="com.chq.pojo"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!--数据库连接信息-->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///test?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="1234"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!--加载sql映射文件-->
        <!-- <mapper resource="com/chq/mapper/StudentMapper.xml"/>-->
        
        <!--Mapper代理方式-->
        <!--扫描mapper包-->
        <package name="com.chq.mapper"/>
    </mappers>
</configuration>
```
5. 编码，通过 SqlSession 的 getMapper方法获取 Mapper接口的代理对象，调用对应方法完成sql的执行
```java
public class StudentTest1 {
    public static void main(String[] args) throws IOException {
        //1.加载mybatis核心配置文件，获取SqlSessionFactory
        String resource="mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        //2.获取SqlSession对象,用来执行sql
        //openSession()：默认开启事务，进行增删改操作后需要使用 sqlSession.commit();手动提交事务
        //openSession(true)：可以设置为自动提交事务（关闭事务）
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //3.执行sql语句
        StudentMapper studentMapper = sqlSession.getMapper(StudentMapper.class);
        List<Student> students = studentMapper.selectAll();
        students.stream().forEachOrdered(System.out::println);
        //4.释放资源
        sqlSession.close();
    }
}
```
**结果：**
![请添加图片描述](https://i-blog.csdnimg.cn/direct/38679512ab384ab990663e669e872b54.png)

**细节：如果Mapper接口名称和SQL映射文件名称相同，并在同一目录下，则可以使用包扫描的方式简化SQL映射文件的加载**

---
## 4.配置文件完成CRUD
**我们准备一张Student表**

![请添加图片描述](https://i-blog.csdnimg.cn/direct/3c310ae3167d4dafb0977194dcd5587e.png =600x)
**在写好Student实体类**
```java
//lombok依赖 导入pom.xml中即可
@Data //set get ToString（）等
@AllArgsConstructor //有参构造
@NoArgsConstructor //无参构造
public class Student {
    private Integer id;
    private String name;
    private Integer age;
    private String username;
    private String password;
}
```

---
**添加(create)**

**StudentMapper**
```java
public interface StudentMapper {

    List<Student> selectAll();
    
    void add(Student student);
}
```
**StudentMapper.xml** 
```xml
 <!-- useGeneratedKeys允许自动生成主键  keyProperty主键名称  -->
    <insert id="add" useGeneratedKeys="true" keyProperty="id">
        insert into student (name, age, username, password)
        values (#{name}, #{age}, #{username}, #{password});
    </insert>
```
**测试**
```java
	@Test
    public void testAdd() throws IOException {
        //封装对象
        Student student = new Student();
        student.setName("嘿嘿嘿");
        student.setAge(88);
        student.setUsername("123456");
        student.setPassword("654321");
        //1.加载mybatis核心配置文件，获取SqlSessionFactory
        String resource="mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        //2.获取SqlSession对象,用来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //3.执行sql语句
        StudentMapper studentMapper = sqlSession.getMapper(StudentMapper.class);
        studentMapper.add(student);
        sqlSession.commit();![请添加图片描述](https://i-blog.csdnimg.cn/direct/6653fa7169a44568982057719076aa41.png)

        //4.释放资源
        sqlSession.close();
    }
```

**查询(retrieve)**
**1. 查询所有数据**
上面Mapper代理开发写过了这里就不写

---
**2. 通过Id查询**
**StudentMapper**
```java
Student selectById(int id);
```
**StudentMapper.xml** 
```xml
<select id="selectById" resultType="com.chq.pojo.Student">
        select *
        from student
        where id = #{id}
</select>
```
**测试**
```java
@Test
    public void testSelectById() throws IOException{
        //1.加载mybatis核心配置文件，获取SqlSessionFactory
        String resource="mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        //2.获取SqlSession对象,用来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();
        StudentMapper studentMapper = sqlSession.getMapper(StudentMapper.class);
        int id = 1;
        Student student = studentMapper.selectById(id);
        System.out.println(student);
    }
```

---
**3.条件查询-多条件查询+动态条件**
>MyBatis 对动态SQL有很强大的支撑
if
 choose (when)
 trim (where, set)
 foreach

**StudentMapper**
```java
List<Student> selectByCondition(Student student);
```
**StudentMapper.xml** 
```xml
<select id="selectByCondition" resultType="com.chq.pojo.Student">
        select *
        from student
        <where>
        	<!--模糊查询   动态条件-->
            <if test="name != null and name != '' ">
                and name like "%"#{name}"%"
            </if>
            <if test="age != null">
                and age = #{age}
            </if>
        </where>
    </select>
```
**测试**
```java
@Test
    public void selectByCondition() throws IOException{
        //1.加载mybatis核心配置文件，获取SqlSessionFactory
        String resource="mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        //2.获取SqlSession对象,用来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();
        StudentMapper studentMapper = sqlSession.getMapper(StudentMapper.class);
        Student student = new Student();
        student.setName("坤");
        student.setAge(16);
        List<Student> students = studentMapper.selectByCondition(student);
        students.stream().forEach(System.out::println);
        System.out.println();
    }
```


**4.查询-单条件查询-动态条件**
**StudentMapper**
```java
List<Student> selectByConditionSingle(Student student);
```
**StudentMapper.xml** 
```xml
<select id="selectByConditionSingle" resultType="com.chq.pojo.Student">
        select *
        from student
        <where>
            <choose><!--相当于switch-->
                <when test="name != null"><!--相当于case-->
                    name like "%"#{name}"%"
                </when>
                <when test="username != null and username != '' "><!--相当于case-->
                    username = #{username}
                </when>
                <when test="password != null and password != ''"><!--相当于case-->
                    password = #{password}
                </when>

            </choose>
        </where>
    </select>
```
**测试**
```java
@Test
    public void selectByConditionSingle() throws IOException{
        //1.加载mybatis核心配置文件，获取SqlSessionFactory
        String resource="mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        //2.获取SqlSession对象,用来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();
        StudentMapper studentMapper = sqlSession.getMapper(StudentMapper.class);
        Student student = new Student();
        student.setName("坤");
        student.setUsername("123456");
        student.setPassword("654321");
        List<Student> students = studentMapper.selectByConditionSingle(student);
        students.stream().forEach(System.out::println);
        System.out.println();
    }
```

---
**修改数据(update)**
**StudentMapper**
```java
void update(Student student);
```
**StudentMapper.xml** 
```xml
<update id="update">
        update student
        <set>
            <if test="name != null and name !='' ">
                name = #{name},
            </if>
            <if test="age != null">
                age = #{age},
            </if>
            <if test="username != null and username !=''">
                username = #{username},
            </if>
            <if test="password != null and password !=''">
                password = #{password},
            </if>

        </set>
        where id = #{id};
    </update>
```
**测试**
```java
 @Test
    public void selectUpdate() throws IOException{
        //1.加载mybatis核心配置文件，获取SqlSessionFactory
        String resource="mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        //2.获取SqlSession对象,用来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();
        StudentMapper studentMapper = sqlSession.getMapper(StudentMapper.class);
        int id = 6;
        Student student = new Student();
        student.setId(id);
        student.setName("加菲猫");
        student.setAge(18);
        student.setUsername("852963");
        student.setPassword("852963");
        System.out.println(studentMapper.selectById(id));
        studentMapper.update(student);
        System.out.println(studentMapper.selectById(id));
        System.out.println();
    }
```
**结果**

![请添加图片描述](https://i-blog.csdnimg.cn/direct/099e5b88037a495888b1999d20a09dbf.png)

---
**删除数据(delete)**
**1.删除一个**
**StudentMapper**
```java
void deleteById(int id);
```
**StudentMapper.xml** 
```xml
<delete id="deleteById">
     delete from student where id = #{id}
</delete>
```
**测试**
```java
@Test
    public  void testDeleteById() throws IOException {
        //接收参数
        int id=4;
        //1.加载mybatis核心配置文件，获取SqlSessionFactory
        String resource="mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        //2.获取SqlSession对象,用来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //3.执行sql语句
        StudentMapper studentMapper = sqlSession.getMapper(StudentMapper.class);
        studentMapper.deleteById(id);
        System.out.println("删除成功");
        //4.释放资源
        sqlSession.commit();
        sqlSession.close();
    }
```

**2.删除多个**
**StudentMapper**
```java
void deleteByIds(int[] ids);
```
**StudentMapper.xml** 
```xml
<delete id="deleteByIds">
        delete 
        from student
        where id in
        <foreach collection="array" item="id" separator="," open="(" close=")">
            #{id}
        </foreach>
    </delete>
```
**测试**
```java
@Test
    public  void testDeleteByIds() throws IOException {
        //接收参数
        int [] ids={5,6,7};
        //1.加载mybatis核心配置文件，获取SqlSessionFactory
        String resource="mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        //2.获取SqlSession对象,用来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //3.执行sql语句
        StudentMapper studentMapper = sqlSession.getMapper(StudentMapper.class);
        studentMapper.deleteByIds(ids);
        System.out.println("删除成功");
        //4.释放资源
        sqlSession.commit();
        sqlSession.close();
    }
```


## 5.注解完成CRUD
使用注解开发会比配置文件开发更加方便
**查询：**@Select
**添加：**@Insert
**修改**：@Update
**删除：**@Delete


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2d6a763a7b4b468d9c89711e5f79bc99.png =1000x)


>**不需要配置xml文件，在StudentMapper中更改**

## 6.动态 SQL
>SQL语句会随着用户的输入或外部条件的变化而变化，我们称为动态SQ

 **if+where标签**
 
>if+where会实现以下功能：
1. 自动添加where
2. 不需要考虑where后是否加and，mybatis会自动处理
3. 不需要考虑是否加空格，mybatis会自动处理
4. 没有 else 标签，也没有 else if 标签。


**样例**
```xml
	<select id="selectByCondition" resultType="com.chq.pojo.Student">
        select *
        from student
        <where>
            <if test="name != null and name != '' ">
                and name like "%"#{name}"%"
            </if>
            <if test="age != null">
                and age = #{age}
            </if>
        </where>
    </select>
```

---

**choose标签**
>choose会实现如下功能：
1. 多个 when 标签中，只能执行一个。也就是说：当一个 when 条件满足并执行后，其它的 when 将不再执行。
2. 当所有 when 都不满足条件时，执行 otherwise 标签。


**样例**
```xml
	<select id="selectByConditionSingle" resultType="com.chq.pojo.Student">
        select *
        from student
        <where>
        	<!--相当于switch-->
            <choose>
            	<!--相当于case-->
                <when test="name != null">
                    name like "%"#{name}"%"
                </when>
                <!--相当于case-->
                <when test="username != null and username != '' ">
                    username = #{username}
                </when>
                <!--相当于case-->
                <when test="password != null and password != ''">
                    password = #{password}
                </when>

            </choose>
        </where>
    </select>
```

---
**trim标签**

>trim标签可以在自己包含的内容中加上某些前缀或后缀，与之对应的属性是：prefix、suffix。 还可以把包含内容的开始内容覆盖，即忽略。也可以把结束的某些内容覆盖，对应的属性是：prefixOverrides、suffixOverrides

**样例**
```xml
	<select id="selectByCondition" resultType="com.chq.pojo.Student">
        select *
        from student
        <trim prefix="WHERE" prefixOverrides="AND |OR">
            <if test="name != null and name != '' ">
                and name like "%"#{name}"%"
            </if>
            <if test="age != null">
                and age = #{age}
            </if>
        </trim>
    </select>
```

**样例解释**

>`prefix="WHERE"`：如果` <trim> `标签内部有内容被包含（即至少有一个` <if>` 条件为真），那么会在这些内容之前添加 WHERE 关键字。
>`prefixOverrides="AND |OR "`：这是一个用竖线 | 分隔的字符串列表，表示如果 `<trim> `标签内部的内容以 AND 或 OR 开头，那么这些前缀将被删除。这通常用于处理动态添加的条件，因为第一个条件之前不应该有 AND 或 OR，但后续的条件之前可能需要这些逻辑运算符。

**注意：**
1. prefix与suffix可以在sql语句中拼接出一对小括号。
2. suffixOberrides可以将最后一个逗号去掉。

---
**set标签**
>set标签主要用于更新操作时使用

**样例**
```xml
	<update id="update">
        update student
        <set>
            <if test="name != null and name !='' ">
                name = #{name},
            </if>
            <if test="age != null">
                age = #{age},
            </if>
            <if test="username != null and username !=''">
                username = #{username},
            </if>
            <if test="password != null and password !=''">
                password = #{password},
            </if>

        </set>
        where id = #{id};
    </update>
```

---

**foreach标签**
>foreach标签可以在sql中迭代一个集合或数组，主要用于拼接in条件

**foreach标签的属性：**
属性名称|说明
-|-
collection|需要遍历的类型，值有：list、array
item|表示遍历出来的对象
open|表示语句的开始部分
close|表示语句的结束部分
separator|表示每次迭代之间以什么符号为间隔
index|每次迭代的位置索引，就是循环变量


**样例**
```xml
    <delete id="deleteByIds">
        delete
        from student
        where id in
        <foreach collection="array" item="id" separator="," open="(" close=")">
            #{id}
        </foreach>
    </delete>
```

# 二、MyBatis-plus
## 1.MyBatis-plus快速入门	
>MyBatis-Plus是MyBatis的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。
>官网：[https://baomidou.com/introduce/](https://baomidou.com/introduce/)

**特性**
特性|说明
-|-
无侵入|只做增强不做改变，不会对现有工程产生影响
损耗小|启动即会自动注入基本 CURD
强大的 CRUD 操作|内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
支持 Lambda 形式调用|通过 Lambda 表达式，方便的编写各类查询条件
支持自定义全局通用操作|支持全局通用方法注入（ Write once, use anywhere ）
内置分页插件|基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
分页插件支持多种数据库|支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
内置性能分析插件|可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
内置全局拦截插件|提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作



**快速开始**
**数据库准备**
**deleted字段后面逻辑删除用到**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ea821f4f2a9b42c1bf48aa1340559ff6.png)
**类型**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4d0c994ee63c42fc900c36c0707988a0.png)



**1. 创建SpringBoot工程**
**2. 导入依赖**
```xml
		<!--mybatis-plus依赖-->
		<dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.1</version>
        </dependency>

        <!--druid依赖-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>

        <!--数据库连接依赖-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>


        <!--Lombok依赖-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
```
 **3.配置`application.yml`**
 ```java
 server:
   port: 8000

spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/test?characterEncoding=utf-8&useSSL=false
    username: root
    password: 1234


 # 扫包  有需要就写 一般情况都是写的
mybatis-plus:
  mapper-locations: classpath:mappers/*.xml
  #日志
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
 ```

**4.编写实体类 Student**
```java
//此处使用了Lombok
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Student extends Model<Student> {

    @TableId
    private Long id;

    private String name;

    private Integer age;

    private String username;

    private String password;
}
```

**5. 编写Mapper**
```java
@Repository
public interface StudentMapper extends BaseMapper<Student> {
}
```

**6.编写Service和ServiceImpl**
```java
public interface StudentService extends IService<Student> {
}


@Service
public class StudentServiceImpl extends ServiceImpl<StudentMapper, Student> implements StudentService {
}
```
**6.测试**
```java
@SpringBootTest
public class StudentTest {
	@Autowired
    private StudentMapper studentMapper;

    @Autowired
    private StudentService studentService;


	@Test
    public void testSelectList(){
        List<Student> studentList = studentMapper.selectList(null);
        studentList.stream().forEach(System.out::println);
    }
}
```

## 2.条件构造器Wrapper
### AbstractWrapper
>==**用于查询条件封装**==
>**官方语言：**
>QueryWrapper(LambdaQueryWrapper) 和 UpdateWrapper(LambdaUpdateWrapper) 的父类用于生成 sql 的 where 条件
>entity 属性也用于生成 sql 的 where 条件
>注意: entity 生成的 where 条件与使用各个 api 生成的 where 条件没有任何关联行为

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/001699ea1a074d1a87f8a971880b8714.png =1000x)

### QueryWrapper
>==**用于构建查询条件**==
>继承自 AbstractWrapper
>自身的内部属性entity也用于生成 where 条件及 LambdaQueryWrapper
>可以通过 new QueryWrapper().lambda() 方法获取

### UpdateWrapper
>用于构建更新条件
>继承自 AbstractWrapper
>自身的内部属性 entity 也用于生成 where 条件及 LambdaUpdateWrapper,
>可以通过 new UpdateWrapper().lambda() 方法获取
### Lambda构造器
**LambdaQueryWrapper**
>支持Lambda表达式的查询条件构造器，更加类型安全，减少了字段名写错的风险。
```java
  	@Test
    public void testLambdaQueryWrapper(){
        // 创建一个LambdaQueryWrapper对象
        LambdaQueryWrapper<Student> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.gt(Student::getAge, 18);
        // 执行查询操作
        List<Student> studentList = studentMapper.selectList(queryWrapper);

        studentList.stream().forEach(System.out::println);

    }
```

**LambdaUpdateWrapper**
>支持Lambda表达式的修改条件构造器，更加类型安全，减少了字段名写错的风险。
```java
	@Test
    public void testLambdaUpdateWrapper(){
        // 创建一个LambdaQueryWrapper对象
        LambdaUpdateWrapper<Student> queryWrapper = new LambdaUpdateWrapper<>();
        queryWrapper.eq(Student::getAge, 18);
        Student student = new Student();
        student.setUsername("000000");
        // 执行查询操作
        System.out.println(studentService.update(student,queryWrapper));
    }
```

## 3.CRUD接口
### MapperCRUD接口
>**通用CRUD封装BaseMapperCRUD接口为 Mybatis-Plus 启动时自动解析实体表关系映射转换为 Mybatis 内部对象注入容器**

**这里详情可以参考BaseMapper.java** 
![请添加图片描述](https://i-blog.csdnimg.cn/direct/dba305022e3044b18b983544f2f8f09c.png)

#### (1) 插入insert

**insert**
```java
/**
 * 插入一条记录
 * @param entity 实体对象
 * @return 插入成功记录数
 */
int insert(T entity);
-------------------------------------------------------------
	@Test
    public void testInsert(){
        //封装一个Student类对象
        Student student = new Student();
        student.setName("懒羊羊");
        student.setAge(23);
        student.setUsername("123456");
        student.setPassword("123456");
        int insert = studentMapper.insert(student);
        System.out.println(insert);
    }

```
---
#### (2) 删除delete
**deleteById**
```java
/**
 * 根据 ID 删除
 * @param id 主键ID
 * @return 删除成功记录数
 */
int deleteById(Serializable id);
-------------------------------------------------------------

	@Test
    public void testDeleteById(){
        System.out.println(studentMapper.deleteById(30L));
    }

```
---
**deleteByMap**
```java
/**
 * 根据 columnMap 条件，删除记录
 * @param columnMap 表字段 map 对象
 * @return 删除成功记录数
 */
int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
-------------------------------------------------------------
	@Test
    public void testDeleteByMap() {
        Map<String,Object> map = new HashMap<>();
        map.put("name","懒羊羊");
        int i = studentMapper.deleteByMap(map);
        System.out.println("i = " + i);
    }

```
---

**delete**
```java
/**
 * 根据 entity 条件，删除记录
 * @param wrapper 实体对象封装操作类（可以为 null）
 * @return 删除成功记录数
 */
int delete(@Param(Constants.WRAPPER) Wrapper<T> wrapper);
-------------------------------------------------------------
	@Test
    public void testDelete() {
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        wrapper.gt("age", 18);
        int delete = studentMapper.delete(wrapper);
        System.out.println("delete = " + delete);
    }

```
---

**deleteBatchIds**
```java
/**
 * 删除（根据ID 批量删除）
 * @param idList 主键ID列表(不能为 null 以及 empty)
 * @return 删除成功记录数
 */
int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
-------------------------------------------------------------
	@Test
    public void testDeleteBatchIds() {
        List<Long> ids = Arrays.asList(1L,2L,3L);
        int i = studentMapper.deleteBatchIds(ids);
        System.out.println("i = " + i);
    }

```
---

#### (3) 更新update
**updateById**
```java
/**
 * 根据 ID 查询
 * @param id 主键ID
 * @return 实体
 */
T selectById(Serializable id);
-------------------------------------------------------------
	@Test
    public void testUpdateById(){
        Student student = new Student();
        student.setId(3L);
        student.setName("灰太狼");
        student.setAge(43);
        student.setUsername("999999");
        student.setPassword("123456");
        System.out.println(studentMapper.updateById(student));
    }
```
---

**update**
```java
/**
 * 根据 whereEntity 条件，更新记录
 * @param entity        实体对象 (set 条件值,可为 null)
 * @param updateWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句）
 * @return 修改成功记录数
 */
int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER) Wrapper<T> updateWrapper);
-------------------------------------------------------------
	@Test
    public void testUpdate() {
        UpdateWrapper<Student> wrapper = new UpdateWrapper<>();
        wrapper.eq("name","懒羊羊");

        Student student = new Student();
        student.setName("沸羊羊");

        int update = studentMapper.update(student, wrapper);
        System.out.println("update = " + update);

    }
```
---

#### (4) 查询select

**selectById**
```java
/**
 * 根据ID查询
 * @param id 主键ID
 * @return 实体
 */
T selectById(Serializable id);
-------------------------------------------------------------
	@Test
    public void testSelectById(){
        System.out.println(studentMapper.selectById(3L));
    }

```
---

**selectBatchIds**
```java
/**
 * 查询（根据ID 批量查询）
 * @param idList 主键ID列表(不能为 null 以及 empty)
 * @return 实体集合
 */
List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
-------------------------------------------------------------
	@Test
    public void testSelectBatchIds() {
        List<Long> ids = Arrays.asList(1L,2L,3L);
        List<Student> studentList = studentMapper.selectBatchIds(ids);
        studentList.stream().forEach(System.out::println);
    }

```
---

**selectByMap**
```java
/**
 * 查询（根据 columnMap 条件）
 * @param columnMap 表字段 map 对象
 * @return 实体集合
 */
List<T> selectByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);

-------------------------------------------------------------
	@Test
    public void testSelectByMap() {
        Map<String,Object> map = new HashMap<>();
        map.put("age",18);
        List<Student> studentList = studentMapper.selectByMap(map);
        studentList.stream().forEach(System.out::println);
    }

```
---

**selectOne**
```java
/**
 * 根据 entity 条件，查询一条记录
 * @param queryWrapper 实体对象
 * @return 实体
 */
T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
-------------------------------------------------------------
	@Test
    public void testSelectOne() {
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        //小灰灰数据库有两条数据 不加唯一条件或不加last("limit 1")会报错 只会查询一条数据 查多了报错
        //wrapper.eq("name","小灰灰").eq("id",1851424527429312513L);
        wrapper.last("limit 1").eq("name","小灰灰");
        Student student = studentMapper.selectOne(wrapper);
        System.out.println(student);
    }

```
---

**selectCount**
```java
/**
 * 根据 Wrapper 条件，查询总记录数
 * @param queryWrapper 实体对象
 * @return 满足条件记录数
 */
Integer selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
-------------------------------------------------------------
	@Test
    public void testSelectCount() {
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        wrapper.gt("age",18).like("name","羊羊");
        Long aLong = studentMapper.selectCount(wrapper);
        System.out.println("aLong = " + aLong);
    }
```
---

**selectList**
```java
/**
 * 根据 entity 条件，查询全部记录
 * @param queryWrapper 实体对象封装操作类（可以为 null）
 * @return 实体集合
 */
List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
-------------------------------------------------------------
	@Test
    public void testSelectList(){
        List<Student> studentList = studentMapper.selectList(null);
        studentList.stream().forEach(System.out::println);
    }

```
---
**selectMaps**
```java
/**
 * 根据 Wrapper 条件，查询全部记录
 * @param queryWrapper 实体对象封装操作类（可以为 null）
 * @return 字段映射对象 Map 集合
 */
List<Map<String, Object>> selectMaps(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
-------------------------------------------------------------
	@Test
    public void testSelectMaps() {
        //将结果映射到Map集合中
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        wrapper.gt("age",17);
        List<Map<String, Object>> maps = studentMapper.selectMaps(wrapper);
        maps.stream().forEach(System.out::println);
    }

```
---

**selectObjs**
```java
/**
 * 根据 Wrapper 条件，查询全部记录
 * 注意： 只返回第一个字段的值
 * @param queryWrapper 实体对象封装操作类（可以为 null）
 * @return 字段映射对象集合
 */
List<Object> selectObjs(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
-------------------------------------------------------------
 	@Test
    public void testSelectObjs() {
        //只返回第一个字段的值
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        wrapper.eq("name","沸羊羊");
        List<Object> list = studentMapper.selectObjs(wrapper);
        list.forEach(System.out::println);
    }

```
---


**SelectPage**
```java
/**
 * 根据 entity 条件，查询全部记录（并翻页）
 * @param page         分页查询条件（可以为 RowBounds.DEFAULT）
 * @param queryWrapper 实体对象封装操作类（可以为 null）
 * @return 实体分页对象
 */
IPage<T> selectPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
-------------------------------------------------------------
@Test
    public void testSelectPage(){
        // 创建 Page 对象，设置当前页码和每页显示的记录数
        Page<Student> page = new Page<>(1, 10);
        QueryWrapper<Student> queryWrapper = new QueryWrapper<>();
        // 例如，这里我们添加一个查询条件：年龄大于18岁
        queryWrapper.gt("age", 18);

        List<Student> records = studentMapper.selectPage(page, queryWrapper).getRecords();
        records.stream().forEach(System.out::println);
    }
```
---

**selectMapsPage**
```java
/**
 * 根据 Wrapper 条件，查询全部记录（并翻页）
 * @param page         分页查询条件
 * @param queryWrapper 实体对象封装操作类
 * @return 字段映射对象 Map 分页对象
 */
IPage<Map<String, Object>> selectMapsPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
-------------------------------------------------------------
	@Test
    public void testSelectMapsPage() {
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        wrapper.like("name", "羊");
        // 执行分页查询
        Page page = new Page(1, 10);
        studentMapper.selectMapsPage(page,wrapper);
    }
```
---

### ServiceCRUD接口
>**通用ServiceCRUD封装IServiceCRUD接口，进一步封装 CRUD采用get查询单行remove删除list查询集合page分页 前缀命**


**这里详情可以参考IService.java  我将列举常用的CRUD方法** 
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6f23734a44524f0e90abef6121a14ede.png)

#### (1) 插入操作
**save**
```java
/**
 * 插入一条记录（选择字段，策略插入）
 * @param entity 实体对象
 */
boolean save(T entity);
-------------------------------------------------------------
	@Test
    public void testSave(){
        Student student = new Student();
        student.setName("双面龟");
        student.setAge(28);
        student.setUsername("999999");
        student.setPassword("999999");
        boolean save = studentService.save(student);
        System.out.println("save = " + save);
    }
```
---

**saveBatch**
```java
/**
 * 插入（批量）
 * @param entityList 实体对象集合
 * @param batchSize  插入批次数量
 */
boolean saveBatch(Collection<T> entityList, int batchSize);
-------------------------------------------------------------
	@Test
    public void testSaveBatch(){
        List<Student> studentList = new ArrayList<>();
        Student student1 = new Student();
        student1.setName("熊大");
        student1.setAge(19);
        student1.setUsername("789456");
        student1.setPassword("456789");
        studentList.add(student1);

        Student student2 = new Student();
        student2.setName("熊二");
        student2.setAge(20);
        student2.setUsername("456456");
        student2.setPassword("789789");

        studentList.add(student2);
        //当数据很多时   一次插入大量数据时间复杂度很高 按批次插入可以提高执行效率
        studentService.saveBatch(studentList,1);
        //数据不多时     可以直接插入
        //studentService.saveBatch(studentList);

    }

```
---

**saveOrUpdateBatch**
```java
/**
 * 批量修改插入
 * @param entityList 实体对象集合
 */
boolean saveOrUpdateBatch(Collection<T> entityList, int batchSize);
-------------------------------------------------------------
@Test
    public void testSaveOrUpdateBatch(){
        List<Student> studentList = new ArrayList<>();
        Student student1 = new Student();
        student1.setId(1851630555701927938L);
        student1.setName("熊大大");
        student1.setAge(29);
        student1.setUsername("789456");
        student1.setPassword("456789");
        studentList.add(student1);

        Student student2 = new Student();
        student2.setName("熊二二");
        student2.setAge(28);
        student2.setUsername("456456");
        student2.setPassword("789789");
        studentList.add(student2);

        //当数据很多时   一次插入大量数据时间复杂度很高 按批次插入可以提高执行效率
        //更改 id = 1851630555701927938L 数据
        //插入 studnet2数据
        studentService.saveOrUpdateBatch(studentList,1);
        //数据不多时     可以直接插入
        //studentService.saveOrUpdateBatch(studentList);
    }
```

#### (2) 删除操作
**removeById**
```java
/**
 * 根据 ID 删除
 * @param id 主键ID
 */
boolean removeById(Serializable id);
-------------------------------------------------------------
	@Test
    public void testRemoveById(){
        boolean b = studentService.removeById(3L);
        System.out.println("b = " + b);
    }
```
---
**removeByMap**
```java
/**
 * 根据 columnMap 条件，删除记录
 * @param columnMap 表字段 map 对象
 */
boolean removeByMap(Map<String, Object> columnMap);
-------------------------------------------------------------
	@Test
    public void testRemoveByMap(){
        Map<String,Object> map = new HashMap<>();
        //删除所有name=沸羊羊的 and age = 23
        map.put("name","沸羊羊");
        map.put("age",23);
        System.out.println(studentService.removeByMap(map));
    }
```
---
**remove**
```java
/**
 * 根据 entity 条件，删除记录
 * @param queryWrapper 实体包装类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
boolean remove(Wrapper<T> queryWrapper);
-------------------------------------------------------------
	@Test
    public void testRemove(){
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        //删除 name中包含 “ 二 ” 的 and age > 18
        wrapper.like("name","二").gt("age",18);
        System.out.println(studentService.remove(wrapper));
    }

```
---
**removeByIds**
```java
/**
 * 删除（根据ID 批量删除）
 * @param idList 主键ID列表
 */
boolean removeByIds(Collection<? extends Serializable> idList);
-------------------------------------------------------------
	@Test
    public void testRemoveByIds(){
        List<Long> ids = Arrays.asList(1L,2L,3L);
        System.out.println(studentService.removeByIds(ids));
    }

```
---
#### (3) 更新操作
**updateById**
```java
/**
 * 根据 ID 选择修改
 * @param entity 实体对象
 */
boolean updateById(T entity);
-------------------------------------------------------------
	@Test
    public void testUpdateById(){
        Student student = new Student();
        student.setId(1851424527429312513L);
        student.setName("红太狼");
        student.setAge(32);
        System.out.println(studentService.updateById(student));
    }

```
---
**update**
```java
/**
 * 根据 whereEntity 条件，更新记录
 * @param entity        实体对象
 * @param updateWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.update.UpdateWrapper}
 */
boolean update(T entity, Wrapper<T> updateWrapper);
-------------------------------------------------------------
	@Test
    public void testUpdate(){
        Student student = new Student();
        student.setAge(25);
        student.setPassword("******");

        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        //更改name=沸羊羊 and age<25 值 age = 25 password="******"
        wrapper.eq("name","沸羊羊").lt("age",25);
        System.out.println(studentService.update(student, wrapper));
    }
```
---
**updateBatchById**
```java
/**
 * 根据ID 批量更新
 * @param entityList 实体对象集合
 * @param batchSize  更新批次数量
 */
boolean updateBatchById(Collection<T> entityList, int batchSize);
-------------------------------------------------------------
	@Test
    public void testUpdateBatchById(){
        List<Student> studentList = new ArrayList<>();
        Student student1 = new Student();

        student1.setId(30L);
        student1.setName("吉吉国王");
        student1.setAge(19);
        studentList.add(student1);

        Student student2 = new Student();
        student2.setId(1029586945L);
        student2.setName("毛毛");
        student2.setAge(11);
        studentList.add(student2);

        //当数据很多时   一次插入大量数据时间复杂度很高 按批次插入可以提高执行效率
        studentService.updateBatchById(studentList,10);

    }
```
---
**saveOrUpdate**
```java
/**
 * TableId 注解存在更新记录，否插入一条记录
 * @param entity 实体对象
 */
boolean saveOrUpdate(T entity);
-------------------------------------------------------------
	@Test
    public void testSaveOrUpdate(){
        Student student = new Student();
        student.setName("熊二");
        student.setAge(66);
        studentService.saveOrUpdate(student);
    }
```
---
#### (4) 查询操作
**getById**
```java
/**
 * 根据 ID 查询
 * @param id 主键ID
 */
T getById(Serializable id);
-------------------------------------------------------------
    @Test
    public void testGetById(){
        System.out.println(studentService.getById(3L));
    }

```
---
**listByIds**
```java
/**
 * 查询（根据ID 批量查询）
 * @param idList 主键ID列表
 */
Collection<T> listByIds(Collection<? extends Serializable> idList);
-------------------------------------------------------------
    @Test
    public void testListByIds(){
        List<Long> ids = Arrays.asList(1L,2L,3L);
        List<Student> studentList = studentService.listByIds(ids);
        studentList.forEach(System.out::println);

    }
```
---
**listByMap**
```java
/**
 * 查询（根据 columnMap 条件）
 * @param columnMap 表字段 map 对象
 */
Collection<T> listByMap(Map<String, Object> columnMap);
-------------------------------------------------------------
    @Test
    public void testListByMap(){
        Map<String,Object> map = new HashMap<>();
        map.put("age",18);
        List<Student> studentList = studentService.listByMap(map);
        studentList.stream().forEach(System.out::println);
    }
```
---
**getOne**
```java
/**
 * 根据 Wrapper，查询一条记录
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 * @param throwEx      有多个 result 是否抛出异常
 */
T getOne(Wrapper<T> queryWrapper, boolean throwEx);
-------------------------------------------------------------
    @Test
    public void testGetOne(){
        QueryWrapper<Student> wrapper = new QueryWrapper<>();

        //last("limit 1") 限制每次查询 只查询一条数据
        wrapper.last("limit 1").eq("age",18);
        studentService.getOne(wrapper,true);

        //如果有多条数据符合查询条件 那么报错
        /*wrapper.eq("age",18);
        studentService.getOne(wrapper,true);*/

        //如果有多条数据符合查询条件 不会报错
        /*wrapper.eq("age",18);
        studentService.getOne(wrapper,false);*/
    }
```
---
**getMap**
```java
/**
 * 根据 Wrapper，查询一条记录
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
Map<String, Object> getMap(Wrapper<T> queryWrapper);
-------------------------------------------------------------
    @Test
    public void testGetMap(){
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        wrapper.eq("age",18).like("name","小");
        Map<String, Object> map = studentService.getMap(wrapper);
        System.out.println("map = " + map);
    }
```
---
**getObj**
```java
/**
 * 根据 Wrapper，查询一条记录
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
Object getObj(Wrapper<T> queryWrapper);
-------------------------------------------------------------
    @Test
    public void testGetObj(){
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        wrapper.eq("age",25);
        //转换函数 TODO
        Object obj = studentService.getObj(wrapper, Function.identity());
        System.out.println("obj = " + obj);
    }
```
---
**count**
```java
/**
 * 根据 Wrapper 条件，查询总记录数
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
int count(Wrapper<T> queryWrapper);
-------------------------------------------------------------
    @Test
    public void testCount(){
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        wrapper.eq("age",18).like("name","二");
        long count = studentService.count(wrapper);
        System.out.println(count);
    }
```
---
**list**
```java
/**
 * 查询列表
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
List<T> list(Wrapper<T> queryWrapper);
-------------------------------------------------------------
    @Test
    public void testList(){
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        wrapper.eq("age",25);
        List<Student> studentList = studentService.list(wrapper);
        studentList.stream().forEach(System.out::println);
    }
```
---
**listMaps**
```java
/**
 * 查询列表
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
List<Map<String, Object>> listMaps(Wrapper<T> queryWrapper);
-------------------------------------------------------------
    @Test
    public void testListMaps(){
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        wrapper.like("name","二");
        List<Map<String, Object>> maps = studentService.listMaps(wrapper);
        maps.stream().forEach(System.out::println);

    }
```
---
**listObjs**
```java
/**
 * 根据 Wrapper 条件，查询全部记录
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
List<Object> listObjs(Wrapper<T> queryWrapper);
-------------------------------------------------------------
    @Test
    public void testListObjs(){
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        wrapper.lt("age",25);
        List<Object> objects = studentService.listObjs(wrapper);
        //打印第一条数据
        objects.stream().forEach(System.out::println);
    }
```
---
#### (5) 分页查询
**page**
```java
/**
 * 翻页查询
 * @param page         翻页对象
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
IPage<T> page(IPage<T> page, Wrapper<T> queryWrapper);
-------------------------------------------------------------
    @Test
    public void testPage(){
        IPage<Student> iPage = new Page<>(1,10);
        //page(IPage<T> page)：无条件分页查询。
        studentService.page(iPage);

        //page(IPage<T> page, Wrapper<T> queryWrapper)：条件分页查询。
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        wrapper.like("name","羊").lt("age",25);
        studentService.page(iPage, wrapper);

    }
```
---
**pageMaps**
```java
/**
 * 翻页查询
 * @param page         翻页对象
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
IPage<Map<String, Object>> pageMaps(IPage<T> page, Wrapper<T> queryWrapper);
-------------------------------------------------------------
    @Test
    public void testPageMaps(){
        IPage<Map<String, Object>> page =new Page<>(1,10);
        //pageMaps(IPage<T> page)：无条件分页查询，返回Map类型的列表。
        //studentService.pageMaps(page);
        //pageMaps(IPage<T> page, Wrapper<T> queryWrapper)：条件分页查询，返回Map类型的列表。
        QueryWrapper<Student> wrapper = new QueryWrapper<>();
        wrapper.like("name","羊").lt("age",25);
        studentService.pageMaps(page, wrapper);

    }
```
---
## 4. 逻辑删除
>逻辑删除是为了方便数据恢复和保护数据本身价值等等的一种方案，但实际就是删除。
>如果需要再查出来就不应使用逻辑删除，而是以一个状态去表示


**SpringBoot 配置方式**
>application.yml 加入配置(如果你的默认值和mp默认的一样,该配置可无)

```yml
mybatis-plus:
  global-config:
    db-config:
      logic-delete-value: 1 # 逻辑已删除值(默认为 1)
      logic-not-delete-value: 0 # 逻辑未删除值(默认为 0)
```

>实体类字段上加上`@TableLogic`注解
```java
//@TableLogic
@TableLogic(value = "0", delval = "1")
//@TableField(fill = FieldFill.INSERT)
private Integer deleted;
```
**效果**
```sql
# 删除时 
update student set deleted=1 where id =1 and deleted=0
# 查找时 
select * from student where deleted=0
```
## 5. 自动填充功能
>实现元对象处理器接口：**com.baomidou.mybatisplus.core.handlers.MetaObjectHandler**
注解填充字段 @TableField(fill = FieldFill.INSERT)

**自定义实现类 MyMetaObjectHandler**
```java
@Component
public class MyMetaObjectHandler implements MetaObjectHandler {
    @Override
    public void insertFill(MetaObject metaObject) {
    	//每次插入新数据时 deleted 不用特别声明 新数据中的deleted字段会自动填充为0
        this.strictInsertFill(metaObject, "deleted", Integer.class, 0);
    }
    @Override
    public void updateFill(MetaObject metaObject) {

    }
}
```

**注意事项**：
>**字段必须声明`TableField`注解，属性fill选择对应策略，该申明告知 `Mybatis-Plus` 需要预留注入 SQL 字段**
>**填充处理器`MyMetaObjectHandler` 在Spring Boot中需要声明`@Component` 注入**
>**必须使用父类的`setFieldValByName()`或者`setInsertFieldValByName`/`setUpdateFieldValByName`方法，否则不会根据注解`FieldFill.xxx`来区分**

## 6. 分页插件
**配置分页插件**
```java
@Configuration
public class MybatisPlusConfig {
    /**
     * 分页插件
     */
    @Bean
    public MybatisPlusInterceptor paginationInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        /// 向MyBatis-Plus的过滤器链中添加分页拦截器，需要设置数据库类型（主要用于分页方言）
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }
}
```
>然后通过`Page<T>`类就可以使用了 和上面分页例子使用方式一样


## 7. 自定义ID生成器
### TableId
**IdType**
```java
@TableId(type = IdType.ASSIGN_ID)
private Long id;
```
名称|说明
--|--
AUTO|数据库ID自增
INPUT|用户手动输入ID
NONE|该类型为未设置主键类型
ASSIGN_ID|使用雪花算法分配ID，主键类型为Number(Long和Integer)或String
ASSIGN_UUID|分配UUID,主键类型为String

### 雪花算法


**雪花算法参数设置**
参数名称|说明
-|-
时间戳|占41位，精确到毫秒级。
工作机器ID|占10位，支持最多1024台机器。
数据中心ID|占5位，支持最多32个数据中心。
序列号|占12位，支持每台机器每毫秒产生4096个ID。

**ID生成过程**
1. 获取当前时间戳：首先，获取当前的时间戳（精确到毫秒）。
2. 提取数据中心ID和工作机器ID：然后，从配置中提取数据中心ID和工作机器ID。
3. 生成序列号：在同一毫秒内，如果已经有ID生成，则序列号自增；如果序列号达到4096，则等待下一毫秒。
4. 组合ID：最后，将时间戳、数据中心ID、工作机器ID和序列号按照雪花算法的规则组合成一个64位的唯一ID。

**样例**
>假设当前时间戳为1699142400000 数据中心ID为00001，工作机器ID为0000000001，当前毫秒内的序列号为0001。

| 符号位(1位) | 时间戳(41位)            | 数据中心ID(5位) | 工作机器ID(10位) | 序列号(12位) |  
|-------------|-------------------------|----------------|------------------|--------------|  
| 0           | 1699142400000     | 00001        | 0000000001       | 0001         |

>将上述各部分组合成一个64位的唯一ID（以十六进制表示）：
>0000000000000000 | 0001699142400000 | 00001 | 0000000001 | 0001

### UUID
**关键部分**
名称|说明
-|-
时间戳|UUID的第一个部分通常与时间有关，用于确保每个UUID都是唯一的。
时钟序列号|这是当前计数器的值，当时间戳发生变化时，时钟序列号会重新开始计数。
全局唯一标识|这通常是一个固定值，如计算机名、网络地址或MAC地址等，用于进一步确保UUID的唯一性。
版本号|UUID还包含一个版本号字段，用于指示生成UUID时所使用的算法版本。

**UUID的生成机制**
>UUID的生成机制基于多种因素，包括时间戳、生成者的唯一标识符（如MAC地址）和硬件信息等。这些因素共同确保了UUID在极小概率下出现重复的可能性。

**UUID的生成算法可能包括**
算法|说明
-|-
基于时间的UUID|这种算法通过计算当前时间戳、随机数和机器MAC地址来生成UUID。<br>由于使用了MAC地址，这种UUID可以保证在全球范围的唯一性。但使用MAC地址也可能带来安全性问题。
DCE安全的UUID|这种算法与基于时间的UUID类似，但会将时间戳的前4位置换为POSIX的UID或GID。
基于名字的UUID|这种算法通过计算名字和名字空间的MD5散列值或SHA-1散列值来生成UUID。<br>这种UUID保证了相同名字空间中不同名字生成的UUID的唯一性，以及不同名字空间中的UUID的唯一性。
随机UUID|这种UUID是通过随机数或伪随机数生成器生成的。<br>虽然理论上存在重复的可能性，但在实际应用中，其重复的概率极低。

**优点**
>**唯一性：UUID在时间和空间上都是唯一的，这确保了它们在不同系统、不同时间生成的UUID不会重复。**
>**去中心化：UUID的生成不需要中心管理机构的介入，这降低了系统的复杂性和成本。**
>**持久性：一旦生成，UUID就不会改变，这确保了它们的持久性和稳定性。**

**缺点**
>**长度较长：UUID的长度为128位（或表示为32个十六进制字符），这在某些应用场景下可能显得过长。**
>**性能问题：在大量生成和使用UUID的场景下，可能会对系统的性能产生一定影响。**

# 三、MyBatis与Mybatis-plus两者区别
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/c59cbd910233d1bb89fbaaa4ab9fc102.png#pic_center =1900x)
>MyBatis-Plus是MyBatis的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。
>MyBatis-Plus是MyBatis 最好的搭档，就像 魂斗罗 中的 1P、2P，基友搭配，效率翻倍

## 特点 
**mybatis**
1. MyBatis就是一个ORM框架。当然，也是一个持久层框架。
2. MyBatis封装了JDBC， 将数据库中的表数据自动封装到对象中。代码精简易读。
2. MyBatis 是支持普通 SQL查询，存储过程和高级映射的优秀持久层框架。
3. MyBatis 消除了几乎所有的JDBC代码和参数的手工设置以及结果集的检索。
4. MyBatis 使用简单的 XML或注解用于配置和原始映射，将接口和 Java 的POJO 映射成数据库中的记录。

**mybatis-plus:**
1. 只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑。 
2. 只需简单配置，即可快速进行单表 CRUD 操作，从而节省大量时间。 
3. 自动分页、逻辑删除、自动填充等功能一应俱全



## 功能实现
**mybatis**
1. 需要手写大量的SQL以完成各种功能的实现。
2. 编程风格更加传统，需要定义mapper.xml文件，并根据传入的参数使用相应的SQL查询语句。
3. 提供了两级缓存机制，但相对简单。
4. 支持动态SQL，允许开发者根据条件构建不同的SQL语句。


**mybatis-plus:**
1. 提供了很多额外的功能，如条件构造器、代码生成器、分页插件、性能分析拦截器等。
2. 通过继承BaseMapper以及使用Lambda表达式，可以像Spring Data JPA类似地使用接口编程方式进行数据库操作。
3. 自动实现CRUD方法，减少代码编写
4. 提供了QueryWrapper等工具，方便构建复杂的查询条件。
6. 支持分页查询，简化分页操作。
7. 有效预防SQL注入攻击

## 性能和效率
**mybatis**
1. 由于直接使用JDBC，性能相对较好。
2. 但需要手动编写SQL语句，对于复杂的查询可能会增加工作量。

**mybatis-plus:**
1. 在性能上基本无损耗，直接面向对象操作。
2. 提供了内置的性能分析插件，可以输出每条SQL语句及其执行时间，帮助开发者发现并优化慢查询。
3. 支持批量操作，并且在某些情况下，可以显著提高批量插入的性能。

## 灵活性
**mybatis**
1. 由于直接使用JDBC，性能相对较好。
2. 但需要手动编写SQL语句，对于复杂的查询可能会增加工作量。

**mybatis-plus:**
1. 虽然提供了很多便利，但在某些复杂业务场景下可能不够灵活。
2. 自动化和简化操作可能难以满足特定的查询或操作需求。


## 学习难度

**mybatis**
1. 学习曲线相对平缓，文档齐全，易于上手。
2. 但需要手动编写SQL语句，对于初学者来说可能有一定的学习成本

**mybatis-plus:**
1. 提供了丰富的功能和API，可能需要一定的学习时间。
2. 尤其是对于初学者来说，需要了解MyBatis-Plus的特性和使用方式。






