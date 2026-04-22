@[TOC](JavaSE   JAVA基础总结 （下）  )



java基础总结上在这：[JavaSE JAVA基础总结（上） 我的学习笔记](https://blog.csdn.net/qq_60972641/article/details/142644807?spm=1001.2014.3001.5501)
# 一、API
## （1）常用API
### Object

**Object类的作用：**
Object类的方法是一切子类对象都可以直接使用的，所有的类都继承了Object类，Object类是Java中的祖宗类

Object类的常用方法：
|方法名|说明|
|--|--|
|public String toString()|默认是返回当前对象在堆内存中的地址信息:类的全限名@内存地址|
|public boolean equals(Object o)|默认是比较当前对象与另一个对象的地址是否相同，相同返回true，不同返回false|

1. 开发中直接输出对象，默认输出对象的地址其实是毫无意义的，更多的时候是希望看到对象的内容数据
2. 父类toString()方法存在的意义就是为了被子类重写，以便返回对象的内容信息，而不是地址信息
3. 直接比较两个对象的地址是否相同完全可以用“==”替代equals。
4. 父类equals方法存在的意义就是为了被子类重写，以便子类自己来定制比较规则。

Object的toString方法的作用：让子类重写，以便返回子类对象的内容
Object的equals方法的作用：默认是与另一个对象比较地址是否一样，让子类重写，以便比较2个子类对象的内容是否相同

**样例：**
```java
public class Student {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                '}';
    }
    
}

public class Test {
    public static void main(String[] args) {
        Student s1 = new Student("张三");
        Student s2 = new Student("张三");

        //如果不重写toString方法则放回的是地址
        System.out.println(s1);
        System.out.println(s1.toString());
        System.out.println(s1.equals(s2)); //false
        System.out.println(s1 == s2); //false
    }
}

```

### Objects
Objects是一个**工具类**，提供了一些方法去完成一些功能，官方在进行字符串比较时，没有用字符串对象的的equals方法，而是选择了Objects的equals方法来比较。

**Objects的常见方法：**
|方法名|说明|
|--|--|
|public static boolean equals(Object a, Object b)|比较两个对象的，底层会先进行非空判断，从而可以避免空指针异常。再进行equals比较|
|public static boolean isNull(Object obj)|判断变量是否为null ,为null返回true ,反之|

**样例：**
```java
@Override
public boolean equals(Object o) {
	// 1、判断是否是同一个对象比较，如果是返回true。
	if (this == o) return true;
	// 2、如果o是null返回false 如果o不是学生类型返回false ...Student != ..Pig
	if (o == null || this.getClass() != o.getClass()) return false;
	// 3、说明o一定是学生类型而且不为null
	Student student = (Student) o;
	return Objects.equals(name, student.name);
}

//此时 Test中
System.out.println(s1.equals(s2)); //true
System.out.println(s1 == s2); //false
//非空判断
System.out.println(Objects.isNull(s1));
```
对象进行内容比较的时候建议使用Objects提供的equals方法,比较的结果是一样的，但是更安全。
### StringBuilder
StringBuilder是一个可变的字符串类，我们可以把它看成是一个对象容器。
**作用：** 提高字符串的操作效率，如拼接、修改等

**StringBuilder 构造器：**
|名称|说明|
|--|--|
|public StringBuilder()| 创建一个空白的可变的字符串对象，不包含任何内容|
|public StringBuilder(String str)|创建一个指定字符串内容的可变字符串对象|

**StringBuilder常用方法：**
|方法名|说明|
|--|--|
|public StringBuilder append(任意类型) |添加数据并返回StringBuilder对象本身|
|public StringBuilder reverse() |将对象的内容反转|
|public int length()| 返回对象内容长度|
|public String toString()|通过toString()|就可以实现把StringBuilder转换为String|

**样例：**
```java
public class Test {
    public static void main(String[] args) {
        StringBuilder sb1=new StringBuilder();
        //append方法
        sb1.append("a");
        sb1.append(1);
        sb1.append(0.1);
        sb1.append(true);
        System.out.println(sb1);
        StringBuilder sb2=new StringBuilder();
        //链式编程
        sb2.append("a").append("b").append("c");
        System.out.println(sb2);
        //reverse方法
        sb2.reverse().append("d");
        System.out.println(sb2);
        //length方法
        System.out.println(sb2.length());
        //toString方法
        String s=sb2.toString();
        System.out.println(s);
    }
}
```

### Math
包含执行基本数字运算的方法，Math类没有提供公开的构造器
**Math 类的常用方法：**
|方法名|说明|
|--|--|
|public static int abs (int a)| 获取参数绝对值|
|public static double ceil (double a) |向上取整|
|public static double floor (double a) |向下取整|
|public static int round (float a)| 四舍五入|
|public static int max (int a,int b) |获取两个int值中的较大值|
|public static double pow (double a,double b) |返回a的b次幂的值
|public static double random ()| 返回值为double的随机值，范围[0.0,1.0)

**样例：**
```java
public class Test {
    public static void main(String[] args) {
        System.out.println(Math.abs(10));//10
        System.out.println(Math.abs(-10));//10
        System.out.println(Math.ceil(4.000001));//5.0
        System.out.println(Math.floor(4.99999));//4.0
        System.out.println(Math.pow(2, 3));//8.0
        System.out.println(Math.round(4.49999));//4
        System.out.println(Math.random());//[0.0,1.0)
    }
}
```
### System
System也是一个工具类，代表了当前系统，提供了一些与系统相关的方法。
**System 类的常用方法：**
|方法名|说明|
|--|--|
|public static void exit(int status)| 终止当前运行的 Java 虚拟机，非零表示异常终止|
|public static long currentTimeMillis()| 返回当前系统的时间毫秒值形式|
|public static void arraycopy(数据源数组, 起始索引, 目的地数组, 起始索引, 拷贝个数)|数组拷贝|

计算机认为时间是有起点的，起始时间： 1970年1月1日 00:00:00 ，时间毫秒值：指的是从1970年1月1日 00:00:00走到此刻的总的毫秒数，

**样例：**
```java
public class Test {
    public static void main(String[] args) {
        System.out.println("程序开始");
        //System.exit(0); //直接结束当前程序
        System.out.println("程序结束");
        
        System.out.println(System.currentTimeMillis()); //1728955278053
        int[]arr1={1,2,3,4,5,6,7};
        int[] arr2=new int[6];
        System.arraycopy(arr1,3,arr2,2,3);
        System.out.println(Arrays.toString(arr2)); //[0,0,4,5,6,0]
    }
}

```
### BigDecimal
用于解决浮点型运算精度失真的问题
**BigDecima 类的常用方法：**
|方法名|说明|
|--|--|
|public BigDecimal add(BigDecimal b)| 加法|
|public BigDecimal subtract(BigDecimal b) |减法|
|public BigDecimal multiply(BigDecimal b)| 乘法|
|public BigDecimal divide(BigDecimal b) |除法|
|public BigDecimal divide (另一个BigDecimal对象，精确几位，舍入模式) |除法|


**样例：**
```java
public class Test {
    public static void main(String[] args) {
        System.out.println(0.09+0.01);
        double a=0.1;
        double b=0.2;
        System.out.println(a+b);
        BigDecimal a1=BigDecimal.valueOf(a);
        BigDecimal b1=BigDecimal.valueOf(b);
        System.out.println(a1.add(b1));
        System.out.println(a1.subtract(b1));
        System.out.println(a1.multiply(b1));
        System.out.println(a1.divide(b1));
    }
}

```

## (2) 日期与时间
### Date
Date类代表当前所在系统的日期时间信息。
**Date的构造器:**
|名称|说明|
|--|--|
public Date() |创建一个Date对象，代表的是系统当前此刻日期时间。
public Date(long time) |把时间毫秒值转换成Date日期对象


**Date的常用方法:**
|方法名|说明|
|--|--|
public long getTime()| 返回从1970年1月1日 00:00:00走到此刻的总的毫秒数
public void setTime(long time)| 设置日期对象的时间为当前时间毫秒值对应的时间

**样例：**
```java
public class DateTest {
    public static void main(String[] args) {
        //1.创建一个Date类的对象
        Date d = new Date();
        System.out.println(d); //Tue Oct 15 09:44:47 CST 2024
        //2.getTime方法获取时间毫秒值
        long time = d.getTime();
        System.out.println(time);//1728956687848
        //当前时间往后走1小时11s
        time += (60 * 60 + 11) * 1000;
        //3.Date(long time)构造器
        Date d2 = new Date(time);
        System.out.println(d2);//Tue Oct 15 10:44:58 CST 2024
        //4.setTime(long time)方法
        Date d3 = new Date();
        d3.setTime(time);
        System.out.println(d3); //Tue Oct 15 10:44:58 CST 2024
    }
}
```
### SimpleDateFormat
 可以去完成日期时间的格式化操作 如将Data类型 或 时间毫秒值类型转换为 人习惯看到的日期时间
 
**SimpleDateFormat构造器：**
|名称|说明|
|--|--|
public SimpleDateFormat(String pattern) |构造一个SimpleDateFormat，使用指定的格式

**SimpleDateFormat常用方法：**
|方法名|说明|
|--|--|
public final String format(Date date) |将日期格式化成日期/时间字符串
public final String format(Object time) |将时间毫秒值格式化成日期/时间字符串
public Date parse(String source) |从给定字符串的开始解析文本以生成日期

```java
public class SdfTest {
    public static void main(String[] args) throws ParseException {
        //1.创建日期对象
        Date d1=new Date();
        System.out.println(d1);//Tue Oct 15 09:48:35 CST 2024
        //2.格式化这个日期对象
        SimpleDateFormat sdf=new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss EEE a");
        System.out.println(sdf.format(d1));//2024年10月15日 09:48:35 星期二 上午
        String dateStr="2024年10月15日 9:48:55";
        SimpleDateFormat sdf2=new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
        Date d2=sdf2.parse(dateStr);
        //3.往后走了2天2小时2分2秒
        long time=d2.getTime()+(2L*24*60*60+2*60*60+2*60+2)*1000;
        System.out.println(sdf2.format(time));//2024年10月17日 11:50:57
    }
}
```
### Calendar
Calendar代表了系统此刻日期对应的日历对象，Calendar是一个抽象类，不能直接创建对象。

**Calendar常用方法：**
|方法名|说明|
|--|--|
public static Calendar getInstance()| 获取当前日历对象
public int get(int field)| 取日期中的某个字段信息。
public void add(int field,int amount) |为某个字段增加/减少指定的值
public final Date getTime() |拿到此刻日期对象。
public long getTimeInMillis() |拿到此刻时间毫秒值

```java
public class CalendarTest {
    public static void main(String[] args) {
        //创建一个日历对象
        Calendar cal=Calendar.getInstance();
        //获取年份...
        System.out.println(cal.get(Calendar.YEAR));//2024
        System.out.println(cal.get(Calendar.MONTH)+1);//月份是从0开始的 所有要加+ //10
        System.out.println(cal.get(Calendar.DAY_OF_YEAR));//289

        cal.add(Calendar.DAY_OF_YEAR,10);
        System.out.println(cal.get(Calendar.DAY_OF_YEAR));//299

        System.out.println(cal.getTimeInMillis());//1729821133491

    }
}
```
## (3) JDK8之后新增日期类
### LocalTime /LocalDate / LocalDateTime
分别表示日期，时间，日期时间对象，他们的类的实例是不可变的对象，他们三者构建对象和API都是通用的。
**常用方法：**
|方法名|说明|
|--|--|
public static Xxxx now();| 静态方法，根据当前时间创建对象
public static Xxxx of(…); |静态方法，指定日期/时间创建对象
public int geYear() |获取年
public int getMonthValue()| 获取月份（1-12）
Public int getDayOfMonth() |获取月中第几天
Public int getDayOfYear() |获取年中第几天
Public DayOfWeek getDayOfWeek()| 获取本周第几天

**LocalDateTime的转换API**
|方法名|说明|
|--|--|
public LocalDate toLocalDate()| 转换成一个LocalDate对象
public LocalTime toLocalTime()| 转换成一个LocalTime对象

**样例：**
```java
public class Test {
    public static void main(String[] args) {
        LocalDate nowDate=LocalDate.now();
        System.out.println(nowDate);
        System.out.println(nowDate.getYear());
        System.out.println(nowDate.getMonthValue());
        System.out.println(nowDate.getDayOfMonth());
        System.out.println(nowDate.getDayOfWeek().getValue());
        LocalDate ld=LocalDate.of(2021,11,11);
        System.out.println(ld);

        LocalTime nowTime = LocalTime.now();
        System.out.println(nowTime);
        System.out.println(nowTime.getHour());
        System.out.println(nowTime.getMinute());
        System.out.println(nowTime.getSecond());
        System.out.println(nowTime.getNano());

        LocalDateTime nowDateTime=LocalDateTime.now();
        System.out.println(nowDateTime);
        System.out.println(nowDateTime.getYear());
        //转换LocalDate
        LocalDate ld2=nowDateTime.toLocalDate();
        System.out.println(ld2);
        //转换LocalTime
        LocalTime lt=nowDateTime.toLocalTime();
        System.out.println(lt);
    }
}

```
![请添加图片描述](https://i-blog.csdnimg.cn/direct/25f66fb290fd4a40925a9f60a32cd353.png)
### DateTimeFormatter

在JDK8中，引入了一个全新的日期与时间格式器DateTimeFormatter，正反都能调用format方法。


```java
public class Demo04DateTimeFormat {
    public static void main(String[] args) {
        LocalDateTime ldt=LocalDateTime.now();
        System.out.println(ldt); //2024-10-16T08:32:28.411
        DateTimeFormatter dtf=DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss EEE a");
        //正反都能调用format方法
        //正向格式化
        System.out.println(dtf.format(ldt));//2024-10-16 08:32:28 星期三 上午
        //逆向格式化
        System.out.println(ldt.format(dtf));//2024-10-16 08:32:28 星期三 上午
        //解析字符串时间
        DateTimeFormatter dtf1=DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        LocalDateTime ldt1=LocalDateTime.parse("2024-10-16 11:11:11",dtf1);
        System.out.println(ldt1);//2024-10-16T11:11:11
        System.out.println(ldt1.getDayOfYear());//290
    }
}
```
### Period、Duration
来计算日期间隔差异，主要是 Period 类方法 getYears()，getMonths() 和 getDays() 来计算,只能精确到年月日。
```java
LocalDate today = LocalDate.now();
System.out.println(today);
LocalDate birthDate = LocalDate.of(1949, 10, 1);
System.out.println(birthDate);
Period period = Period.between(birthDate, today);
System.out.printf(period.getYears());
```

```java
LocalDateTime today = LocalDateTime.now();
System.out.println(today);
LocalDateTime birthDate = LocalDateTime.of(1949,10,1,0,0,0);
System.out.println(birthDate);
Duration duration = Duration.between(birthDate, today);//第二个参数减第一个参数
System.out.println(duration.toDays());//两个时间差的天数
System.out.println(duration.toHours());//两个时间差的小时数
System.out.println(duration.toMinutes());//两个时间差的分钟数
System.out.println(duration.toMillis());//两个时间差的毫秒数
System.out.println(duration.toNanos());//两个时间差的纳秒数
```
## (4) 包装类
其实就是8种基本数据类型对应的引用类型。
基本数据类型 |引用数据类型
-|-
byte| Byte
short| Short
int |Integer
long |Long
char| Character
float| Float
double| Double
boolean |Boolean

Java为了实现一切皆对象，为8种基本类型提供了对应的引用类型。集合和泛型其实也只能支持包装类型，不支持基本数据类型。

**自动装箱：** 基本类型的数据和变量可以直接赋值给包装类型的变量。
**自动拆箱:** 包装类型的变量可以直接赋值给基本数据类型的变量

**包装类的特有功能**
1. 包装类的变量的默认值可以是null，容错率更高。
2. 可以把基本类型的数据转换成字符串类型(用处不大)
调用toString()方法得到字符串结果。
调用Integer.toString(基本类型的数据)。
3. 可以把字符串类型的数值转换成真实的数据类型（真的很有用）
Integer.parseInt(“字符串类型的整数”)
Double.parseDouble(“字符串类型的小数”)。


## (5) 正则表达式
正则表达式可以用一些规定的字符来制定规则，并用来校验数据格式的合法性。

常用正则表达式可以参考:
<img src="https://img-blog.csdnimg.cn/img_convert/5f62d72eeeeb84f1bdb9652ae5751784.jpeg">

正则表达式在字符串方法中的使用
方法名| 说明
|--|--|
public String replaceAll(String regex,StringnewStr)| 按照正则表达式匹配的内容进行替换
public String[] split(String regex) |按照正则表达式匹配的内容进行分割字符串，反回一个字符串数组。

举个例子：
```java
private static void checkEmail() {
        Scanner scanner = new Scanner(System.in);
        while(true){
            System.out.println("请输入您的邮箱：");
            String email = scanner.next();
            //判断手机号码格式是否正确
            //12345a6@qq.com     1234567@pci.com.cn
            if(email.matches("\\w{1,30}@[a-zA-Z0-9]{2,20}(\\.[a-zA-Z0-9]{2,20}){1,2}")){
                System.out.println("邮箱格式正确，注册完成！");
                break;

            }else {
                System.out.println("格式有误");
            }
        }
```
可以通过此网站进行快速创建正则表达式语句： [https://www.sojson.com/regex/generate.html](https://www.sojson.com/regex/generate.html)
## (6) Arrays类
### Arrays类概述，常用功能演示
数组操作工具类，专门用于操作数组元素的

**Arrays类的常用API：**

方法名 |说明
|--|--|
public static String toString(类型[] a) |返回数组的内容（字符串形式）
public static void sort(类型[] a) |对数组进行默认升序排序
public static <T> void sort(类型[] a, Comparator<? superT> c)| 使用比较器对象自定义排序
public static int binarySearch(int[] a, int key) |二分搜索数组中的数据，存在返回索引，不存在返回负数
### Arrays类对于Comparator比较器的支持
用于排序

**Arrays类的排序方法**
方法名 |说明
--|--
public static void sort(类型[] a) |对数组进行默认升序排序
public static <T> void sort(类型[] a, Comparator<? super T> c)| 使用比较器对象自定义排序

```java
//封装一个学生类 里面有name 和age两个属性
public class Test {
    public static void main(String[] args) {
        Student[] students=new Student[3];
        students[0] = new Student("张一", 30);
        students[1] = new Student("张二", 10);
        students[2] = new Student("张三", 20);
        System.out.println(Arrays.toString(students));
        Arrays.sort(students, new Comparator<Student>() {
            @Override
            public int compare(Student o1, Student o2) {
                return o1.getAge()-o2.getAge();//按照年年龄升序
            }
        });
        System.out.println(Arrays.toString(students));
    }
}
```
## (7）Lambda表达式
在学习Lambda表达式之前 先了解内部类
### 内部类
内部类就是定义在一个类里面的类，里面的类可以理解成（寄生），外部类可以理解成（宿主）

**内部类的使用场景、作用：** 当一个事物的内部，还有一个部分需要一个完整的结构进行描述，而这个内部的完整的结构又只为外部事
物提供服务，那么整个内部的完整结构可以选择使用内部类来设计，同时内部类通常可以方便访问外部类的成员，包括私有的成员

### 匿名内部类
本质上是一个没有名字的局部内部类，定义在方法中、代码块中等。

**作用：**  方便创建子类对象，最终目的为了简化代码编写。

**格式：**
```java
new 类|抽象类名|或者接口名() {
	重写方法;
};
Animal a = new Animal() {
	public void run() {
		...
	}
};
a. run();
```

**特点:**

1. 匿名内部类是一个没有名字的内部类。
2. 匿名内部类写出来就会产生一个匿名内部类的对象。
3. 匿名内部类的对象类型相当于是当前new的那个的类型的子类类型
### Lambda概述
Lambda表达式是JDK 8开始后的一种新语法形式

**作用：** 简化匿名内部类的代码写法
**Lambda表达式的简化格式:**
```java
(匿名内部类被重写方法的形参列表) -> {
		被重写方法的方法体代码。
}
注：-> 是语法形式，无实际含义; Lambda表达式只能简化函数式接口的匿名内部类的写法形式
```

```java
public interface Dog {
    void eat();
}

public class Test {
    public static void main(String[] args) {
        Dog dog1 = new Dog(){
            @Override
            public void eat() {
                System.out.println("狗吃肉");
            }
        };
        dog1.eat();//狗吃肉
        //简化写法
        Dog dog2=()-> System.out.println("够吃骨头");
        dog2.eat();//够吃骨头
    }
}
```

**Lambda表达式使用前提:** 必须是接口的匿名内部类，接口中只能有一个抽象方法

**Lambda表达式的省略写法（进一步在Lambda表达式的基础上继续简化: **

1.参数类型可以省略不写。
2.如果只有一个参数，参数类型可以省略，同时()也可以省略。
3.如果Lambda表达式的方法体代码只有一行代码。可以省略大括号不写,同时要省略分号！
4.如果Lambda表达式的方法体代码只有一行代码。可以省略大括号不写。此时，如果这行代码是return语句，必须省略return不写，同时也必须省略";"不写

#  二、集合
## （1）单列集合:Collection（表示一组对象）
集合和数组都是容器。
**数组的特点：**
![请添加图片描述](https://i-blog.csdnimg.cn/direct/805f0c0d4df1480d836b7de7e45a1466.png)

数组定义完成并启动后，类型确定、长度固定。数组可以存储基本类型和引用类型的数据。
适合元素的个数和类型确定的业务场景，不适合做需要增删数据操作。
**集合的特点：**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/51cd7816c90d476baf12023bbc2782cd.png)

集合的大小不固定，启动后可以动态变化，类型也可以选择不固定。集合只能存储引用数据类型的数据。 …
集合适合做数据个数不确定，且要做增删元素的场景
###  Collection集合的特点
Collection单列集合，每个元素（数据）只包含一个值
Map双列集合，每个元素包含两个值（键值对）。

**通过一张图来了解Collection特点及体系**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8e8529b9ee8542d5b8690dc1bc25a6b7.png)
**集合对于泛型的支持:** 集合都是支持泛型的，可以在编译阶段约束集合只能操作某种数据类型

### Collection的常用API
Collection集合是单列集合的祖宗接口，它的功能是全部单列集合都可以继承使用的。

方法名称 |说明
-- | --
public boolean add(E e) |把给定的对象添加到当前集合中
public void clear() |清空集合中所有的元素
public boolean remove(E e)| 把给定的对象在当前集合中删除
public boolean contains(Object obj) |判断当前集合中是否包含给定的对象
public boolean isEmpty()| 判断当前集合是否为空
public int size() |返回集合中元素的个数。
public Object[] toArray()| 把集合中的元素，存储到数组中

**样例**：
```java
		//1.有序、可重复、有索引 List集合
        Collection list=new ArrayList();
        list.add("Java");
        list.add("Java");
        list.add(10);
        list.add(10);
        System.out.println(list);
        //2.无序、不重复、无索引 Set集合
        Collection list1=new HashSet();
        list1.add("Java");
        list1.add("Java");
        list1.add(10);
        list1.add(10);
        System.out.println(list1);
        //3.使用泛型
        Collection<String> list2=new ArrayList<>();
        list2.add("Java");
        list2.add("html");
        //list2.add(10); //报错
        System.out.println(list2);
        //4.集合和泛型不支持基本数据类型，只支持引用数据类型
        Collection<Integer> list3=new ArrayList<>();
        list3.add(100);
        list3.add(200);
        System.out.println(list3);

        //5.清空集合元素
        //list.clear();
        System.out.println(list);

        //6.判断集合是否为空
        System.out.println(list.isEmpty());
        //7.获取集合的大小
        System.out.println(list.size());
        //8.判断集合中是否包含某个元素
        System.out.println(list.contains("Java"));
        //9.删除某个元素
        System.out.println(list.remove("Java"));
        System.out.println(list);

        //10.把集合转换成数组
        Object[] objects = list.toArray();
        System.out.println(Arrays.toString(objects));
```
### 集合的遍历方式	
**方式一：迭代器:**
遍历就是一个一个的把容器中的元素访问一遍，迭代器在Java中的代表是Iterator，迭代器是集合的专用遍历方式。
Collection集合获取迭代器：
**Iterator<E> iterator()** ：返回集合中的迭代器对象，该迭代器对象默认指向当前集合的0索引
**Iterator中的常用方法：**
方法名称 |说明
--|--
boolean hasNext() |询问当前位置是否有元素存在，存在返回true ,不存在返回false
E next() |获取当前位置的元素，并同时将迭代器对象移向下一个位置，注意防止取出越界

```java
Iterator<String> it = list.iterator();
	while(it.hasNext()){
		String s = it.next();
		System.out.println(s);
	}
```

**方式二：foreach/增强for循环:**
**增强for循环格式：**
```java
//增强for循环：既可以遍历集合也可以遍历数组。
for(元素数据类型 变量 : 数组或者Collection集合){
	 
}
	
Collection<String> list = new ArrayList<>();
...
for(String s : list) {
	System.out.println(s);
}
```

**方式三：lambda表达式:**
简单、直接的遍历集合的方式。（推荐）
**default void forEach(Consumer<? super T> action):** 结合lambda遍历集合


```java
Collection<String> lists = new ArrayList<>();
...
lists.forEach(new Consumer<String>() {
	@Override
	public void accept(String s) {
		System.out.println(s);
	}
});
//结合lambda遍历集合
lists.forEach(s -> System.out.println(s);

```
一行代码搞定，代码更整洁清晰

### List系列集合
#### 1.List系列集合特点，特有API

**List系列集合特点：**
1. ArrayList、LinekdList ：有序，可重复，有索引。
2. 有序：存储和取出的元素顺序一致
3. 有索引：可以通过索引操作元素
4. 可重复：存储的元素可以重复

**List集合特有方法**
方法名称| 说明
--|--
void add(int index,E element) |在此集合中的指定位置插入指定的元素
E remove(int index) |删除指定索引处的元素，返回被删除的元素
E set(int index,E element)| 修改指定索引处的元素，返回被修改的元素
E get(int index) |返回指定索引处的元素

**List集合的遍历方式：**
1. 迭代器
2. 增强for循环
3. Lambda表达式
4. for循环（因为List集合存在索引）

#### 2.ArrayList集合
ArrayList底层是基于数组实现的：根据索引定位元素快，增删需要做元素的移位操作，第一次创建集合并添加第一个元素的时候，在底层创建一个默认长度为10的数组

底层是 **数组结构** 实现，查询快、增删慢

ArrayList 具备了List接口的特性（有序、重复、索引）。
ArrayList 集合底层的实现原理是**数组**，大小可变。
ArrayList 的特点：查询速度快、增删慢。
#### 3.LinkedList集合
底层数据结构是双链表，查询慢，首尾操作的速度是极快的，所以多了很多首尾操作的特有API

底层是**链表结构**实现，查询慢、增删快 

LinkedList 具备了 List 接口的特性。
LinkedList 底层实现原理是双向链表。
LinkedList 的增删速度快、查询慢

**LinkedList集合的特有功能：**
方法名称| 说明
--|--
public void addFirst (E e) |在该列表开头插入指定的元素
public void addLast (E e) |将指定的元素追加到此列表的末尾
public E getFirst () |返回此列表中的第一个元素
public E getLast ()| 返回此列表中的最后一个元素
public E removeFirst () |从此列表中删除并返回第一个元素
public E removeLast () |从此列表中删除并返回最后一个元素

---
### Set集合
**1.Set系列集合特点:**
1. 无序：存取顺序不一致
2. 不重复：可以去除重复
3. 无索引：没有带索引的方法，所以不能使用普通for循环遍历，也不能通过索引来获取元素。

**Set集合实现类特点:**
1. HashSet : 无序、不重复、无索引。
2. LinkedHashSet：有序、不重复、无索引。
3. TreeSet：排序、不重复、无索引。

Set集合的功能基本上与Collection的API一致


**2.TreeSet集合概述和特点:**
1. 不重复、无索引、可排序
2. 可排序：按照元素的大小默认升序（有小到大）排序。
3. 注意：TreeSet集合是一定要排序的，可以将元素按照指定的规则进行排序。

**TreeSet集合默认的规则:**
1. 对于数值类型：Integer , Double，官方默认按照大小进行升序排序。
2. 对于字符串类型：默认按照首字符的编号升序排序。
3. 对于自定义类型如对象，TreeSet无法直接排序。需要自己制定排序规则，让自定义的类实现Comparable接口重写里面的compareTo方法来定制比较规则。

---
### 泛型深入
**泛型概述**
1. 泛型：可以在编译阶段约束操作的数据类型，并进行检查
2. 泛型的格式：<数据类型>; 注意：泛型只能支持引用数据类型。
3. 集合体系的全部接口和实现类都是支持泛型的使用的。

**泛型的好处：** 统一数据类型，把运行时期的问题提前到了编译期间，避免了强制类型转换可能出现的异常，因为编译阶段类型就能确定下来。
**泛型可以在很多地方进行定义:** 类后面=>泛型类 ，方法申明上=>泛型方法  ，接口后面=>泛型接口

**1. 泛型类：**
定义类时同时定义了泛型的类就是泛型类。
泛型类的格式：
```java
修饰符 class 类名<泛型变量>{ }
public class MyArrayList<T> { }
```
此处泛型变量T可以随便写为任意标识，常见的如E、T、K、V等
编译阶段可以指定数据类型，类似于集合的作用

**2.泛型方法：**
定义方法时同时定义了泛型的方法就是泛型方法。
泛型方法的格式：
```java
修饰符 <泛型变量> 方法返回值 方法名称(形参列表){}
public <T> void show(T t) { }
```
方法中可以使用泛型接收一切实际类型的参数，方法更具备通用性。


**3.泛型接口**
使用了泛型定义的接口就是泛型接口。
泛型接口的格式：
```java
修饰符 interface 接口名称<泛型变量>{}
public interface Data<E>{}
```
泛型接口可以让实现类选择当前功能需要操作的数据类型


**4.泛型通配符、上下限**
**通配符:** **?**
**?** 可以在“使用泛型”的时候代表一切类型

**泛型的上下限：**
? extends Car: ?必须是Car或者其子类 泛型上限
? super Car ： ?必须是Car或者其父类 泛型下限
### 可变参数
可变参数用在形参中可以接收多个数据。
可变参数的格式：数据类型...参数名称

**可变参数的作用:** 传输参数非常灵活，方便。可以不传输参数，可以传输1个或者多个，也可以传输一个数组。可变参数在方法内部本质上就是一个数组。

**可变参数的注意事项：**
一个形参列表中可变参数只能有一个，可变参数必须放在形参列表的最后面

--- 

### 集合工具类Collections
java.utils.Collections:是集合工具类，Collections并不属于集合，是用来操作集合的工具类。

Collections常用的API
方法名称 |说明
--|--
public static <T> boolean addAll(Collection<? super T> c, T... elements) |给集合对象批量添加元素
public static void shuffle(List<?> list) |打乱List集合元素的顺序


**Collections排序相关API**
使用范围：只能对于List集合的排序

方法名称| 说明
--|--
public static <T> void sort(List<T> list) |将集合中元素按照默认规则排序
public static <T> void sort(List<T> list，Comparator<? super T> c)| 将集合中元素按照指定规则排序

样例
```java
		List<String> names=new ArrayList<>();
		//1.addAll方法一次添加多个元素
        Collections.addAll(names,"张一","张二","张三","张四");
        System.out.println(names);
        //2.shuffle方法：打乱集合排序
        Collections.shuffle(names);
        System.out.println(names);

        //3.sort方法:默认升序
        List<Integer>list=new ArrayList<>();
        Collections.addAll(list,10,30,20,40);
        System.out.println(list);
        
        Collections.sort(list);
        System.out.println(list);
```
---
## （2） 双列集合：Map(表示一组映射关系或键值对)	
### Map集合
#### 1.Map集合概述和使用
Map是双列集合的祖宗接口，它的功能是全部双列集合都可以继承使用的

1. Map集合是一种双列集合，每个元素包含两个数据
2. Map集合的每个元素的格式：key=value(键值对元素)。
3. Map集合也被称为“键值对集合”。

**Map集合整体格式：**
Collection集合的格式: [元素1,元素2,元素3..]
Map集合的完整格式：{key1=value1 , key2=value2 , key3=value3 , ...}
#### 2.Map集合体系
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/3791e55a45674174b866c61e0a4ca245.png)
**Map集合体系特点：**
1. Map集合的特点都是由键决定的。
2. Map集合的键是无序,不重复的，无索引的，值不做要求（可以重复）。
3. Map集合后面重复的键对应的值会覆盖前面重复键的值

**Map集合实现类特点：**
1. HashMap:元素按照键是无序，不重复，无索引，值不做要求。（与Map体系一致）
2. LinkedHashMap:元素按照键是有序，不重复，无索引，值不做要求。
3. TreeMap：元素按照建是排序，不重复，无索引的，值不做要求

#### 3.Map常用API
方法名称 |说明
-- | --
V put(K key,V value)| 添加元素
V remove(Object key)| 根据键删除键值对元素
void clear() |移除所有的键值对元素
boolean containsKey(Object key) |判断集合是否包含指定的键
boolean containsValue(Object value)|判断集合是否包含指定的值
boolean isEmpty() |判断集合是否为空
int size() |集合的长度，也就是集合中键值对的个数

**样例：**
```java
public class Test {
    public static void main(String[] args) {
        Map<String,Integer> maps=new HashMap<>();//无序、不重复、无索引
        maps.put("java",1);
        maps.put("html",2);
        maps.put("mysql",3);
        maps.put("c",4);
        System.out.println(maps);//{java=1, c=4, html=2, mysql=3}
        
        //移除所有的键值对元素
        //maps.clear();
        //System.out.println(maps);

        //删除某个元素
        maps.remove("java");
        System.out.println(maps);//{c=4, html=2, mysql=3}
        //通过键获取值
        System.out.println(maps.get("html"));//2
        //集合的大小
        System.out.println(maps.size());//3
        //判断集合是否包含指定的键
        System.out.println(maps.containsKey("mysql"));//true
        //判断集合是否包含指定的值
        System.out.println(maps.containsValue(3));//true
        //判断集合是否为空
        System.out.println(maps.isEmpty());//false
        //遍历集合
        maps.forEach((k,v)-> System.out.println(k+"="+v));
    }
}
```
#### 4.Map集合的遍历方式
**方式一：键找值的方式遍历：先获取Map集合全部的键，再根据遍历键找值**
 先获取Map集合的全部键的Set集合,遍历键的Set集合，然后通过键提取对应值。
 
 **键找值涉及到的API:**
 
 方法名称 |说明
-- |--
Set<K> keySet() |获取所有键的集合
V get(Object key) |根据键获取值

**方式二：键值对的方式遍历，把“键值对“看成一个整体，难度较大**
先把Map集合转换成Set集合，Set集合中每个元素都是键值对实体类型了，遍历Set集合，然后提取键以及提取值。

**键值对涉及到的API:**
 
 方法名称 |说明
-- |--
Set<Map.Entry<K,V>> entrySet() |获取所有键值对对象的集合
K getKey() |获得键
V getValue() |获取值


**方式三：Lambda表达式**
更简单、更直接的遍历集合的方式。
**Map结合Lambda遍历的API：**
方法名称 |说明
--|--
default void forEach(BiConsumer<? super K, ? super V> action) |结合lambda遍历Map集合
---
#### 5.Map的实现类
**Map 的实现类之一：HashMap**
1. HashMap 是 Map 接口使用频率较高的实现类。
2. 允许使用 null 键和 null 值，和 HashSet 一样，不能保证映射的顺序。
3. 一个 key - value 构成一个 entry 。
4. HashMap 判断两个 key 相等的标准：两个 key 的 hashCode() 和 equals() 分别都相等。
5. HashMap 是 数组+链表+红黑树（JDK7 及以前版本：HashMap 是 数组+链表结构。）

**HashMap的特点：** HashMap是Map里面的一个实现类。特点都是由键决定的：无序、不重复、无索引，没有额外需要学习的特有方法，直接使用Map里面的方法就可以了。

---
**Map 的实现类之二：LinkedHashMap**
1. LinkedHashMap 是 HashMap 的子类。
2. 在 HashMap 存储结构的基础上，使用了一对双向链表来记录添加元素的顺序。
3. 和 LinkedHashSet 类似，LinkedHashMap 可以维护 Map 的迭代顺序：迭代顺序和 key - value 对的插入顺序一致。

**LinkedHashMap的特点：**
由键决定：有序，不重复，无索引   这里的有序指的是保证存储和输出的元素顺序一致

---
**Map 的实现类之三：TreeMap**
1. TreeMap 存储 Key - Value 对时，需要根据 key - value 对进行排序。
2. TreeMap 可以保证所有的 key - value 对处于有序状态。
3. TreeMap 底层使用**红黑树结构**存储数据。
4. TreeMap 的 key 的排序：
    自然排序：TreeMap 的所有的 key 必须实现 Comparable 接口，而且所有的 key 应该是同一个类的对象，否则将会抛出 ClasssCastException 
     定制排序：创建 TreeMap 时，传入一个Comparator 对象，该对象负责对TreeMap中的所有key进行排序。此时不需要 Map 的 key 实现 Comparable 接口。
 5. TreeMap 判断两个 key 相等的标准：两个 key 通过 compareTo() 方法或者 compare() 方法返回 0 。


**TreeMap集合概述和特点：** 由键决定特性：不重复、无索引、可排序 ，可排序：按照键数据的大小默认升序（有小到大）排序。只能对键排序。
**注意：**TreeMap集合是一定要排序的，可以默认排序，也可以将键按照指定的规则进行排序，TreeMap跟TreeSet一样底层原理是一样的

**TreeMap集合自定义排序规则有2种：**
1. 类实现Comparable接口，重写比较规则。
2. 集合自定义Comparator比较器对象，重写比较规则。
---
**Map 的实现类之四：Hashtable**
1. Hashtable 是个古老的 Map 实现类，JDK1.0 就提供了。不同于 HashMap ，Hashtable 是线程安全的。
2. Hashtable 的实现原理和 HashMap 相同，功能相同。底层都是使用哈希表结构，查询速度快，很多情况下可以互用。
3. 和 HashMap 不同的是，**Hashtable 不允许使用 null 作为 key 和 value** 。
4. 和 HashMap 相同的是，Hashtable 不能保证 key - value 对的顺序。
5. Hashtable 判断两个 key 相等、两个 value 相等的标准，与 HashMap 一致。

---
**Map 的实现类之五：Properties**
1. Properties 类是 Hashtable 的子类，该对象是用来处理属性文件的。
2. 由于属性文件的 key 和 value 都是字符串类型，所以 Properties 里的 key 和 value 都是字符串类型。
3. 存取数据的时候，使用 **setProperty**和 **getProperty **方法。
## （3） 集合与数组的区别
区别名称|数组|集合
-|-|-
存储类型|数组是固定大小的，并且只能存储相同类型的元素（例如，int[] 只能存储整数）|集合是动态大小的，可以存储不同类型的对象（具体取决于集合的类型，例如 ArrayList<String> 只能存储字符串）。
大小调整|数组的大小在创建时确定，之后不能改变。如果需要更多空间，必须创建一个新的数组并将旧数组的元素复制到新数组中|集合的大小可以动态变化。例如，ArrayList 可以根据需要自动调整其大小。
存储对象|数组可以直接存储基本数据类型（如 int, char）和对象引用|集合只能存储对象引用（即使是基本数据类型的包装类，如 Integer，Character）。
访问速度|数组在内存中是连续存储的，因此访问速度非常快|集合的实现通常基于对象引用，访问速度可能稍慢，尤其是在复杂的集合操作（如查找和插入）时。
功能和方法|数组提供了有限的内置功能，主要是基本的遍历和访问操作。如果需要更多功能，通常需要使用循环和条件语句手动实现。|集合框架提供了丰富的内置方法，如添加、删除、查找、排序等。例如，List 接口提供了 add(), remove(), get() 等方法。
泛型支持|数组不支持泛型，这意味着你不能创建一个能够存储任意类型对象的数组（只能使用基本数据类型的数组或对象数组）|集合框架完全支持泛型，这允许你创建能够存储特定类型对象的集合（例如 ArrayList<String>）。
迭代|数组可以使用传统的 for 循环进行迭代，或者使用增强的 for 循环（Java 5 引入）|集合通常使用迭代器（Iterator）或增强的 for 循环进行迭代，还可以使用 Java 8 引入的流（Stream）进行高级迭代操作。
线程安全|数组不是线程安全的，多线程访问时需要手动同步|有些集合类（如 Vector 和 Hashtable）是线程安全的，但大多数（如 ArrayList 和 HashMap）不是。Java 提供了 Collections.synchronizedList() 等方法来创建线程安全的集合。

---
# 三、异常
### 异常概述、体系
异常是程序在“编译”或者“执行”的过程中可能出现的问题，注意：语法错误不算在异常体系中。
比如:数组索引越界、空指针异常、 日期格式化异常，等…
异常一但出现，如果没有提取处理，程序就会退出jvm虚拟机而终止
体现的是程序的安全，健壮

异常体系图
![请添加图片描述](https://i-blog.csdnimg.cn/direct/17981f210adf4144839fad4cc9a91989.png)

**Error：** 系统级别问题、JVM退出等，代码无法控制。
**Exception：** java.lang包下，称为异常类，它表示程序本身可以处理的问题
**RuntimeException及其子类：** 运行时异常，编译阶段不会报错。 (空指针异常，数组索引越界异常)
**除RuntimeException之外所有的异常：**编译时异常，编译期必须处理的，否则程序不能通过编译。 (日期格式化异常)。

---
### 异常的默认处理流程
1. 默认会在出现异常的代码那里自动的创建一个异常对象：ArithmeticException。
2. 异常会从方法中出现的点这里抛出给调用者，调用者最终抛出给JVM虚拟机。
3. 虚拟机接收到异常对象后，先在控制台直接输出异常栈信息数据。
4. 直接从当前执行的异常点干掉当前程序。
5. 后续代码没有机会执行了，因为程序已经死亡

默认的异常处理机制并不好，一旦出现异常，程序立即死亡！

---
### 运行时异常
**常见运行时异常：**
直接继承自RuntimeException或者其子类，编译阶段不会报错，运行时可能出现的错误。，一般是程序员业务没有考虑好或者是编程逻辑不严谨引起的程序错误

**运行时异常示例**
1. 数组索引越界异常: ArrayIndexOutOfBoundsException
2. 空指针异常 : NullPointerException，直接输出没有问题，但是调用空指针的变量的功能就会报错。
3. 类型转换异常：ClassCastException
4. 数学操作异常：ArithmeticException
5. 数字转换异常： NumberFormatException

**运行时异常的处理机制：**
运行时异常编译阶段不会出错，是运行时才可能出错的，所以编译阶段不处理也可以。按照规范建议还是处理：建议在最外层调用处集中捕获处理即可




---
### 编译时异常
**常见编译时异常**
不是RuntimeException或者其子类的异常，编译阶就报错，必须处理，否则代码不通过，在编译阶段就爆出一个错误, 目的在于提醒不要出错，编译时异常是可遇不可求，会提示处理异常


**编译时异常的处理机制：**
1. 出现异常直接抛出去给调用者
2. 出现异常自己捕获处理

**注意：实际开发中，只要代码能够编译通过，并且功能能完成，每一种异常处理方式都是可以的。**

**异常处理方式1 - throws**
throws：用在方法上，可以将方法内部出现的异常抛出去给本方法的调用者处理
**抛出异常格式：**
```java
方法 throws 异常1 ，异常2 ，异常3 ..{
}
```
**规范做法：**
```java
//代表可以抛出一切异常
方法 throws Exception{
}
```
**异常处理方式2 —— try…catch…**
用在方法内部，可以将方法内部出现的异常直接捕获处理， 发生异常的方法自己独立完成异常的处理，程序可以继续往下执行。
格式：
```java
try{
// 监视可能出现异常的代码！
}catch(异常类型1 变量){
// 处理异常
}catch(异常类型2 变量){
// 处理异常
}...
```
**建议格式：**
```java
//Exception可以捕获处理一切异常类型！
try{
// 可能出现异常的代码！
}catch (Exception e){
e.printStackTrace(); // 直接打印异常栈信息
}
```
### 自定义异常
**自定义异常的必要:** Java无法为这个世界上全部的问题提供异常类。如果企业想通过异常的方式来管理自己的某个业务问题，就需要自定义异常类了。

**自定义异常的好处：**
可以使用异常的机制管理业务问题，如提醒程序员注意。
同时一旦出现bug，可以用异常的形式清晰的指出出错的地方

**自定义异常的分类**

**1. 自定义编译时异常**
1. 定义一个异常类继承Exception. l 重写构造器。
2. 在出现异常的地方用throw new 自定义对象抛出，

**作用：** 编译时异常是编译阶段就报错，提醒更加强烈，一定需要处理！！

**2. 自定义运行时异常**
1. 定义一个异常类继承RuntimeException. l 重写构造器。
2. 在出现异常的地方用throw new 自定义对象抛出!

**作用：** 提醒不强烈，编译阶段不报错！！运行时才可能出现！！

**样例：**
```java
// 自定义运行时异常
public class AgeRuntimeException extends RuntimeException{
    public AgeRuntimeException() {
    }
    public AgeRuntimeException(String message) {
        super(message);
    }
}
//测试类
public class Test {
    public static void main(String[] args) {
        checkAge(300);
    }

    private static void checkAge(int age)  {
        if (age<0|| age>200){
            throw new AgeRuntimeException(age+"有误");
        }else{
            System.out.println("年龄通过");
        }
    }
}
```
**控制台打印结果：**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0f56cc274c964a1581545900efc36c55.png)

# 四、 IO
## （1）文件操作—File
### File类的使用
java.io.File 用于表示文件（目录），也就是说程序员可以通过 File 类在程序中操作硬盘上的文件和目录。
File 类只用于表示文件（目录）的信息（名称、大小等），换句话说只能访问文件或目录的相关属性，不能对文件的内容进行访问

**File类包含的静态方法有4个：**
方法名|说明
--|--
pathSeparator() |与系统有关的路径分隔符字符，出于方便考虑，它被表示为一个字符串。
pathSeparatorChar()| 与系统有关的默认路径分隔符字符。返回的是字符char
separator() |与系统有关的默认名称分隔符，出于方便考虑，它被表示为一个字符串。
separatorChar() |与系统有关的默认名称分隔符。返回字符char

 **构造方法：**
 方法名|说明
--|--
 public File(String pathname) ：|通过将给定的路径名字符串转换为抽象路径名来创建新的File实例。
 public File(String parent, String child) ：|从父路径名字符串和子路径名字符串创建新的 File实例。
public File(File parent, String child) ：|从父抽象路径名和子路径名字符串创建新的 File实例。

**小贴士：**
1. 一个File对象代表硬盘中实际存在的一个文件或者目录。
2. 无论该路径下是否存在文件或者目录，都不影响File对象的创建。
3. 因为各个操作系统的分隔符含义不同，所以要字符转义： \写成\
4. 绝对路径和相对路径：
	绝对路径：从盘符开始的路径，这是一个完整的路径。
	相对路径：相对于项目目录的路径，这是一个便捷的路径，开发中经常使用。
### File类常用方法
**获取功能的方法：**
 方法名|说明
--|--
public String getAbsolutePath()|返回此File的绝对路径名字符串。
public String getPath() |将此File 转换为路径 字符串。
public String getName() |返回由此File表示的文件或目录的名称。
public long length() |返回由此File表示的文件的长度。API中说明：length()，<br>表示文件的长度。但是File对象表示目录，则返回值未指定。



**判断功能的方法：**
方法名|说明
--|--
public boolean exists() |此File表示的文件或目录是否实际存在。
public boolean isDirectory() |此File表示的是否为目录。
public boolean isFile() |此File表示的是否为文件。

**创建删除功能的方法：**
方法名|说明
--|--
public boolean createNewFile() |当且仅当具有该名称的文件尚不存在时，创建一个新的空文件。
public boolean delete() |删除由此File表示的文件或目录。
public boolean mkdir() |创建由此File表示的目录。
public boolean mkdirs() |创建由此File表示的目录，包括任何必需但不存在的父目录。

**目录的遍历：**
方法名|说明
--|--
public String[] list() |返回一个String数组，表示该File目录中的所有子文件或目录。
public File[] listFiles() |返回一个File数组，表示该File目录中的所有的子文件或目录。
### FileFilter 接口
1. 通过 listFiles 方法我们可以获取一个目录下的所有子项，但有些时候我们并不希望获取全部子项，而是想获取部分满足我们实际需求的子项时，我们可以使用 File 的重载方法:
2. File[] listFiles(FileFilter filter)
3. 这里我们看到，该重载方法 要求我们传入一个参数，其类型是 FileFilter。什么是 FileFilter 呢?FileFilter 是用于抽象路径名的过滤器，说白了就是定义一个规则，那么结合 listFiles 方法，我们就可以将满足此过滤规则的子项返回，其他则忽略
4. FileFilter 是一个接口，所以当我们需要定义某种过滤规则时，我们可以定义一个类来实现这个接口，而此接口的实例可传递给 File 类的 listFiles(FileFilter) 方法
5. 该方法的参数FileFilter实例的accept方法并进行过滤，listFiles方法会将所有accept方法返回true的子项保留并返回。这个例子里我们会将 dir 中子项的名字以"."开头的返回

**File样例：**
```java
public class TestFile {
    //main方法和普通方法的默认路径是不一样的
    public static void main(String[] args) throws IOException {
        new File("demo").mkdir();
        File file = new File("demo" + File.separator + "HelloWorld.txt");
        file.createNewFile();
        System.out.println(file.getAbsoluteFile());
    }
    @Test
    public void testLength() {
        File file = new File("demo" + File.separator + "HelloWorld.txt");
        //System.out.println(file.getAbsoluteFile());
        System.out.println(file.length());
    }
    @Test
    public void testCreateNewFile() throws IOException {
        File file = new File("demo" + File.separator + "Hello.txt");
        if (!file.exists()) {
            file.createNewFile();
        }
    }
    @Test
    public void testDeleteFile() {
        File file = new File("demo" + File.separator + "Hello.txt");
        file.delete();
    }
    @Test
    public void testMkDir() {
        File file = new File("myDir");
        file.mkdir();
    }
    @Test
    public void testMkDirs() {
        File file = new File("a" + File.separator + "b" + File.separator + "c");
        file.mkdirs();
    }
    @Test
    public void testDeleteDir() {
        File file = new File("demo");
        System.out.println(file.delete());
    }
    @Test
    public void testListFiles() {
        File file = new File(".");
        //System.out.println(file.getAbsoluteFile());
        File[] files = file.listFiles();
        for (File f : files) {
            System.out.println(f);
        }
    }
    @Test
    public void testFileFilter() {
        File file = new File(".");
        System.out.println(file.getAbsoluteFile());
        File[] files = file.listFiles(pathname -> pathname.getName().endsWith("txt"));
        for (File f : files) {
            System.out.println(f);
        }
    }
}
```
## （2）基本 IO 操作(字节流)
### InputStream 与 OutputStream
**输入与输出:**
输入是一个从外界进入到程序的方向，通常我们需要“读取”外界的数据时，使用输入。所以输入是用来读取数据的。
输出是一个从程序发送到外界的方向，通常我们需要”写出”数据到外界时，使用输出。所以输出是用来写出数据的。
**节点流与处理流:**
按照流是否直接与特定的地方 (如磁盘、内存、设备等) 相连，分为节点流和处理流两类。
节点流：可以从或向一个特定的地方（节点）读写数据。
处理流：是对一个已存在的流的连接和封装，通过所封装的流的功能调用实现数据读写。
处理流的构造方法总是要带一个其他的流对象做参数。一个流对象经过其他流的多次包装，称为流的链接。
**InputStream 与 OutputStream 常用方法:**
方法名|说明
--|--
int read()| 读取一个字节，以 int 形式返回，该 int 值的”低八位”有效，若返回值为-1 则表示 EOF。
int read(byte[] d)|尝试最多读取给定数组的 length 个字节并存入该数组，返回值为实际读取到的字节量
void write(int d)|写出一个字节,写的是给定的 int 的”低八位
void write(byte[] d)| 将给定的字节数组中的所有字节全部写出
### 文件流
**创建 FOS 对象(重写模式)**
FileOutputStream 是文件的字节输出流，我们使用该流可以以字节为单位将数据写入文件。
**构造方法:**
方法名|说明
--|--
FileOutputStream(File file)|创建一个向指定 File 对象表示的文件中写入数据的文件输出流。
FileOutputStream(String filename)| 创建一个向具有指定名称的文件中写入数据的输出文 件流。

里需要注意，若指定的文件已经包含内容，那么当使用 FOS 对其写入数据时，会将该文件中原有数据全部清除

**创建 FOS 对象(追加模式)**
在文件的原有数据之后追加新数据则需要以下构造方法创建 FOS
**构造方法:**
方法名|说明
--|--
FileOutputStream(File file,boolean append)|创建一个向指定 File 对象表示的文件中写入数据的文件输出流
FileOutputStream(String filename,boolean append)|创建一个向具有指定名称的文件中写入数据的输出文 件流。

以上两个构造方法中，第二个参数若为 true,那么通过该 FOS 写出的数据都是在文件末尾追加的。

**创建 FIS 对象**
FileInputStream 是文件的字节输入流，我们使用该流可以以字节为单位读取文件内容。
**构造方法:**
方法名|说明
--|--
FileInputStream(File file):| 创建用于读取给定的 File 对象所表示的文件 FIS
FileInputStream(String name)| 创建用于读取给定的文件系统中的路径名 name 所指定的文件的 FIS

**read()和 write(int d)方法**
方法名|说明
----|--
int read()|从此输入流中读取一个数据字节,若返回-1 则表示 EOF(End Of File)<br>FileOutputStream 继承自 OutputStream,其提供了以字节为单位向文件写数据的方法 write。
void write(int d)| 将指定字节写入此文件输出流

**read(byte[] d)和 write(byte[] d)方法**
方法名|说明
----|--
int read(byte[] b)| 从此输入流中将最多 b.length 个字节的数据读入一个 byte 数组中 。<br>FileInputStream 也支持批量读取字节数据的方法:
void write(byte[] d)| 将 b.length 个字节从指定 byte 数组写入此文件输出流中<br>FileOutputStream 也支持批量写出字节数据的方法
void write(byte[] d,int offset,int len)|将指定 byte 数组中从偏移量 off 开始的 len 个字节写入此文件输出流
---
### 缓冲流
. **BOS 基本工作原理:**
与缓冲输入流相似，在向硬件设备做写出操作时，增大写出次数无疑也会降低写出效率，为此我们可以使用缓冲输出流来一次性批量写出若干数据减少写出次数来提高写出效率
BufferedOutputStream 缓冲输出流内部也维护着一个缓冲区，每当我们向该流写数据时，都会先将数据存入缓冲区，当缓冲区已满时，缓冲流会将数据一次性全部写出。
**BOS 实现写出缓冲:**
```java
public void testBOS() throws Exception {
	 FileOutputStream fos = new FileOutputStream("demo.dat");
 	//创建缓冲字节输出流
	 BufferedOutputStream bos= new BufferedOutputStream(fos);
	//所有字节被存入缓冲区，等待一次性写出
	 bos.write("helloworld".getBytes());
	//关闭流之前，缓冲输出流会将缓冲区内容一次性写出
	 bos.close();
}
```
**BOS 的 flush 方法:**
使用缓冲输出流可以提高写出效率，但是这也存在着一个问题，就是写出数据缺乏即时性。有时我们需要在执行完某些写出操作后，就希望将这些数据确实写出，而非在缓冲区中保存直到缓冲区满后才写出。这时我们可以使用缓冲流的一个方法 flush
方法名|说明
----|--
void flush()|清空缓冲区，将缓冲区中的数据强制写出

**BIS 基本工作原理:**
在读取数据时若以字节为单位读取数据，会导致读取次数过于频繁从而大大的降低读取效率。为此我们可以通过提高一次读取的字节数量减少读写次数来提高读取的效率

BufferedInputStream 是缓冲字节输入流。其内部维护着一个缓冲区(字节数组)，使用该流在读取一个字节时，该流会尽可能多的一次性读取若干字节并存入缓冲区，然后逐一的将字节返回，直到缓冲区中的数据被全部读取完毕，会再次读取若干字节从而反复。这样就减少了读取的次数，从而提高了读取效率

BIS 是一个处理流，该流为我们提供了缓冲功能。

**BIS 实现输入缓冲:**
使用缓冲流来实现文件复制:
```java
	FileInputStream fis = new FileInputStream("java.zip");
	BufferedInputStream bis = new BufferedInputStream(fis);
	FileOutputStream fos = new FileOutputStream("copy_java.zip");
	BufferedOutputStream bos = new BufferedOutputStream(fos);
	int d = -1;
	while((d = bis.read())!=-1){
		bos.write(d);
 	}
	bis.close();//读写完毕后要关闭流，只需要关闭最外层的流即可
	bos.close();
```
---
### 对象流（较特殊）
**对象序列化概念:** 对象是存在于内存中的。有时候我们需要将对象保存到硬盘上，又有时我们需要将对象传输到另一台计算机上等等这样的操作。这时我们需要将对象转换为一个字节序列，而这个过程就称为对象序列化。相反，我们有这样一个字节序列需要将其转换为对应的对象，这个过程就称为对象的反序列化。

序列化：把对象转换为字节序列的过程称为对象的序列化。
反序列化：把字节序列恢复为对象的过程称为对象的反序列化。

**使用 OOS 实现对象序列化**
 ObjectOutputStream 是用来对对象进行序列化的输出流
 其实现对象序列化的方法为:
 方法名|说明
--|--
  void writeObject(Object o)|该方法可以将给定的对象转换为一个字节序列后写出。
 
```java
	//序列化
	@Test
	public  void testOOs() throws Exception {
		FileOutputStream fos=new FileOutputStream("emp.obj");
		ObjectOutputStream oos=new ObjectOutputStream(fos);
		Emp emp=new Emp("张三",23,"男",10000);
		oos.writeObject(emp);
		System.out.println("序列化完成！");
		oos.close();
	}
```
  
**使用 OIS 实现对象反序列化：**
ObjectInputStream 是用来对对象进行反序列化的输入流
其实现对象反序列化的方法为:
 方法名|说明
--|--
Object readObject()|该方法可以从流中读取字节并转换为对应的对象。

```java
	//反序列化
	@Test
    public void testOis() throws Exception {
        FileInputStream fis=new FileInputStream("emp.obj");
        ObjectInputStream ois=new ObjectInputStream(fis);
        Emp emp = (Emp) ois.readObject();
        System.out.println(emp);
    }
```

**Serializable 接口：**

1. ObjectOutputStream 在对对象进行序列化时有一个要求，就是需要序列化的对象所属的类必须实现 Serializable 接口。
2. 实现该接口不需要重写任何方法。其只是作为可序列化的标志
3. 通常实现该接口的类需要提供一个常量 serialVersionUID，表明该类的版本。若不显示的声明，在对象序列化时也会根据当前类的各个方面计算该类的默认 serialVersionUID，但不同平台编译器实现有所不同，所以若向跨平台，都应显示的声明版本号。
4. 如果声明的类序列化存到硬盘上面，之后随着需求的变化更改了类别的属性(增加或减少或改名)，那么当反序列化时，就会出现 InvalidClassException，这样就会造成不兼容性的问题
5. 但当 serialVersionUID 相同时，它就会将不一样的 field 以 type 的预设值反序列化，可避开不兼容性
问题。


**transient 关键字：**
1.  对象在序列化后得到的字节序列往往比较大，有时我们在对一个对象进行序列化时可以忽略某些不必要的属性，从而对序列化后得到的字节序列”瘦身”
2. 被该关键字修饰的属性在序列化时其值将被忽略

## （3）文件数据 IO 操作（字符流）
### Reader 和 Writer
Reader 是所有字符输入流的父类，而 Writer 是所有字符输出流的父类。字符流是以字符(char)为单位读写数据的。一次处理一个 unicode。字符流都是高级流，其底层都是依靠字节流进行读写数据的，所以底层仍然是基于字节读写数据的

**常用方法**
方法名|说明
--|--
int read()|读取一个字符。
int read(char[] chs)|从该流中读取一个字符数组 length 个字符并存入该数组，<br>返回值为实际读取到的字符量。 Writer 的常用方法
void write(int c)|写出一个字符,写出给定 int 值表示的字符。
void write(char[] chs)|将给定字符数组中所有字符写出
void write(String str)|将给定的字符串写出
void write(char[] chs,int offset,int len)|将给定的字符数组中从 offset 处开始连续的 len 个字符写出
### 转换流
**字符转换流原理：**
InputStreamReader：字符输入流， 使用该流可以设置字符集，并按照指定的字符集从流中按照该编码将字节数据转换为字符并读取。
OutputStreamWriter：字符输出流，使用该流可以设置字符集，并按照指定的字符集将字符转换为对应字节后通过该流写出。

**指定字符编码：**
InputStreamReader 的构造方法允许我们设置字符集:
方法名|说明
--|--
 InputStreamReader(InputStream in,String charsetName)|基于给定的字节输入流以及字符编码创建 ISR
 InputStreamReader(InputStream in)|该构造方法会根据系统默认字符集创建 ISR


OutputStreamWriter:的构造方法：
方法名|说明
--|--
 OutputStreamWriter(OutputStream out,String charsetName)|基于给定的字节输出流以及字符编码创建 OSW
 OutputStreamWriter(OutputStream out)| 该构造方法会根据系统默认字符集创建 OSW

**使用 OutputStreamWriter：**
```java
public void testOutput() throws IOException{
	FileOutputStream fos= new FileOutputStream("demo.txt");
	OutputStreamWriter writer= new OutputStreamWriter(fos,"UTF-8");
	String str = "大家好!";//UTF-8 中文为 3 个字节，英文符号占 1 个字节
	writer.write(str);//写出后该文件大小应该为 10 字节
	writer.close();
}
```

**使用 InputStreamReader：**
```java
 public void testInput() throws IOException{
	 FileInputStream fis = new FileInputStream("demo.txt");
 	/*
 	* 这里设置了字符编码为 GBK
 	* 之后再通过 ISR 读取 demo.txt 文件时
 	* 就使用 GBK 编码读取字符了
 	*/
 	InputStreamReader reader= new InputStreamReader(fis,"GBK");
 	int c = -1;
 	while((c = reader.read()) != -1){
 		System.out.print((char)c);
	}
	reader.close();
}
```
### PrintWriter
**创建 PrintWriter 对象:**
PrintWriter 是具有自动行刷新的缓冲该字符输出流。其提供了比较丰富的构造方法:
1. PrintWriter(File file)
2. PrintWriter(String fileName)
3. PrintWriter(OutputStream out)
4. PrintWriter(OutputStream out,boolean autoFlush)
5. PrintWriter(Writer writer)
6. PrintWriter(Writer writer,boolean autoFlush)

其中参数为 OutputStream 与 Writer 的构造方法提供了一个可以传入 boolean 值参数，该参数用于表示 PrintWriter 是否具有自动行刷新。

**PrintWriter 的重载 print 和 println 方法:**
使用 PrintWriter 写出字符串时我们通常不使用 Writer 提供的 write()相关方法，而是使用 print 和println 等方法，PrintWriter 提供了若干重载的 print 与 println 方法，其中 println 方法是在写出数据后自动追加一个系统支持的换行符。
**重载方法有**
方法名|说明
--|--
 void print(int i)|打印整数
void print(char c)|打印字符
 void print(boolean b)|打印 boolean 值
 void print(char[] c)|打印字符数组
void print(double d)|打印 double 值
 void print(float f)|打印 float 值
 void print(long l)|打印 long 值
 void print(String str)|打印字符串
 
注:这些方法还有类似的 println 方法，参数与上面相同。

**使用 PrintWriter 输出字符数据:**
```java
	FileOutputStream fos = new FileOutputStream("demo.txt");
	OutputStreamWriter osw = new OutputStreamWriter(fos,"UTF-8");
	//创建带有自动行刷新的 PW
	PrintWriter pw = new PrintWriter(osw,true);
	pw.println("大家好!");
	pw.close();
```
### BufferedReader
**构建 BufferedReader 对象:**
方法名|说明
--|--
 BufferedReader(Reader reader)|BufferedReader 是缓冲字符输入流，其内部提供了缓冲区，可以提高读取效率
 
 **例如:**
```java
	FileInputStream fis = new FileInputStream("demo.txt");
	InputStreamReader isr = new InputStreamReader(fis);
	BufferedReader br = new BufferedReader(isr);
```


**使用 BR 读取字符串:**
BufferedReader 提供了一个可以便于读取一行字符串
方法名|说明
--|--
String readLine() | 该方法连续读取一行字符串，直到读取到换行符为止，<br>返回的字符串中不包含该换行符。若 EOF 则该方法返回 null。
 
 **例如:**

```java
	FileInputStream fis = new FileInputStream("demo.txt");
	InputStreamReader isr = new InputStreamReader(fis);
	BufferedReader br = new BufferedReader(isr);
	String str = null;
	while((str = br.readLine()) != null){
		 System.out.println(str);
	}
	br.close();
```

# 五、线程
线程(thread)是一个程序内部的一条执行路径，我们之前启动程序执行后，main方法的执行其实就是一条单独的执行路径，程序中如果只有一条执行路径，那么这个程序就是单线程的程序。
多线程是指从软硬件上实现多条执行流程的技术，消息通信、淘宝、京东系统都离不开多线程技术。
## 多线程的创建
**多线程的实现方案一：继承Thread类（ Java是通过java.lang.Thread 类来代表线程的）**
1. 定义一个子类MyThread继承线程类java.lang.Thread，重写run()方法
2.  创建MyThread类的对象
3.  调用线程对象的start()方法启动线程（启动后还是执行run方法的）
4. 方式一优缺点
优点：编码简单
缺点：线程类已经继承Thread，无法继承其他类，不利于扩展。

**样例：**
```java
public class ThreadTest {

    public static void main(String[] args) {
        //3.创建一个新线程对象
        Thread thread=new MyThread();
        //4.调用start方法,启动新的线程
        thread.start();
        //thread.run();
        for (int i = 0; i <5 ; i++) {
            System.out.println("主线程"+i);
        }
    }
}
//1.创建一个线程类
class MyThread extends Thread{
    //2.重写run方法
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("子线程"+i);
        }
    }
}
```

**多线程的实现方案二：实现Runnable接口**
1. 定义一个线程任务类MyRunnable实现Runnable接口，重写run()方法
2. 创建MyRunnable任务对象
3. 把MyRunnable任务对象交给Thread处理。
4. 调用线程对象的start()方法启动线程
5. 方式二优缺点：
优点：线程任务类只是实现接口，可以继续继承类和实现接口，扩展性强。
缺点：编程多一层对象包装，如果线程有执行结果是不可以直接返回的。
   
 **Thread的构造器**
 方法名|说明
--|--
 public Thread(String name) |可以为当前线程指定名称
 public Thread(Runnable target) |封装Runnable对象成为线程对象
 public Thread(Runnable target ，String name )| 封装Runnable对象成为线程对象，并指定线程名称
 
 **实现Runnable接口(匿名内部类形式)**
1. 可以创建Runnable的匿名内部类对象。
2. 交给Thread处理
3. 调用线程对象的start()启动线程

```java
public class RunnableTest{
    public static void main(String[] args) {
        //3.创建一个线程任务对象
        Runnable runnable=new MyRunnable();
        //4.把任务对象交给Thread
        Thread thread=new Thread(runnable);
        //5.启动线程
        thread.start();
        for (int i = 0; i < 5; i++) {
            System.out.println("主线程"+i);
        }
    }
}
//1.创建一个线程类
class MyRunnable implements Runnable{
    //2.重写run方法
    @Override
    public void run() {
        for (int i = 0; i <5 ; i++) {
            System.out.println("子线程"+i);
        }
    }
}
```

**多线程的实现方案三：利用Callable、FutureTask接口实现**
1. 得到任务对象
定义类实现Callable接口，重写call方法，封装要做的事情
用FutureTask把Callable对象封装成线程任务对象
2. 把线程任务对象交给Thread处理
3. 调用Thread的start方法启动线程，执行任务
4. 线程执行完毕后、通过FutureTask的get方法去获取任务执行的结果
5.  方式三优缺点：
  优点：线程任务类只是实现接口，可以继续继承类和实现接口，扩展性强。可以在线程执行完毕后去获取线程执行的结果。
  缺点：编码复杂一点。

FutureTask的API
方法名|说明
--|--
 public V get() throws Exception |获取线程执行call方法返回的结果
 public FutureTask<>(Callable call) |把Callable对象封装成FutureTask对象。

```java
public class CallableTest{
    public static void main(String[] args) {
        //3.创建一个Callable对象
        Callable<String> callable=new MyCallable(100);
        //4.把Callable对象交给FutrueTask
        FutureTask<String> futureTask=new FutureTask<>(callable);
        //5.交给Thread
        Thread thread=new Thread(futureTask);
        //6.启动线程
        thread.start();
        try {
            String s = futureTask.get();
            System.out.println("结果"+s);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
//1.创建了一个线程任务类
class MyCallable implements Callable<String>{
    private int n;
    public MyCallable(int n) {
        this.n = n;
    }
    //2.重写call方法
    @Override
    public String call() throws Exception {
        int sum=0;
        for (int i = 1; i <= n; i++) {
            sum+=i;
        }
        return "子线程计算的结果："+sum;
    }
}
```
 **3种方式对比**
方式|优点|缺点
--|--|--
继承Thread类|编程比较简单，可以直接使用Thread类中的方法| 扩展性较差，不能再继承其他的类，不能返回线程执行的结果
实现Runnable接口|扩展性强，实现该接口的同时还可以继承其他的类|编程相对复杂，不能返回线程执行的结果
实现Callable接口|扩展性强，实现该接口的同时还可以继承其他的类|可以得到线程执行的结果编程相对复杂

## Thread的常用方法
**Thread常用API说明**
 Thread常用方法：获取线程名称getName()、设置名称setName()、获取当前线程对象currentThread()。 至于Thread类提供的诸如：yield、join、interrupt、不推荐的方法 stop 、守护线程、线程优先级等线程的控制方法，在开发中很少使用，了解即可。

**Thread常用方法：**
方法名|说明
--|--
 String getName ()| 获取当前线程的名称，默认线程名称是Thread-索引
 void setName (String name)| 设置线程名称
 public static Thread currentThread()| 返回对当前正在执行的线程对象的引用
 public static void sleep(long time)| 让线程休眠指定的时间，单位为毫秒
 public void run() |线程任务方法
 public void start() |线程启动方法

**Thread常用构造器**
方法名|说明
--|--
 public Thread(String name) |可以为当前线程指定名称
 public Thread(Runnable target)| 把Runnable对象交给线程对象
 public Thread(Runnable target ，String name ) |把Runnable对象交给线程对象，并指定线程名称
## 线程安全
多个线程同时操作同一个共享资源的时候可能会出现业务安全问题，称为线程安全问题，线程安全问题出现的原因：
1. 存在多线程并发
2. 同时访问共享资源
3. 存在修改共享资源

**线程安全问题发生的原因是什么：** 多个线程同时访问同一个共享资源且存在修改该资源

**例子：** 小明和小红是一对夫妻，他们有一个共同的账户，余额是10万元。如果小明和小红同时来取钱，而且2人都要取钱10万元
假设银行取钱需要三步
1.判断余额是否足够
2.吐出100000元
3.更新账户余额
小明进入程序进行完成第二步取出100000元 还未更新账户余额 ，此时小红进行余额判断 由于账户未更新 第一步判断余额还剩100000元，则小红也会取出100000元
结果：2人都取钱10万，银行亏了10万。
## 线程同步
**线程同步的核心思想：** 加锁，把共享资源进行上锁，每次只能一个线程进入访问完毕以后解锁，然后其他线程才能进来

**方式一：同步代码块**
**作用：** 把出现线程安全问题的核心代码给上锁
**原理：** 每次只能一个线程进入，执行完毕后自动解锁，其他线程才可以进来执行
```java
 synchronized(同步锁对象) {
	操作共享资源的代码(核心代码)
}
```
同步代码块是如何实现线程安全的，对出现问题的核心代码使用synchronized进行加锁，每次只能一个线程占锁进入访问
同步代码块的同步锁对象要求：
  1. 对于实例方法建议使用this作为锁对象。
  2. 对于静态方法建议使用字节码（类名.class）对象作为锁对象。

锁对象的规范要求规范上：建议使用共享资源作为锁对象。

锁对象用任意唯一的对象不好，会影响其他无关线程的执行

**方式二：同步方法**
 **作用：**把出现线程安全问题的核心方法给上锁
 **原理：**每次只能一个线程进入，执行完毕以后自动解锁，其他线程才可以进来执行
 
 ```java
 修饰符 synchronized 返回值类型 方法名称(形参列表) {
	操作共享资源的代码
}
```
同步代码块锁的范围更小，同步方法锁的范围更大。
 
 同步方法是如何保证线程安全的
1. 对出现问题的核心方法使用synchronized修饰
 2. 每次只能一个线程占锁进入访问
 
同步方法的同步锁对象的原理
 1. 对于实例方法默认使用this作为锁对象。
 2. 对于静态方法默认使用类名.class对象作为锁对象。

**方式三：Lock锁**
**Lock锁概念**
1. 为了更清晰的表达如何加锁和释放锁，JDK5以后提供了一个新的锁对象Lock，更加灵活、方便
 2. Lock实现提供比使用synchronized方法和语句可以获得更广泛的锁定操作
 3. Lock是接口不能直接实例化，这里采用它的实现类ReentrantLock来构建Lock锁对象

方法名|说明
--|--
  public ReentrantLock() |获得Lock锁的实现类对象
  void lock()| 获得锁
  void unlock() |释放锁

## 线程通信(了解)
**线程通信的三个常见方法**
方法名称 |说明
--|--
void wait () |当前线程等待，直到另一个线程调用notify() 或 notifyAll()唤醒自己
void notify () |唤醒正在等待对象监视器(锁对象)的单个线程
void notifyAll ()|唤醒正在等待对象监视器(锁对象)的所有线程

**注意：** 上述方法应该使用当前同步锁对象进行调用
## 线程池（重点）
### 1.线程池概念
 **概念**线程池就是一个可以复用线程的技术。
**不使用线程池的问题：** 如果用户每发起一个请求，后台就创建一个新线程来处理，下次新任务来了又要创建新线程，而创建新线程的开销是很大的，这样会严重影响系统的性能。
**线程池的工作原理**
线程池
 1. 工作线程WorkThread
 2. 任务队列（WorkQueue）

任务接口
 1. Runnable
 2. Callable

**线程池工作原理图：**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f375c89f3cfa49ad813f7e775f3a4192.png)
### 2. 线程池实现的API、参数说明
JDK 5.0起提供了代表线程池的接口：ExecutorService
**如何得到线程池对象：**
**方式一：使用ExecutorService的实现类ThreadPoolExecutor自创建一个线程池对象**
ExecutorService==>ThreadPoolExecutor

