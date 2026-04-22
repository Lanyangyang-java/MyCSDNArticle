

@[TOC](JavaSE   JAVA基础总结 （上）  )

---

**前言：** 学习java有了两年半了，之前从没在CSDN上发布，这也是将之前所学习的javaSE的笔记，记录在CSDN上，同时这是是我的第一篇博客，如有写的不对的地方，欢迎大佬斧正!!!

java基础总结下在这：[JavaSE JAVA基础总结（下） 我的学习笔记](https://blog.csdn.net/qq_60972641/article/details/142926649?spm=1001.2014.3001.5501)
# 一、Java基础语法 
### 1.数据类型
数据类型的作用：数据类型就是用来约束变量储存数据的形式 。 
***数据类型  变量名称 = 初始值；***


关键字 |数据类型 |  取值范围 | 内存占用
-------- | ----- | ----- | -----
byte  | 整形|-128~127  | 1个字节
short  | 整形|-32,768~32,767| 2个字节
int  | 整形| -2,147,483,648~2,147,483,647 | 4个字节
long |整形| -9,223,372,036,854,775,808到9,223,372,036,854,775,807 | 8个字节
flaot | 浮点数  | 大约±3.4E+38（有效数字大约6~7位） | 4个字节
double | 浮点数| 大约±1.7E+308（有效数字大约15位） | 8个字节
char | 字符  | 0~65535| 通常是1个字节，Java中占用2个字节
boolean |  布尔 | true  false | 1个字节

除了以上的基础类型还有引用数据类型基本上可以分为三类：
类Class引用，接口interface引用 ，数组引用；可以是我们创建的，也可以是java库中的
### 2.运算符
**算数运算符:**
| 运算符 | 名称 |说明|
|--|--|--|
|+  |加法运算符 | 可以求和，可以拼接String类型的字符串|
|-  |减法运算符 | a=-a 取反， a-b 相减|
|*  |乘法运算符 | 相乘|
|/  |除法运算符 | 求商|
|++  |自增运算符 | a++ 先取值再自增1，++a 先自增1再取值|
|--  |自减运算符| a-- 先取值再自减1，b--先自减1再取值|

**关系运算符或比较运算符：**
| 运算符 | 名称 |说明|
|--|--|--|
|> |大于 | 检查左边的值是否大于右边的值|
|< |小于 | 检查左边的值是否小于右边的值|
|>= |大于等于 | 检查左边的值是否大于或等于右边的值。|
|<=|小于等于| 检查左边的值是否小于或等于右边的值|
| ==  |等于 | 检查两个值是否相等|
| !=  |不等于 | 检查两个值是否不相等|

**逻辑运算符：**
| 运算符 | 名称 |说明|
|--|--|--|
|&& |逻辑与 | 当且仅当两个操作数都为true时，结果才为true。|
|&#124;&#124;|逻辑或 |如果任一值为为true，结果就为true。|
|! |逻辑非 | 真值或假值转换为对应的相反值|

**赋值运算符：**
| 运算符 | 名称 |说明|
|--|--|--|
|= |基本赋值运算符 | 将一个值或表达式的结果存储在变量中|
|+=|加法赋值 | 右边的值加到左边的的值上，结果存入左边|
|-= |减法赋值 | 两变量相除|
|*=|乘法赋值 | 两变量相乘|
| /= |除法赋值 |求值，如a/b的值 |
|%  |取模赋值| 求模，如a%b的模|

**条件运算符（三目运算符）：**
**boolean表达式  ？表达式1：表达式2;**   表达式为true执行表达式1，表达式为false，则执行表达式2。

如 1 > 2 ? a = 1：a =  2;  此时a的值为 2。


### 3.选择结构
选择结构按照顺序结构按照编写的顺序，从上到下依次运行，整体流程是差不多的，像 if ，if....else...，switch 等
**执行流程可以概况为:**
1.首先计算关系表达式1的值
2.如果值为true就执行语句体1；
3.如果值为false就计算关系表达式2的值
4.如果值为true就执行语句体2；如果值为false就计算关系表达式3....
5.所有表达式均不满足条件则，执行后面的语句内容
**选择结构语法：**
**if结构:**
```java
if(结果为boolean的表达式){
        语句体;
 	}
```
**if....else...结构:**
```java
if(结果为boolean的表达式){
        语句体1;
    } else  {
        语句体2;
    }
```
**switch结构:**
```java
switch(表达式){
        case 值1:
            语句体1
            break;
        case 值2:
            语句体2
            break;
        default:
            语句体3
            break;
```
**if...else if...else结构:**
```java
if(条件表达式1){
    语句体1
}else if(条件表达式2){
    语句体2
}else{
    语句体3
}
```

### 4.循环结构
在某些条件满足的情况下，反复执行特定代码的功能，常见的循环结构有for，while，do...while
**循环四要素：**
1.循环变量的初始化表达式。
2.循环条件。
3.循环变量的修改。
4.循环体语句块。
**循环结构语法：**
**for循环格式**
```java
for(初始化条件1(初始表达式);循环条件语句2(循环条件);迭代语句3(循环变量的修改){
         循环体语句4;
 }
 /**
 循环流程：
 1.执行初始化语句，完成循环变量的初始化；
 2.执行循环条件语句 2，看循环条件语句的值是 true ，还是 false；
                 如果是 true ，执行第三步；
                 如果是 false ，循环语句中止，循环不再执行；
 3.执行循环体语句3；
 4.执行迭代语句 4 ，针对循环变量重新赋值；
 5.根据循环变量的新值，重新从第二步开始再执行一遍；
 */
```
**while循环格式**
```java
1.初始化语句;
while(2.循环条件 Boolean类型的表达式){
       3.循环体语句;
       4.条件控制语句;
}
/**
 循环流程：
 1.执行初始化语句
 2.执行条件判断语句，看其结果是true还是false
             如果是false，循环结束
             如果是true，继续执行
 3.执行循环体语句
 4.执行条件控制语句
 5.回到2继续
 */
```
**do...while循环格式**
```java
//1.初始化语句;
do{
    2.循环体语句;
    3.条件控制语句;
}while(条件判断语句);
/**
 循环流程：
 1.执行初始化语句
 2. 执行循环体语句
 3. 执行条件控制语句
 4. 执行条件判断语句，看其结果是true还是false
             如果是false，循环结束
             如果是true，继续执行
 5.回到2继续
 */
```
小结：while是谈好了在干，do...while是干了再说，知道循环多少次最好用for，不知道用while。

**补充：**
死循环：没有循环次数，或不知道什么时候结束的循环被称为死循环，死循环
死循环格式
```java
//for死循环格式 :
for(初始化条件1;;迭代语句3){

}

//while死循环格式 :
while(true){

}

//do..while死循环格式 :
do{

}while(true);
```
### 5.数组

**概念:** 相同类型数据的有序集合
**特点：**
1.数组的长度一旦确定不能修改
2.创建数组的时候会在内存中开辟一块连续的空间
3.存取元素的速度快，因为可以通过 [下标index] ，直接定位到任意一个元素
4.数组是引用类型，如果直接打印，打印的是数组的地址


**数组的声明和初始化**
数组的声明：数据类型[] 数组名;
```java
int [] arr; 声明了arr数据 数据类型为int类型
```
```java
静态初始化：直接给数组把元素定义好
int [] arr = {1,2,3}; 直接规定好
```
```java
动态初始化：给数组规定长度，然后手动给数组每一个位置添加元素
int [] arr = new arr[10]; 直接规定好此数组长度为10 ，手动添加元素
```


**数组四部走**
1.声明数组，告诉计算数据类型是什么
2.分配数组，告诉计算机有几个连续的空间
3.赋值，向格子里面放数据
4.数据处理
```java
int [] arr;
arr = new int[5];
arr[0] = 1;
arr[0] *= 10;
```

**数组遍历**
就是将数组中的每个元素分别获取出来。

### 6.方法
是一段用来完成''特定功能''的代码片段（代码块；在c语言中也称之为函数），Java 里面的方法不能独立存在，所有的方法必须定义在类里面。

**方法语句体：**

修饰符 返回值类型 方法名( [参数类型1 参数名1, 参数类型2 参数名2,...] ){
           方法体; 
    return 返回值;
}

参数列表中声明的变量：形式参数==>实际参数，形式参数与实际参数为同一数据类型

**方法三要素：**
返回值、方法名、参数列表

**方法的作用：**
1.把复用的逻辑抽取出来，封装成方法，提高代码的重用性
2.实现相对独立的逻辑，提高代码的维护性
3.可以对具体实现进行隐藏、封装

**方法的调用：**
方法的调用这里认识一个关键字 **static** ，被**static** 修饰的的方法为静态方法，否则就是非静态方法。也可以说被 **static**修饰的属于类的，通过"类名.方法名（）"调用。没有被static修饰属于对象，通过"对象名.方法（）"调用。
在这里我们闯将两个类 一个小猫 一个小狗,小猫类中的cat方法用static修饰，小狗类的dog方法没有被static修饰
```java
public class Cat {
    public static void cat(){
        System.out.println("小猫吃鱼");
    }
}
```
```java
public class Dog {
    public void dog(){
        System.out.println("小狗吃肉");
    }
}
```
测试类
```java
public class Test1 {
    public static void main(String[] args) {
        //调用cat方法
        Cat.cat();
        //调用dog方法
        Dog d = new Dog();
        d.dog();
    }
}
```
方法也是可以调用方法的
```java
public class Test2 {
    public static void main(String[] args) {
        cat();
    }
    public static void cat(){
        System.out.println("小猫吃鱼");
        dog();
    }
    public static void dog(){
        System.out.println("小狗吃肉");
    }
}
```

**方法的重载：**
同一个类中，方法名字相同，参数列表不同，则是方法重载。

注意：
1.方法重载指同一个类中定义的多个方法之间的关系，
2.参数列表的不同包括，参数个数不同，参数数据类型不同，参数顺序不同
3.方法的重载与方法的修饰符和返回值没有任何关系

作用：
可读性好，方法名称相同，调用的时候，就不需要记那么多的方法名称，而是知道了方法的功能就可以直接的给他传递不同的参数

方法重载的识别技巧：
1.只要是同一个类中，方法名称相同、形参列表不同，那么他们就是重载的方法，其他都不管！
（如：修饰符，返回值类型都无所谓）
2.形参列表不同指的是：形参的个数、类型、顺序不同，不关心形参的名称

**方法的重写：**

规则：
1.参数列表必须完全与被重写的方法参数列表相同。
2.返回的类型必须与被重写的方法的返回类型相同（Java1.5 版本之前返回值类型必须一样，之后的 Java 版本放宽了限制，返回值类型必须小于或者等于父类方法的返回值类型）。
3.访问权限不能比父类中被重写方法的访问权限更低（public>protected>default>private）。
4.重写方法一定不能抛出新的检査异常或者比被重写方法声明更加宽泛的检査型异常。例如，父类的一个方法声明了一个检査异常IOException，在重写这个方法时就不能抛出 Exception，只能拋出 IOException 的子类异常，可以抛出非检査异常。

注意：
1.重写的方法可以使用 @Override 注解来标识。
2.父类的成员方法只能被它的子类重写。
3.声明为 final 的方法不能被重写。
4.声明为 static 的方法不能被重写，但是能够再次声明。
5.构造方法不能被重写。
6.子类和父类在同一个包中时，子类可以重写父类的所有方法，除了声明为 private 和 final 的方法。
7.子类和父类不在同一个包中时，子类只能重写父类的声明为 public 和 protected 的非 final 方法。
8.如果不能继承一个方法，则不能重写这个方法。


**方法常见问题：**
1.方法的编写顺序无所谓。
2.方法与方法之间是平级关系，不能嵌套定义。
3.方法的返回值类型为void（无返回值），方法内则不能使用return返回数据，如果方法的返回值类型写了具体类型，方法内部则
4.必须使用return返回对应类型的数据。
5.return语句下面，不能编写代码，因为永远执行不到，属于无效的代码。
6.方法不调用就不执行, 调用时必须严格匹配方法的参数情况。
7.有返回值的方法调用时可以选择定义变量接收结果，或者直接输出调用，甚至直接调用；无返回值方法的调用只能直接调用。

### 7.递归
指在当前方法内调用自己的这种现象。

递归的分类:
1.递归分为两种，直接递归和间接递归。
2.直接递归称为方法自身调用自己。
3.间接递归可以A方法调用B方法，B方法调用C方法，C方法调用A方法。

注意事项：
1.递归一定要有条件限定，保证递归能够停止下来，否则会发生栈内存溢出。
2.在递归中虽然有限定条件，但是递归次数不能太多。否则也会发生栈内存溢出。
3.构造方法,禁止递归， 构造方法 创建对象使用的，不能让对象一直创建下去。

**样例**
递归累加求和
```java
public static void main(String[] args) {
        //求1-100的累加和
        System.out.println(sum(100));
    }

    public static int sum(int n){
        if (n == 1){
            return 1;
        }else {
            return n + sum(n-1);
        }
    }
    //结果为5050
```
---
在java基础语法练习中，有两个常用的对象
输入对象Scanner
```java
public static void main(String[] args) {
		//创建 Scanner 对象的基本语法
        Scanner scanner = new Scanner(System.in);
        //输入int类型
        int sc1 = scanner.nextInt();
        //输入字符串类型
        String sc2 = scanner.next();
        //输出
        System.out.println(sc1);
        System.out.println(sc2);
    }
```
随机数Random
random() 方法返回随机生成的一个实数 区间左闭右开
```java
public static void main(String[] args) {
        Random random = new Random();
        int r1 = random.nextInt(10);
        System.out.println("生成一个0-9的随机数=>" + r1);
        int r2 = random.nextInt(10) + 10;
        System.out.println("生成一个10-20的随机数=>" + r2);
    }
```
# 二、面向对象
---
**面向对象介绍：**
并不是一门技术，而是一门编程思想，把现实世界物体全部看成一个个对象来解决实际问题
## 1.面向对象编程
####  （1）为什么要学习面向对象
从解决问题的角度来看：将复杂的问题简单化
从程序设计的角度来看：提高代码的可维护性，可拓展性，可重复性

简单来说就是：生活中我们解决问题就是按照对象化方式进行的，如果程序也能按照的方式来解决问题，那么程序就更符合人类的思维习惯。
把现实世界物体全部看成一个个对象来解决实际问题，代码看起来更容易理解、更简单、更易维护。
#### （2）面向过程与面向对象区别
**面向对象：** 
以对象为核心，强调事件的角色和主体。面向对象编程将构成问题的事物分解成若干个对象，每个对象都包含自身的属性和行为。对象通过消息传递进行交互，从而完成特定的功能。

**特点：** 具有封装、继承和多态等特性。封装将数据和行为封装在一起，形成一个独立的对象；继承使得子类能够共享父类的属性和方法，从而实现代码的复用；多态则允许不同的对象对同一消息作出不同的响应。

**优势：** 
高度拓展性和复用性
易于维护 
耦合度低

---
**面向过程：**
以过程为核心，强调事件的流程和顺序。面向过程编程将问题分解为一系列步骤，然后按照这些步骤的顺序编写代码。每个步骤通常由一个函数来实现，函数之间通过调用关系来组织。

**特点：**注重流程化和模块化。面向过程编程将问题分解为一系列步骤，并按照这些步骤的顺序编写代码。每个步骤通常由一个函数来实现，函数之间通过调用关系来组织。

**优势：**
执行效率高
直观易懂
#### （3）对象
对象：是真实存在的具体实例，具有行为和特征的实体（万物皆对象），在Java中，必须先设计类，才能获得对象

**特征：** 称为属性，一般为名词，代表对象有什么
**行为：** 称为方法，一般为动词，代表对象能做什么


创建对象
**语法：** 类名 对象名 = new 新类名（）；
**注意：** 这里的类名不仅仅是类，也是数据类型
**对象创造过程**
1.内存中开辟对象空间
2.为各个属性赋予初始值
3.执行构造方法的代码
4.将对象的地址赋值给变量
#### （4）类
类：是对象共同特征的描述。


定义了对象应有的特征行为，类是对对象的抽象模板，同时，对象拥有多个相同特征和行为，对象是对类的具体化
一个Java文件中只能有一个定义一个类被public修饰

class 黑匣子{//告诉计算机这一类事物的名字是黑匣子
}
```java
public class Student {
// 属性 (成员变量)
	String name;
// 行为（方法）
	public void study(){
	
	}
}
```
**定义类的补充注意事项：**
1.成员变量的完整定义格式是：修饰符 数据类型 变量名称 =初始化值 ,一般无需指定初始化值，存在默认值。
2.类名首字母建议大写、英文、有意义，满足驼峰模式，不能用关键字，满足标志符规定。
3.一个代码文件中可以定义多个类，但是只能一个类是public修饰的，public修饰的类名必须是Java代码的文件名称
#### （5）成员变量与局部变量的区别
成员变量：是在类里面定义的变量
局部变量：指的是在方法里面定义的变量

| 区别 | 局部变量 |成员变量|
|--|--|--|
|位置 |定义在方法内 | 定义在方法外，类的里面|
|默认值 |没有默认值 | 系统提供默认值|
|作用域|所在代码块内{} | 在整个类中|
|是否重名|在不同作用域可以重名| 不可以重名|
| 生命周期|方法执行完| 对象被销毁|


#### （6）构造器
**构造器的作用:**
用于初始化一个类的对象，并返回对象的地址。
```java
Car c = new Car();
```
构造器的定义格式:
```java
/**
修饰符 类名(形参列表){
	...
}
*/

public class Car {
...
// 无参数构造器
public Car(){
...
}
// 有参数构造器
public Car(String name){
	...
}
}
```
初始化对象的格式:
```java
//类名 对象名称 = new 构造器；
Car c = new Car();
```

**构造器的分类:**
1.无参数构造器（默认存在的）：初始化的对象时，成员变量的数据均采用默认值。
2.有参数构造器:在初始化对象的时候，同时可以为对象进行赋值。

**注意事项**
任何类定义出来，默认就自带了无参数构造器，写不写都有。
```java
public class Car {
	...
	// 无参数构造器（默认存在的）
}
```
一旦定义了有参数构造器，无参数构造器就没有了，此时就需要自己写一个无参数构造器了。
```java
public class Car {
	...
	// 无参数构造器（需要写出来了）
	public Car(){
	}
	// 有参数构造器
	public Car(String n, String p){
	}
}
```
#### （7）this的关键字
this关键字可以出现在成员方法、构造器中，代表当前对象的地址。

**this的作用:**
访问当前对象的成员变量、成员方法。

1.this.属性名
（调用本类的属性）
2.this.方法名
（调用本类的方法）
3.this（参数1，参数2....）
（调用本类的构造方法，一般很少用）


先写一个This的实体类ThisTest
```java
public class ThisTest {

    private String name;

    public void show(String name){
        //这个name是show中传入的参数“蔡徐坤”
        System.out.println(name); 
        //这个name被this调用 调用本类的name属性 本类的name属性被set方法赋值为“懒洋洋”
        System.out.println(this.name); 
        //同事this还可以调用本类的构造函数 但一般不常用
        this.Cat();
    }
    public void setName(String name) {
        this.name = name;
    }
    public static void Cat(){
        System.out.println("喵喵叫");
    }
}

```
再写一个测试类
```java
public class ThisMain {
    public static void main(String[] args) {
        ThisTest test = new ThisTest();
        test.setName("懒洋洋");
        test.show("蔡徐坤");
    }
}

/**输出结果为：
		蔡徐坤
		懒洋洋
		喵喵叫
*/
```
#### （8）标准JavaBean
也可以理解成实体类，其对象可以用于在程序中封装数据

**标准JavaBean须满足如下要求:**
1.成员变量使用 private 修饰
2.提供每一个成员变量对应的 setXxx() / getXxx()。
3.必须提供一个无参构造器。


#### （9）静态关键字：static
**static的作用、修饰成员变量的用法:**
***static关键字的作用:***
静态成员变量（有static修饰，属于类、加载一次，可以被共享访问），访问格式。
```java
//类名.静态成员变量
```
实例成员变量（无static修饰，属于对象），访问格式：
```java
//对象.实例成员变量
```
两种成员变量各自在什么情况下定义:
静态成员变量：表示在线人数等需要被共享的信息。
实例成员变量：属于每个对象，且每个对象信息不同时（name,age,…等）

每种成员方法的使用场景是怎么样的:
1.表示对象自己的行为的，且方法中需要访问实例成员的，则该方法必须申明成实例方法。
2.如果该方法是以执行一个通用功能为目的，或者需要方便访问，则可以申明成静态方法

**static的注意事项**
1.静态方法只能访问静态的成员，不可以直接访问实例成员。
2.实例方法可以访问静态的成员，也可以访问实例成员。
3.静态方法中是不可以出现this关键字的。

#### （10）代码块
**代码块概述:**代码块是类的5大成分之一（成员变量、方法、构造器、代码块，内部类），定义在类中方法外。
在Java类下，使用 { } 括起来的代码被称为代码块 。

**代码块分为类：**
静态代码块:
格式：static{}
特点：需要通过static关键字修饰，随着类的加载而加载，并且自动触发、只执行一次
使用场景：在类加载的时候做一些静态数据初始化的操作，以便后续使用。

 构造代码块（了解，用的少）：
格式：{}
特点：每次创建对象，调用构造器执行时，都会执行该代码块中的代码，并且在构造器执行前执行
使用场景：初始化实例资源。

**静态代码块的作用：**
如果要在启动系统时对数据进行初始化。


#### （11）单例设计模式

**设计模式：**
开发中经常遇到一些问题，一个问题通常有n种解法，但其中肯定有一种解法是最优的，这个最优
的解法被人总结出来了，称之为设计模式。设计模式有20多种，对应20多种软件开发中会遇到的问题，学设计模式主要是学2点：
1.这种模式用来解决什么问题。
2.遇到这种问题了，该模式是怎么写的，他是如何解决这个问题的。

**单例模式：**
可以保证系统中，应用该模式的这个类永远只有一个实例，即一个类永远只能创建一个对象
例如任务管理器对象我们只需要一个就可以解决问题了，这样可以节省内存空间。
***1.饿汉单例设计模式***
在用类获取对象的时候，对象已经提前为你创建好了，是立即加载的方式，无论是否会用到这个对象，都会加载

设计步骤：
1.定义一个类，把构造器私有。
2.定义一个静态变量存储一个对象。
```java
//定义一个单例类
public class SingleInstance {
	//定义一个静态变量存储一个对象即可 :属于类，与类一起加载一次 
	public static SingleInstance instance = new SingleInstance ();
	//单例必须私有构造器
	private SingleInstance (){
		System.out.println("创建了一个对象");
	}
}
```
**懒汉单例模式:**
在真正需要该对象的时候，才去创建一个对象(延迟加载对象)，是延迟加载的方式，只有使用的时候才会加载。 并且有线程安全的考量
设计步骤：
1.定义一个类，把构造器私有。
2.定义一个静态变量存储一个对象。
3.提供一个返回单例对象的方法
```java
// 定义一个单例类
class SingleInstance{
	//定义一个静态变量存储一个对象即可 :属于类，与类一起加载一次
	public static SingleInstance instance ;
	//单例必须私有构造器
	private SingleInstance(){}
	//必须提供一个方法返回一个单例对象
	public static SingleInstance getInstance(){
		...
		return ...;
	}
}
```
#### （12）包
包是用来分门别类的管理各种不同类的，类似于文件夹、建包利于程序的管理和维护,建包的语法格式：package 公司域名倒写.技术名称。报名建议全部英文小写，且具备意义
```java
package com.chq.test;
public class Student {
}
```
建包语句必须在类的最上面，一般IDEA工具会帮助创建

**导包：**

相同包下的类可以直接访问，不同包下的类必须导包,才可以使用！导包格式：import 包名.类名。
假如一个类中需要用到不同类，而这个两个类的名称是一样的，那么默认只能导入一个类，另一个类要带包名访问。

#### （13）权限修饰符
**权限修饰符：** 用来控制一个成员能够被访问的范围的。，可以修饰成员变量，方法，构造器，内部类，不同权限修饰符修饰的成员能够被访问的范围将受到限制。
**权限修饰符的分类和具体作用范围：**
权限修饰符：有四种作用范围由小到大（private -> 缺省 -> protected - > public ）
|修饰符|同一个类中|同一个包下的其他类|不同包下的子类|不同包下的无关类|
|--|--|--|--|--|
|private|√|||
|缺省|√|√||
|protected|√|√|√|
|public|√|√|√|√|

**自己定义成员（方法，成员变量，构造器等）一般满足如下要求：**
1.成员变量一般私有
2.方法一般公开
3.如果该成员只希望本类访问，使用private修饰
4.如果该成员只希望本类，同一个包下的其他类和子类访问，使用protected修饰
#### （14）final

**final的作用:**
1.final 关键字是最终的意思，可以修饰（方法，变量，类）
2.修饰方法：表明该方法是最终方法，不能被重写。
3.修饰变量：表示该变量第一次赋值后，不能再次被赋值(有且仅能被赋值一次)。
4.修饰类：表明该类是最终类，不能被继承。

**final修饰变量的注意:**
1.final修饰的变量是基本类型：那么变量存储的数据值不能发生改变。
2.final修饰的变量是引用类型：那么变量存储的地址值不能发生改变，但是地址指向的对象内容是可以发生变化的。
#### （15）常量
常量是使用了public static final修饰的成员变量，必须有初始化值，而且执行的过程中其值不能被改变
```java
public class Constant {
	public static final String LOGIN_NAME = “admin";
	public static final String PASS_WORD = “123456";
}
```
配置信息，方便程序的维护，同时也能提高可读性,

**常量命名规范：** 英文单词全部大写，多个单词下划线连接起来。

**常量的执行原理**
在编译阶段会进行“宏替换”，把使用常量的地方全部替换成真实的字面量。
这样做的好处是让使用常量的程序的执行性能与直接使用字面量是一样的。

**选择常量做信息标志和分类**
代码可读性好，实现了软编码形式。
虽然可以实现可读性，但是入参值不受约束，代码相对不够严谨

#### （16）枚举
枚举是Java中的一种特殊类型
**枚举的作用：** 是为了做信息的标志和信息的分类。
**定义枚举类的格式：**
```java
/**
修饰符 enum 枚举名称{
	第一行都是罗列枚举类实例的名称。
}
*/

enum Season{
	SPRING , SUMMER , AUTUMN , WINTER;
}

```
**枚举的特征：**
枚举类都是继承了枚举类型：java.lang.Enum
枚举都是最终类，不可以被继承。
构造器都是私有的，枚举对外不能创建对象。
枚举类的第一行默认都是罗列枚举对象的名称的。

**枚举做信息标志和分类：**
代码可读性好，入参约束严谨，代码优雅，是最好的信息分类技术！建议使用!

#### （17）抽象类
在Java中abstract是抽象的意思，如果一个类中的某个方法的具体实现不能确定，就可以申明成abstract修饰的抽象方法（不能写方法体了），这个类必须用abstract修饰，被称为抽象类。

一个类中没有包含足够的信息来描绘一个具体的对象，这样的类就是抽象类。

格式：
```java
//修饰符abstract返回值类型方法名称(形参列表);
//修饰符 abstract class 类名
public abstract class Animal{
	public abstract void run();
}

```
**抽象类的作用:** 可以被子类继承、充当模板的、同时也可以提高代码复用。
**抽象方法是:** 只有方法签名，没有方法体，使用了abstract修饰。
**继承抽象类:** 一个类如果继承了抽象类，那么这个类必须重写完抽象类的全部抽象方法。否则这个类也必须定义成抽象类。

**特征和注意事项**
1.有得有失:得到了抽象方法，失去了创建对象的能力。
2.抽象类中不一定有抽象方法，有抽象方法的类一定是抽象类
3.一个类继承了抽象类必须重写完抽象类的全部抽象方法，否则这个类也必须定义成抽象类

**final和abstract关系:** abstract定义的抽象类作为模板让子类继承，final定义的类不能被继承。

抽象类可以定义属性  也可以有模板方法  模板方法子类通用方法，也可以在抽象类中定义抽象方法 子类去实现抽象方法
样例计算AB两个银行利息 和 查询总额：
账户类
```java
public abstract class Amount {
    private String cardId;
    private double money;
    
    public Amount(String cardId, double money) {
        this.cardId = cardId;
        this.money = money;
    }

    public String getCardId() {
        return cardId;
    }

    public void setCardId(String cardId) {
        this.cardId = cardId;
    }

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        this.money = money;
    }

    /**
     * 模板方法
     */
    public final void Sum(){
        System.out.println(cardId+ "有" + money);
    }
    public abstract double calc();
}

```
A银行
```java
public class A extends Amount{

    @Override
    public double calc() {
        setMoney(getMoney()+100);
        return getMoney();
    }

    public A(String cardId, double money) {
        super(cardId, money);
    }
}

```
B银行

```java
public class B extends Amount{
    @Override
    public double calc() {
        setMoney(getMoney()+1000);
        return getMoney();
    }

    public B(String cardId, double money) {
        super(cardId, money);
    }
}

```
测试
```java
public class Test {
    public static void main(String[] args) {
        A a = new A("1001",1000);
        a.calc();
        a.Sum();

        B b = new B("2001",1000);
        b.calc();
        b.Sum();
    }
}

```
运行结果
![请添加图片描述](https://i-blog.csdnimg.cn/direct/202782f5c1f44d16844206bc0e8051b2.png)


#### （18）接口
接口也是一种规范
**接口的定义与特点：**
1.接口格式
```java
//接口用interface来定义
public interface 接口名{
	//常量
	//抽象方法
}

```
2.jdk8之前只有常量和抽象方法没有其他成分了
3. jdk8z之后，java只对接口的成员变量进行了新增：默认方法	静态方法 私有方法（jdk1.9之后）
4. 接口不能实例化
5. 接口中的成员都是public修饰，写不写都是，因为规范目的是为了公开化

**接口用法：**
接口是用来被类实现（implements）的，实现接口类统称为实现类，实现类可统称为所谓的子类
```java
修饰符 class  实现类 implements 接口1，接口2，接口3，....{
}
//实现的关键字：implements
```
从上可看出，接口可以被类单实现，也可以被类多实现

接口实现的注意事项：一个类实现接口，必须重写全部的抽象方法，否则这个类就要被定义为抽象类

**基本小结**
1.类与类单继承
2.类与接口多实现
3.接口与接口多继承，一个接口可同时继承多个接口

## 2.面向对象三大特征
#### 1.封装
###### （1）什么是封装？
面向对象的三大特征之一，合理隐藏，合理暴露。一般会把成员变量使用private隐藏起来，通过getter和setter方法暴露其访问，隐藏具体实现的细节。
生活中就有许多封装的样例：
例如电插板，只给你暴露插口，而具体的细节用一个盒子（相当于上述的黑匣子 --类）给隐藏起来,这就相当于一个封装。
<figure>
	<img src="https://img-blog.csdnimg.cn/img_convert/c1cc920127bb67ef79309058501710d4.jpeg" width="400"/>
</figure>

###### （2）为什么要封装？
通过方法来控制成员变量的操作，提高代码的安全性，把功能用方法进行封装提高代码的可维护性和可重复性。
###### （3） private
private是 一个访问限定修饰符，可以用来修饰成员（成员变量，成员方法）
**特点：**
1.被private修饰只能在本类进行访问
2.针对private修饰的成员变量，如果需要被其他类使用，需要提供相应的（get/set）方法
3.提供“get变量名（）”方法，用于获取成员变量的名字。方法名用public修饰
4.提供“set变量（参数）”的方法，用于设置成员变量的值，方法用public修饰
 
出于安全性考虑，尽量考虑采用 封装，这样可以隐藏类的细节，只对外开放接口即可实现对象之间的交互，private会将具体细节给隐藏起来。

如此学生类

```java
public class Student {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
    
    public void show(){
        System.out.println(name);
        System.out.println(age);
    }
}
```


---
#### 2.继承
###### （1）什么是继承？
继承就是java允许我们用extends关键字，让一个类和另一个类建立起一种父子关系。 使子类继承父类的特征和方法，还可以在子类重新定义，以及追加属性和方法

###### （2）为什么要继承?
当子类继承父类后，就可以直接使用父类公共的属性和方法了。因此，用好这个技术可以很好的我们提高代码的复用性
###### （3）怎样继承?
继承通过extents关键字来实现 格式：class 子类 extends 父类 {}
```java
//Student称为子类（派生类），People称为父类
public class Student extends People {
	...
}
public class Teacher extends People {
	...
}
```
![请添加图片描述](https://i-blog.csdnimg.cn/direct/dfb55943f6b2420a83e4cbb9ac81b416.png)
###### （4）继承的特点
**特点：**
1.子类继承父类，子类可以享有父类的属性和方法，但是子类不能继承父类的构造器。
2.子类可以拥有自己的属性和方法
3.在Java中只支持单继承，但是支持多重继承，即一个子类只能直接继承一个类，但是一个父类可以有多个子类
4.Java中所有类的，都直接或间接继承自Object（当一类没有显示写出extends父类，那么这个类的父类就是继承Object）

多继承：
 如儿子继承爸爸 爸爸继承爷爷
 <figure>
	<img src=" https://i-blog.csdnimg.cn/direct/33012ace5e004679ba81f3dff71c30c3.png" width="600"/>
</figure>
*不可被继承：**
1.构造方法不可被继承
2.使用private修饰的属性和方法不可被继承
3.父类和子类不在同一个包下，使用默认的修饰符的属性和方法不能被继承

**样例**
```java
//Student称为子类（派生类），People称为父类
public class People {
    private String name;
    private int age;

    /**
     * 共同行为
     */
    public void eat(){
        System.out.println(name+"吃东西");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```
子类Student
```java
public class Student extends People{
    private double grade;

    /**
     * 独有行为
     */
    public void study(){
        System.out.println(getName() + "学习");
    }

    public void eat(){
        System.out.println(getName() + "吃大餐");
    }

    public double getGrade() {
        return grade;
    }

    public void setGrade(double grade) {
        this.grade = grade;
    }
}

```
测试
```java
public class Test {
    public static void main(String[] args) {
        Student student = new Student();
        student.setName("坤哥"); //父类的
        student.setAge(18); //父类的
        student.setGrade(80.5); //子类的
        System.out.println(student.getName());
        System.out.println(student.getAge());
        System.out.println(student.getGrade());
        student.eat();//父类
        student.study();//子类
    }
}
```
###### （5）继承后：成员变量、成员方法的访问特点
在子类方法中访问成员（成员变量、成员方法）满足：**就近原则**
1.先子类局部范围找
2.然后子类成员范围找
3.然后父类成员范围找，如果父类范围还没有找到则报错。
就近原则，子类没有找子类、子类没有找父类、父类没有就报错！

如果子父类中，出现了重名的成员，会优先使用子类的，此时如果一定要在子类中使用父类的怎么办？
可以通过super关键字，指定访问父类的成员。
格式：
```java
super.父类成员变量/父类成员方法
```
###### （6）继承后：子类构造器的特点
子类继承父类后构造器的特点：子类中所有的构造器默认都会先访问父类中无参的构造器，再执行自己.
因为子类在初始化的时候，有可能会使用到父类中的数据，如果父类没有完成初始化，子类将无法使用父类的数据，子类初始化之前，一定要调用父类构造器先完成父类数据空间的初始化

**调用父类构造器**
子类构造器的第一行语句默认都是：super()，不写也存在
###### （7） 继承后：子类构造器访问父类有参构造器
**super调用父类有参数构造器的作用：**
初始化继承自父类的数据

如果父类中没有无参数构造器，只有有参构造器，则会报错。因为子类默认是调用父类无参构造器的

**子类调用父类有参构造器：**
子类构造器中可以通过书写 super(…)，手动调用父类的有参数构造器

###### （8）super关键字：
super标识直接父类对象的引用

**作用：**调用父类属性，方法，构造方法
调用属性：super.属性名
调用方法：super.方法名
调用构造方法：super（参数1，参数2...）

如果当子类没有与父类重名的属性和方法时，super完全可以省略，当有重名属性和方法时，super.属性名，super.方法名，都表示的是调用父类的属性和方法

**使用细节：**
1.在父类中有其他的构造方法的时候，尽量保留一个无参构造
2.super在调用构造方法的时候，要放在子类构造方法的第一行代码

**子类创建过程：**
子类的创建，是要先创建父类的对象，再创建子类的对象


###### （9）this、super使用总结：

this引用的是当前对象本身，可以用来访问当前对象的成员变量、成员方法.
super引用的是当前对象的父类对象，可以用来访问父类的成员变量、成员方法或构造方法。
在构造方法中，this(…)用于调用当前类的其他构造方法，而super(…)用于调用父类的构造方法。注意，这两者在构造方法中必须是第一条语句。
this和super都不能在静态方法、静态代码块或类变量声明中使用，因为它们都依赖于具体的对象实例。
| 关键字|访问成员变量 |访问成员方法|访问构造方法|
|--|--|--|--|
|this|this.成员变量访问<br>本类成员变量 |this.成员方法(…)<br>访问本类成员方法|this(…)<br>访问本类构器|
|super|super.成员变量<br>访问父类成员变量|super.成员方法(…)<br>访问父类成员方法|super(…)<br>访问父类构造器|




---
#### 3.多态
###### （1）什么是多态
同类型的对象，执行同一个行为，会表现出不同的行为特征
程序中的多态：父类指向子类的对象

**多态的常见形式:**
```java
父类类型 对象名称 = new 子类构造器;
接口 对象名称 = new 实现类构造器;
```
###### （2）为什么使用多态
使用多态可以大大提高代码的复用性,便于扩展和维护
###### （3）怎么使用多态
1.用于提升代码的复用性,作为方法的参数使用,作为方法的返回值使用
2.提高代码的拓展性，复用性
3.当父类的引用指向子类的对象时，如果子类重写了父类的方法，那么调用的就是子类重写后的方法。既体现了多种类的传递，又体现了不同类型的特征，既复用了父类的属性和方法，又拓展了自己的逻辑，并且这里的拓展，并不影响原有的逻辑。这也体现我们面向对象八大设计原则中的开闭原则：对拓展开放，对修改关闭

**多态的前提:** 有继承/实现关系；有父类引用指向子类对象；有方法重写。
###### （4）多态的特点
**优势:**在多态形式下，右边对象可以实现解耦合，便于扩展和维护。
```java
Animal a = new Cat();
a.run(); // 后续业务行为随对象而变，后续代码无需修改
```


**成员访问特点:**
成员变量编译看父类，运行看父类
成员方法编译看父类，运行看子类

**多态的条件:**
要有继承或实现关系,要有父类引用指向子类对象
###### （5）向上转型和向下转型
父类引用如果想要调用子类自己独有的方法，那么必要更发生向下转型
向上转型：父类引用指向子类对象就是向上转型
**向上转型的格式：**
```java
 父类类型 变量名=new 子类类型();
```
向下转型：将父类引用强制转为子类引用

**向下转型的格式：**
```java
子类型 引用名 = （子类型）父类引用
```
**注意：** 如果转型后的类型和对象真实类型不是同一种类型，那么在转换的时候就会出现ClassCastException
```java
Animal t = new Tortoise();
Dog d = (Dog)t; // 出现异常 ClassCastException
```
Java建议强转转换前使用instanceof判断当前对象的真实类型，再进行强制转换
变量名 instanceof 真实类型,判断关键字左边的变量指向的对象的真实类型，是否是右边的类型或者是其子类类型，是则返回true，反之返回false。
###### （6）instance关键字
当我们使用向下转型的时候，存在一定的风险：如果被转的引用类型变量，对应的实际类型和目标类型不是同一种类型，那么在转换的时候就会出现ClassCastExcException(类型转化异常)
解决方案：关键字instanceof
```java
变量名 instanceof 类型
```

通俗的理解：判断关键字左边的变量，是否是右边的类型，返回Boolean类型结果

---
**多态样例：**
以电脑USP为例

USP接口
```java
public interface USB {
    void connect();
    void unconnect();
}

```
鼠标和键盘实现USP
```java
public class Mouse implements USB{
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Mouse(String name) {
        this.name = name;
    }

    @Override
    public void connect() {
        System.out.println(name+"鼠标连接");
    }

    @Override
    public void unconnect() {
        System.out.println(name+"鼠标拔出");

    }
    /**
     * 独有方法
     */
    public void click(){
        System.out.println(name+"鼠标点击");
    }
}

```
```java
public class KeyBoard implements USB{
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public KeyBoard(String name) {
        this.name = name;
    }

    @Override
    public void connect() {
        System.out.println(name+"键盘连接");
    }

    @Override
    public void unconnect() {
        System.out.println(name+"键盘拔出");

    }
    /**
     * 独有功能
     */
    public void keyDown(){
        System.out.println(name+"键盘打字");
    }

}


```
设计电脑类
```java
public class Computer {
    /**
     * 提供一个安装的入口
     */
    public void installUSB(USB u){
        u.connect();
        //执行独有的功能
        //判断u是那个子类实现的USB
        if (u instanceof Mouse){
            Mouse m=(Mouse) u;
            m.click();
        } else if (u instanceof KeyBoard) {
            KeyBoard k=(KeyBoard) u;
            k.keyDown();
        }
        u.unconnect();

    }

}
```

测试
```java
public class Test {
    public static void main(String[] args) {
        //创建电脑对象
        Computer c=new Computer();
        //创建USB设备对象
        USB u=new Mouse("罗技鼠标");
        c.installUSB(u);

        USB k=new KeyBoard("华为键盘");
        c.installUSB(k);

    }
}

```




----
