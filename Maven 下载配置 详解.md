@[TOC](Maven 下载配置 详解 我的学习笔记)

---

# 一、Maven 简介
>Apache Maven 是一个项目管理和构建工具，它基于项目对象模型(POM)的概念，通过一小段描述信息来管理项目的构建、报告和文档
>官网：[http://maven.apache.org/](http://maven.apache.org/)

---
 **功能**
 
**Maven是专门用于管理和构建Java项目的工具，它的主要功能**
>**提供了一套标准化的项目结构
提供了一套标准化的构建流程（编译，测试，打包，发布……）
提供了一套依赖管理机制**

**标准化的项目结构**
>**Maven提供了一套标准化的项目结构，所有IDE使用Maven构建的项目结构完全一样，所有IDE创建的Maven项目可以通用**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/cedea008e3234b56afb69f2caf8ec077.png)

---
**标准化的构建流程**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/208f3f2cfd3c4aed84ff7f82a6e58945.png)
>Maven提供了一套简单的命令来完成项目构建

---
依赖管理机制**

**依赖管理其实就是管理你项目所依赖的第三方资源 (jar包、插件…)**
>Maven 使用标准的坐标配置来管理各种依赖
只需要简单的配置就可以完成依赖管理

**例如**
```xml
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.32</version>
    </dependency>
```
---
 **作用**
1. 标准化的项目结构
2. 标准化的构建流程
3. 方便的依赖管理


---
**Maven 模型**
1. 项目对象模型 (Project Object Model)
2. 依赖管理模型(Dependency)
3. 插件(Plugin)

 
 ---
 **Maven 仓库**
 
 1. 仓库分类：
 >**本地仓库：自己计算机上的一个目录
   中央仓库：由Maven团队维护的全球唯一的仓库--------地址：https://repo1.maven.org/maven2/
   远程仓库(私服)：一般由公司团队搭建的私有仓库**

2. 查找依赖对应jar包
>当项目中使用坐标引入对应依赖jar包后，首先会查找本地仓库中是否有对应的jar包
>如果有，则在项目直接引用，如果没有，则去中央仓库中下载对应的jar包到本地仓库
>还可以搭建远程仓库，将来jar包的查找顺序则变为 **本地仓库 => 远程仓库 => 中央仓库**

---
# 二、maven安装配置
**下载**
>**maven下载官网: [https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)**


![请添加图片描述](https://i-blog.csdnimg.cn/direct/72fcc5583651470780d50bced7f41c07.png)
>**解压zip及完成安装**


**配置环境变量**
>**高级系统设置=>环境变量=>系统变量=>新建=>配置MAVEN_HOME=>确定=>Path=>新建=>`%MAVEN_HOME%\bin`**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1a1728efb2f84fef8b081e4b62b23205.png)

**创建本地仓库**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1189f135ccd4405b9663a7456b4db9f2.png)

**配置本地仓库**
>**修改 conf/settings.xml 中的 `<localRepository> `为一个指定目录**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5f87b2ba1bc04d99987d8e016710a89b.png)
**配置阿里云镜像**

>**修改 conf/settings.xml 中的 `<mirrors>`标签**
>
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8f3fa27cdcbb4374a4e5aa6d0964c563.png)


---
**代码如下**
```xml
	<mirror>
		<id>alimaven</id>
		<mirrorOf>central</mirrorOf>
		<name>aliyun maven</name>
		<url>https://maven.aliyun.com/repository/central</url>
	</mirror>
	
    <mirror>
		<id>alimaven</id>
		<mirrorOf>public</mirrorOf>
		<name>aliyun maven</name>
		<url>https://maven.aliyun.com/repository/public</url>
	</mirror>
		
    <mirror>
		<id>alimaven</id>
		<mirrorOf>gradle-plugin</mirrorOf>
		<name>aliyun maven</name>
		<url>https://maven.aliyun.com/repository/gradle-plugin</url>
	</mirror>
		
    <mirror>
		<id>alimaven</id>
		<mirrorOf>apache snapshots</mirrorOf>
		<name>aliyun maven</name>
		<url>https://maven.aliyun.com/repository/apache-snapshots</url>
	</mirror>
	
	<mirrors>
		<mirror>
			<id>central</id>
			<name>Maven Repository Switchboard</name>
			<url>https://repo1.maven.org/maven2/</url>
			<mirrorOf>central</mirrorOf>
		</mirror>
	</mirrors>
```
>**maven安装及配置完成**
# 三、maven基本使用
**Maven 常用命令**
命令名称|作用
-|-
compile|编译
clean|清理
test|测试
package|打包
install|安装

**快捷命令**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/9706c70afa314d54aa69f63f948c62b0.png)

---
**Maven 生命周期**
>**Maven 构建项目生命周期描述的是一次构建过程经历经历了多少个事件**

**Maven 对项目构建的生命周期划分为3套**
>**同一生命周期内，执行后边的命令，前边的所有命令会自动执行**
1. clean：清理工作
2. default：核心工作，例如编译，测试，打包，安装等
3. site：产生报告，发布站点等

**default 构建生命周期**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/aafa817bc20f4fecaa597ab98641bd58.png =800x)

---
# 四、idea配置maven
## idea配置maven环境
>**File =>Settings=>Build...**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d6082619a9584dea98ac68b9530dc8d8.png)


## maven坐标
>**Maven 中的坐标是资源的唯一标识，使用坐标来定义项目或引入项目中需要的依赖**

**Maven 坐标主要组成**

>groupId：定义当前Maven项目隶属组织名称（通常是域名反写，例如：com.chq）
artifactId：定义当前Maven项目名称（通常是模块名称，例如 order-service、goods-service）
version：定义当前项目版本号

```xml
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.32</version>
    </dependency>
```
## idea创建maven项目
**1. 创建模块，选择Maven**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/bc9b368773174af48c2a37af4377f4cb.png =600x)


**2. 填写模块名称，坐标信息**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e12a223c60dd463fbd656a30276b6013.png =600x)

**3. 点击finish，创建完成**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/03f046a048404e42b4b32acb6ae5f100.png =600x)


## 配置Maven-Helper插件
>**File=>Settings=>Plugins=>搜索Maven=>选择Maven Helper=>Install=>重启Idea**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/721ce623df364c22947ee287f3915016.png =600x)
**使用**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f780ea57e65f49028e7b49e5dd2132bc.png =600x)




# 五、依赖管理
**使用坐标导入 jar 包**
1. 在 pom.xml 中编写 `<dependencies> `标签
2. 在 `<dependencies> `标签中 使用` <dependency> `引入坐标
3. 定义坐标的 `groupId  artifactId version`
4. 点击刷新按钮，使坐标生效

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a076f8e3c65d44cb9784d20ec46c3a23.png =300x)

**依赖范围**
>**通过设置坐标的依赖范围(scope)，可以设置 对应jar包的作用范围：编译环境、测试环境、运行环境**
>**`<scope>`默认值：compile**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/83dc8980f4864324a90e8132bf9bfcec.png)

依赖范围|编译|测试|运行|例子
-|-|-|-|-
compile|T|T|T|logback
test|F|T|F|junit
provided|T|T|F|servlet-api
runtime|F|T|T|jdbc
system|T|T|F|储存在本地的jar包

>**import: 引入DependecyManagement**