**方式二：使用Executors（线程池的工具类）调用方法返回不同特点的线程池对象**
ThreadPoolExecutor构造器的参数说明:
```java
public ThreadPoolExecutor(int corePoolSize,
						  int maximumPoolSize,
						  long keepAliveTime,
						  TimeUnit unit,
						  BlockingQueue<Runnable> workQueue,
						  ThreadFactory threadFactory,
						  RejectedExecutionHandler handler)
```
参数名称|说明|要求
--|--|--
corePoolSize|指定线程池的线程数量（核心线程）|不能小于0
maximumPoolSize|指定线程池可支持的最大线程数|最大数量 >= 核心线程数量
keepAliveTime|指定临时线程的最大存活时间|不能小于0
unit|指定存活时间的单位(秒、分、时、天)|时间单位
workQueue|指定任务队列|不能为null
threadFactory|指定用哪个线程工厂创建线程|不能为null
handler|指定线程忙，任务满的时候，新任务来了怎么办|不能为null

**ThreadPoolExecutor创建线程池对象示例**
```java
ExecutorService pools = new ThreadPoolExecutor(3, 5
						, 8 , TimeUnit.SECONDS, new ArrayBlockingQueue<>(6),
						Executors.defaultThreadFactory() , new ThreadPoolExecutor.AbortPolicy());
```


