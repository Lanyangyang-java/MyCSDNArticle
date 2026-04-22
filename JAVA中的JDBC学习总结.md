@[TOC](JDBC学习总结 我的学习笔记)

# 一、JDBC 简介
**1.JDBC 概念**
1. JDBC 就是使用Java语言操作关系型数据库的一套API
2. 全称：( Java DataBase Connectivity ) Java 数据库连接
3. 同一套Java代码，操作不同的关系型数据库——一套标准接口


**2.JDBC 本质**
1. 官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口
2. 各个数据库厂商去实现这套接口，提供数据库驱动jar包
3. 我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类

**3.JDBC 好处**
1. 各数据库厂商使用相同的接口，Java代码不需要针对不同数据库分别开发
2. 可随时替换底层数据库，访问数据库的Java代码基本不变

![请添加图片描述](https://i-blog.csdnimg.cn/direct/a90c1460e7204e0998f60e2ac99a641c.png)

# 一、JDBC 快速入门
**步骤**
样例一：
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


//运行结果
/**
1,张三,18
2,蔡徐坤,16
3,猪猪侠,19
4,常豪强,20
*/
```
数据库数据studen表
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c3e4001a6ba742c1a6b0364ecfcb4562.png)

# 一、JDBC API 详解
## 1.DriverManager
**DriverManager(驱动管理类)作用** 
1. 注册驱动   
2. 获取数据库连接

**注册驱动**
```java
Class.forName("com.mysql.jdbc.Driver");
```

**注意：**
MySQL 5之后的驱动包，可以省略注册驱动的步骤
自动加载jar包中META-INF/services/java.sql.Driver文件中的驱动类

**获取连接**
```java
 getConnection(url,username,password);
```

 参数 
**url：** 连接路径
1. 语法 jdbc:mysql://ip地址(域名):端口号/数据库名称?参数键值对1&参数键值对2
2. 示例 jdbc:mysql://127.0.0.1:3306/test
3. 细节 
 如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为：jdbc:mysql:///数据库名称?参数键值对    	  
 配置 useSSL=false 参数，禁用安全连接方式，解决警告提示

**username：** 数据库用户名
**password：** 数据库密码
## 2.Connection
**Connection(数据库连接对象)作用**
1. 获取执行 SQL 的对象
2. 管理事务

**获取执行 SQL 的对象**
```java
//普通执行SQL对象
Statement createStatement()

//预编译SQL的执行SQL对象：防止SQL注入
PreparedStatement prepareStatement (sql)
```

**JDBC 事务管理：Connection接口中定义了3个对应的方法**
1. 开启事务：setAutoCommit(boolean autoCommit)：true为自动提交事务；false为手动提交事务，即为开启事务
2. 提交事务 commit();
3. 回滚事务 rollback();
## 3.Statement
**Statement作用:** 执行SQL语句

**执行SQL语句**
```java
//执行DML、DDL语句
int executeUpdate(sql)
//返回值：(1) DML语句影响的行数 (2) DDL语句执行后，执行成功也可能返回0



//执行DQL 语句
ResultSet executeQuery(sql)
//返回值： ResultSet 结果集对象
```
  
## 4.ResultSet
**ResultSet(结果集对象)作用:** 封装了DQL查询语句的结果
```java
执行DQL 语句，返回 ResultSet 对象
ResultSet resultSet = stmt.executeQuery(sql);
```
**获取查询结果**
**1.boolean next()**
  1. 将光标从当前位置向前移动一行
  2. 判断当前行是否为有效行
  3. 返回值
   true：有效行，当前行有数据
   false：无效行，当前行没有数据

**2.xxx getXxx(参数)** (这里可以参考上面的样例)
1. 获取数据
2. xxx：数据类型；如：int getInt(参数) ; String getString(参数)
3. 参数
    int：列的编号，从1开始
    String：列的名称

**使用步骤**
游标向下移动一行，并判断该行否有数据：next()
获取数据：getXxx(参数)
循环判断是否是最后一行末尾
```java
while(rs.next()){
	//获取数据
	rs.getXxx(参数);
}
```

## 5.PreparedStatement
**PreparedStatement作用：** 预编译SQL语句并执行：预防SQL注入问题

**SQL注入：** SQL注入是通过操作输入来修改事先定义好的SQL语句，用以达到执行代码对服务器进行攻击的方法

**PreparedStatement 原理**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b41de46a4fcd445daa6aa08bc6b3ea90.png)


PreparedStatement 好处：
1. 在获取PreparedStatement对象时，将sql语句发送给mysql服务器进行检查，编译（这些步骤很耗时）
2. 执行时就不用再进行这些步骤了，速度更快
3. 如果sql模板一样，则只需要进行一次检查、编译




**PreparedStatement使用：**
4. 获取 PreparedStatement 对象
```java
// SQL语句中的参数值，使用？占位符替代
String sql = "select * from student where name = ?";

// 通过Connection对象获取，并传入对应的sql语句
PreparedStatement pstmt = conn.prepareStatement(sql);
```
5. 设置参数值
 **PreparedStatement对象：** setXxx(参数1，参数2)：给 ? 赋值
   **Xxx：** 数据类型 ； 如 setInt (参数1，参数2)
   **参数**
   参数1： ？的位置编号，从1 开始
   参数2： ？的值

6. 执行SQL
```java
executeUpdate(); 或 executeQuery(); ：不需要再传递sq
```

给上述student表新增username 和 password两个字段

样例二：
```java
public class PreparedStatementTest {
    public static void main(String[] args) throws Exception{
        //1.注册驱动
        //Class.forName("com.mysql.jdbc.Driver");
        //2.获取连接
        String url="jdbc:mysql:///test?useSSL=false";
        String username="root";
        String password="1234";
        Connection conn = DriverManager.getConnection(url, username, password);
        //接收用户输入的用户名和密码
        String name="123456";
        String pwd="'or'1'='1";
        //3.定义sql
        String sql="select * from student where username=? and password=?";

        //4.获取执行sql的对象
        PreparedStatement pstmt = conn.prepareStatement(sql);
        pstmt.setString(1,name);
        pstmt.setString(2,pwd);
        //5.执行sql语句
        ResultSet rs = pstmt.executeQuery();
        //6.处理结果
        if (rs.next()){
            System.out.println("登录成功！");
        }else{
            System.out.println("登录失败！");
        }
        //7.释放资源
        rs.close();
        pstmt.close();
        conn.close();
    }
}
```

我们会发现和上面的样例有些许不同
1. 如果使用Statement那么**pwd="'or'1'='1"** 无论是和值都会登录成功
2. 使用PreparedStatement能够很好的避免这个问题，动态地把参数传给PreprareStatement时，即使参数里有敏感字符如上图中 or ‘1=1’，它也会作为一个字段的值来处理，而不会作为一个SQL指令，从而杜绝Sql注入的问题

# 一、数据库连接池
## 1.数据库连接池简介
 1. 数据库连接池是个容器，负责分配、管理数据库连接(Connection)
 2. 它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个；
 3. 释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏
 4. 好处
  资源重用
  提升系统响应速度
  避免数据库连接遗漏
## 2.数据库连接池实现
1.  标准接口
  DataSource
  官方(SUN) 提供的数据库连接池标准接口，由第三方组织实现此接口
  功能：获取连接——Connection getConnection()
2. 常见的数据库连接池：
  DBCP
  C3P0
  Druid
3. Druid(德鲁伊)
  Druid连接池是阿里巴巴开源的数据库连接池项目
  功能强大，性能优秀，是Java语言最好的数据库连接池之一## 3.Druid 数据库连接池


## 3.Druid 数据库连接池
1.  导入jar包  使用Maven工程导入依赖即可
```java
		<dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.18</version>
        </dependency>
```
2. 定义配置文件
3. 加载配置文件
4. 获取数据库连接池对象
5. 获取连接

样例：

**druid.properties**
```java
# 驱动名称
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/test?useSSL=false&useServerPrepStmts=true
username=root
password=1234
#初始化时建立物理连接的个数
initialSize=5
# 最大链接数
maxActive=10
# 最大等待时间
maxWait=3000

```

测试获取链接
```java
public class DruidTest {
    public static void main(String[] args) throws Exception {
        //1.导入jar包
        //2.定义配置文件
        //3.加载配置文件
        Properties prop=new Properties();
        prop.load(new FileInputStream("JDBCDemo/src/main/resources/druid.properties"));
        //4.获取连接池对象
        DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);
        //5.获取数据库连接
        Connection connection = dataSource.getConnection();
        System.out.println(connection);
    }
}
```