**临时线程创建时间**
新任务提交时发现核心线程都在忙，任务队列也满了，并且还可以创建临时线程，此时才会创建临时线程

**什么时候会开始拒绝任务**
核心线程和临时线程都在忙，任务队列也满了，新的任务过来的时候才会开始任务拒绝。
### 3.线程池处理Runnable任务 或Callable任务
使用ExecutorService的方法
1. void execute(Runnable target)b处理Runnable任务
2. Future<T> submit(Callable<T> command) 处理Callable任务，并得到任务执行完后返回的结果。

**ExecutorService的常用方法**
方法名称 |说明
--|--
void execute(Runnable command) |执行任务/命令，没有返回值，一般用来执行 Runnable 任务
Future<T> submit(Callable<T> task)| 执行任务，返回未来任务对象获取线程结果，一般拿来执行 Callable 任务
void shutdown() |等任务执行完毕后关闭线程池
List<Runnable> shutdownNow() |立刻关闭，停止正在执行的任务，并返回队列中未执行的任务

新任务拒绝策略
策略| 详解
-|-
ThreadPoolExecutor.AbortPolicy| 丢弃任务并抛出RejectedExecutionException异常。是默认的策略
ThreadPoolExecutor.DiscardPolicy| 丢弃任务，但是不抛出异常 这是不推荐的做法
ThreadPoolExecutor.DiscardOldestPolicy |抛弃队列中等待最久的任务 然后把当前任务加入队列中
ThreadPoolExecutor.CallerRunsPolicy |由主线程负责调用任务的run()方法从而绕过线程池直接执行

### 4.Executors工具类实现线程池
**Executors：** 线程池的工具类通过调用方法返回不同类型的线程池对象。
**Executors得到线程池对象的常用方法**

方法名称 |说明
-|-
public static ExecutorService newCachedThreadPool()| 线程数量随着任务增加而增加，如果线程任务执行完毕且空闲了一段时间则会被回收掉。
public static ExecutorService newFixedThreadPool (int nThreads)| 创建固定线程数量的线程池，如果某个线程因为执行异常而结束，<br>那么线程池会补充一个新线程替代它。
public static ExecutorService newSingleThreadExecutor ()| 创建只有一个线程的线程池对象，如果该线程出现异常而结束，<br>那么线程池会补充一个新线程。
public static ScheduledExecutorService newScheduledThreadPool (int corePoolSize) |创建一个线程池，可以实现在给定的延迟后运行任务，<br>或者定期执行任务。

**注意：** Executors的底层其实也是基于线程池的实现类ThreadPoolExecutor创建线程池对象的。

**Executors使用可能存在的陷阱** 
大型并发系统环境中使用Executors如果不注意可能会出现系统风险

方法名称 |存在问题
-|-
public static ExecutorService newFixedThreadPool (int nThreads);<br>public static ExecutorService newSingleThreadExecutor() |允许请求的任务队列长度是Integer.MAX_VALUE，可能出现OOM错误（ java.lang.OutOfMemoryError ）
public static ExecutorService newCachedThreadPool();<br> public static ScheduledExecutorService newScheduledThreadPool (int corePoolSize)|创建的线程数量最大上限是Integer.MAX_VALUE，线程数可能会随着任务1:1增长，也可能出现OOM错误（ java.lang.OutOfMemoryError ）


线程池ExecutorService的实现类：ThreadPoolExecutor
建议使用ThreadPoolExecutor来指定线程池参数，这样可以明确线程池的运行规则，规避资源耗尽的风险。


### 5.线程池样例
先利用Runnable创建一个线程
```java
public class MyRunnable implements Runnable{
    @Override
    public void run() {
        for (int i = 0; i <5 ; i++) {
            System.out.println(Thread.currentThread().getName()+"输出"+i);
        }
    }
}
```
先利用Callable创建一个线程
```java
public class MyCallable implements Callable<String> {
    private  int n;

    public MyCallable() {
    }

    public MyCallable(int n) {
        this.n = n;
    }

    @Override
    public String call() throws Exception {
        int sum=0;
        for (int i = 1; i <=n ; i++) {
            sum+=i;
        }
        return Thread.currentThread().getName()+"执行1~"+n+"的和"+sum;
    }
}
```
创建线程池进行测试
```java
public class ThreadPoolTest1 {
    //创建一个线程池对象
    public static void main(String[] args) {
        ExecutorService pool = new ThreadPoolExecutor(3, 5, 6, TimeUnit.SECONDS,
                new ArrayBlockingQueue<>(5), new ThreadPoolExecutor.AbortPolicy());
        //给任务线程池处理
        Runnable target = new MyRunnable();
        pool.execute(target);
        pool.execute(target);
        pool.execute(target);
        pool.execute(target);
        pool.execute(target);
        pool.execute(target);
        pool.execute(target);
        pool.execute(target);

        //创建临时线程
        pool.execute(target);
        pool.execute(target);
        //不创建，拒绝策略触发
        pool.execute(target);

    }
}

public class ThreadPoolTest2 {
    public static void main(String[] args) throws Exception {
        ExecutorService pool = new ThreadPoolExecutor(3, 5, 6, TimeUnit.SECONDS,
                new ArrayBlockingQueue<>(5), new ThreadPoolExecutor.AbortPolicy());
        //给任务线程池处理
        Future<String> f1 = pool.submit(new MyCallable(100));
        Future<String> f2 = pool.submit(new MyCallable(200));
        Future<String> f3 = pool.submit(new MyCallable(300));
        Future<String> f4 = pool.submit(new MyCallable(400));
        Future<String> f5 = pool.submit(new MyCallable(500));
        System.out.println(f1.get());
        System.out.println(f2.get());
        System.out.println(f3.get());
        System.out.println(f4.get());
        System.out.println(f5.get());
    }
}

public class ThreadPoolTest3 {
    public static void main(String[] args) {
        ExecutorService pool = Executors.newFixedThreadPool(3);
        pool.execute(new MyRunnable());
        pool.execute(new MyRunnable());
        pool.execute(new MyRunnable());
        pool.execute(new MyRunnable());

    }
}

```
## 线程的生命周期

**线程的状态：**也就是线程从生到死的过程，以及中间经历的各种状态及状态转换,理解线程的状态有利于提升并发编程的理解能力

**Java线程的状态:** java总共定义了6种状态   6种状态都定义在Thread类的内部枚举类中。

线程状态 |描述
-|-
NEW(新建)| 线程刚被创建，但是并未启动。
Runnable(可运行) |线程已经调用了start()等待CPU调度
Blocked(锁阻塞) |线程在执行的时候未竞争到锁对象，则该线程进入Blocked状态；。
Waiting(无限等待) |一个线程进入Waiting状态，另一个线程调用notify或者notifyAll方法才能够唤醒
Timed Waiting(计时等待)|同waiting状态，有几个方法有超时参数，调用他们将进入Timed Waiting状态。<br>带有超时参数的常用方法有Thread.sleep 、Object.wait。
Teminated(被终止) |因为run方法正常退出而死亡，或者因为没有捕获的异常终止了run方法而死亡。


**简单来看**
新建状态（ NEW ）=>  创建线程对象
就绪状态（ RUNNABLE ）=> start方法
阻塞状态（ BLOCKED ）=>  无法获得锁对象
等待状态（ WAITING ）=> wait方法
计时等待（ TIMED_WAITING ）=>  sleep方法
结束状态（ TERMINATED ）=>  全部代码运行完毕
### 定时器
定时器是一种控制任务延时调用，或者周期调用的技术。
**作用：**闹钟、定时邮件发送。
**方式一：Timer**
构造器 |说明
-|-
public Timer()| 创建Timer定时器对象

方法 |说明
-|-
public void schedule (TimerTask task, long delay, long period) |开启一个定时器，按照计划处理TimerTask任务

Timer定时器的特点和存在的问题
1. Timer是单线程，处理多个任务按照顺序执行，存在延时与设置定时器的时间有出入。
2. 可能因为其中的某个任务的异常使Timer线程死掉，从而影响后续任务执行。

**方式二： ScheduledExecutorService**

**定时器的实现方式**
ScheduledExecutorService是 jdk1.5中引入了并发包，目的是为了弥补Timer的缺陷, ScheduledExecutorService内部为线程池
Executors的方法| 说明
-|-
public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize) |得到线程池对象

ScheduledExecutorService的方法 |说明
-|-
public ScheduledFuture<?> scheduleAtFixedRate(Runnable command, long initialDelay, long period,TimeUnit unit)|周期调度方法

**ScheduledExecutorService的优点**
基于线程池，某个任务的执行情况不会影响其他定时任务的执行。
# 六、单元测试和反射
## 单元测试
单元测试就是针对最小的功能单元编写测试代码，Java程序最小的功能单元是方法，因此，单元测试就是针对Java方法的测试，进而检查方法的正确性

**Junit单元测试框架：** JUnit是使用Java语言实现的单元测试框架，它是开源的，Java开发者都应当学习并使用JUnit编写单元测试。此外，几乎所有的IDE工具都集成了JUnit，这样我们就可以直接在IDE中编写并运行JUnit测试。

**JUnit优点:**
1. JUnit可以选择执行哪些测试方法，可以一键执行全部测试方法的测试。
2. JUnit可以生测试报告，如果测试良好则是绿色；如果测试失败，则是红色。
3. 单元测试中的某个方法测试失败了，不会影响其他测试方法的测试

**单元测试快速入门**
1. 将JUnit的jar包导入到项目中
IDEA通常整合好了Junit框架，一般不需要导入。
如果IDEA没有整合好，需要自己手工导入如下2个JUnit的jar包到模块
2. 编写测试方法：该测试方法必须是公共的无参数无返回值的非静态方法。
3. 在测试方法上使用@Test注解：标注该方法是一个测试方法
4. 在测试方法中完成被测试方法的预期正确性测试。
5. 选中测试方法，选择“运行” ，如果测试良好则是绿色；如果测试失败，则是红色


**单元测试常用注解：**
**Junit常用注解(Junit 4.xxxx版本)**
注解 |说明
-|-
@Test| 测试方法
@Before |用来修饰实例方法，该方法会在每一个测试方法执行之前执行一次。
@After |用来修饰实例方法，该方法会在每一个测试方法执行之后执行一次。
@BeforeClass |用来静态修饰方法，该方法会在所有测试方法之前只执行一次。
@AfterClass |用来静态修饰方法，该方法会在所有测试方法之后只执行一次。

**Junit常用注解(Junit 5.xxxx版本)**

注解 |说明
-|-
注解 |说明
@Test |测试方法
@BeforeEach |用来修饰实例方法，该方法会在每一个测试方法执行之前执行一次。
@AfterEach| 用来修饰实例方法，该方法会在每一个测试方法执行之后执行一次。
@BeforeAll |用来静态修饰方法，该方法会在所有测试方法之前只执行一次。
@AfterAll |用来静态修饰方法，该方法会在所有测试方法之后只执行一次

开始执行的方法:初始化资源，执行完之后的方法:释放资源。
## 反射
**反射概述：**

1. 反射是指对于任何一个Class类，在"运行的时候"都可以直接得到这个类全部成分。
2. 在运行时,可以直接得到这个类的构造器对象：Constructor
3. 在运行时,可以直接得到这个类的成员变量对象：Field
4. 在运行时,可以直接得到这个类的成员方法对象：Method
5. 这种运行时动态获取类信息以及动态调用类中成分的能力称为Java语言的反射机制。

**反射的关键：**
反射的第一步都是先得到编译后的Class类对象，然后就可以得到Class的全部成分。
```java
HelloWorld.java -> javac -> HelloWorld.class
Class c = HelloWorld.class;
```
---
**反射获取类对象**

反射的第一步是获取Class类对象，才可以解析类的全部成分
. 获取Class类的对象的三种方式
 方式一：Class c1 = Class.forName(“全类名”);
 方式二：Class c2 = 类名.class
 方式三：Class c3 = 对象.getClass();
 
```java
public class Test {
	public static void main(String[] args) throws Exception {
        // 1、Class类中的一个静态方法：forName(全限名：包名 + 类名)
        System.out.println(Class.forName("com.chq.Student"));
        // 2、类名.class
        System.out.println(Student.class);
        // 3、对象.getClass() 获取对象对应类的Class对象。
        System.out.println(new Student().getClass());
    }
}

```
---
**反射获取构造器对象**

第一步：获得class对象
第二步：获得Constructor对象
第三步：创建对象

**使用反射技术获取构造器对象并使用**
反射的第一步是先得到类对象，然后从类对象中获取类的成分对象。

**Class类中用于获取构造器的方法:**
方法 |说明
-|-
Constructor<?>[] getConstructors ()| 返回所有构造器对象的数组（只能拿public的）
Constructor<?>[] getDeclaredConstructors ()| 返回所有构造器对象的数组，存在就能拿到
Constructor<T> getConstructor (Class<?>... parameterTypes)| 返回单个构造器对象（只能拿public的）
Constructor<T> getDeclaredConstructor (Class<?>... parameterTypes)| 返回单个构造器对象，存在就能拿到

获取构造器的作用依然是初始化一个对象返回

**Constructor类中用于创建对象的方法**
符号 |说明
-|-
T newInstance(Object... initargs) |根据指定的构造器创建对象
public void setAccessible(boolean flag)| 设置为true,表示取消访问检查，进行暴力反射




---
**使用反射技术获取成员变量对象**

第一步：获得class对象
第二步：获得Field对象
第三步：赋值或者获取值

 **使用反射技术获取成员变量对象并使用：**
 反射的第一步是先得到类对象，然后从类对象中获取类的成分对象。
 **Class类中用于获取成员变量的方法：**
方法 |说明
-|-
Field[] getFields() |返回所有成员变量对象的数组（只能拿public的）
Field[] getDeclaredFields()| 返回所有成员变量对象的数组，存在就能拿到
Field getField(String name)| 返回单个成员变量对象（只能拿public的）
Field getDeclaredField(String name)| 返回单个成员变量对象，存在就能拿到


**Field类中用于取值、赋值的方法**
符号 |说明
-|-
void set(Object obj, Object value)| 赋值
Object get(Object obj) |获取值。


如果某成员变量是非public的，需要打开权限（暴力反射），然后再取值、赋值 setAccessible(boolean)

---

**反射获取方法对象**
第一步：获得class对象
第二步：获得Method对象
第三步：运行方法

**使用反射技术获取方法对象并使用：**
反射的第一步是先得到类对象，然后从类对象中获取类的成分对象。

**Class类中用于获取成员方法的方法：**
方法 |说明
-|-
Method[] getMethods ()| 返回所有成员方法对象的数组（只能拿public的）
Method[] getDeclaredMethods ()| 返回所有成员方法对象的数组，存在就能拿到
Method getMethod (String name, Class<?>... parameterTypes)| 返回单个成员方法对象（只能拿public的）
Method getDeclaredMethod (String name, Class<?>... parameterTypes) |返回单个成员方法对象，存在就能拿到

获取成员方法的作用依然是在某个对象中进行执行此方法

**Method类中用于触发执行的方法：**
符号 |说明
-|-
Object invoke (Object obj, Object... args)|运行方法<br> 参数一：用obj对象调用该方法<br>参数二：调用方法的传递的参数（如果没有就不写）<br>返回值：方法的返回值（如果没有就不写）

---
**反射的作用-绕过编译阶段为集合添加数据**

**可以给约定了泛型的集合存入其他类型的元素：** 编译成Class文件进入运行阶段的时候，泛型会自动擦除。反射是作用在运行时的技术，此时已经不存在泛型了。

