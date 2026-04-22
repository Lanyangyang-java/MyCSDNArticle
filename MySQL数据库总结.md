@[TOC](MySQL数据库总结)

---
# 一、数据库相关概念
## 1.数据库
1. 存储数据的仓库，数据是有组织的进行存储
2.   英文：DataBase，简称 DB

---
## 2.数据库管理系统
1. 管理数据库的大型软件
2.  英文：DataBase Management System，简称 DBMS

---
## 3.SQL
1. 操作关系型数据库的编程语言
2. 定义操作所有关系型数据库的统一标准
3. 英文：Structured Query Language，简称 SQL，结构化查询语言
---
## 4.常见的关系型数据库管理系统
数据库名称|简介
-|-
Oracle|收费的大型数据库，Oracle 公司的产品
MySQL| 开源免费的中小型数据库。后来 Sun公司收购了 MySQL，而 Sun 公司又被 Oracle 收购
SQL Server|MicroSoft 公司收费的中型的数据库。C#、.net 等语言常使用
PostgreSQL|开源免费中小型的数据库
DB2|BM 公司的大型收费数据库产品
SQLite|嵌入式的微型数据库。如：作为 Android 内置数据库
MariaDB|开源免费中小型的数据库
---
# 二、MySQL 数据库
## 1.MySQL目录结构
![请添加图片描述](https://i-blog.csdnimg.cn/direct/72c0eb25ad08489e9724498588dd825f.png)

---
## 2.MySQL 数据模型
**关系型数据库：**
关系型数据库是建立在关系模型基础上的数据库，简单说，关系型数据库是由多张能互相连接的 二维表 组成的数据库
**优点**
1. 都是使用表结构，格式一致，易于维护。
2. 使用通用的 SQL 语言操作，使用方便，可用于复杂查询。
3. 数据存储在磁盘中，安全。


**数据模型**
![请添加图片描述](https://i-blog.csdnimg.cn/direct/684e6e3614834f9f818d8ade33949e9c.png)

---
# 三 、SQL
## 1.SQL简介
1. 结构化查询语言，一门操作关系型数据库的编程语言
2. 定义操作所有关系型数据库的统一标准
3. 对于同一个需求，每一种数据库操作的方式可能会存在一些不一样的地方，我们称为“方言”
4. 英文：Structured Query Language，简称 SQL
***
## 2.SQL 通用语法
1. SQL 语句可以单行或多行书写，以分号结尾。
2. MySQL 数据库的 SQL 语句不区分大小写，关键字建议使用大写。
3. 注释：
单行注释: -- 注释内容 或 #注释内容(MySQL 特有)
多行注释: /* 注释 */

---
## 3.SQL 分类
名称|作用
-|-
DDL(Data Definition Language) 数据定义语言|操作数据库，表等
DML(Data Manipulation Language) 数据操作语言|对表中的数据进行增删改
DQL(Data Query Language) 数据查询语言|对表中的数据进行查询
DCL(Data Control Language) 数据控制语言|对数据库进行权限控制

---
## 4.DDL（数据定义）
### 操作数据库
**1.查询**

```sql
SHOW DATABASES;
```
**2.创建**

创建数据库
```sql
CREATE DATABASE 数据库名称;
```
创建数据库(判断，如果不存在则创建)
```sql
CREATE DATABASE IF NOT EXISTS 数据库名称;
```
**3. 删除**

删除数据库
```sql
DROP DATABASE 数据库名称;
```

删除数据库(判断，如果存在则删除)
```sql
DROP DATABASE IF EXISTS 数据库名称;
```
**4.使用数据库**

查看当前使用的数据库
```sql
SELECT DATABASE();
```
使用数据库
```sql
USE 数据库名称;
```
---
### 操作表
**1.创建（Create）**

```sql
CREATE TABLE 表名 (
	字段名1 数据类型1,
	字段名2 数据类型2,
	…
	字段名n 数据类型n
);
#注意：最后一行末尾，不能加逗号
```

**2.查询（Retrieve）**

查询当前数据库下所有表名称
```sql
SHOW TABLES;
```
查询表结构
```sql
DESC 表名称;
```

**3.修改（ALTER）**
修改表名
```sql
ALTER TABLE 表名 RENAME TO 新的表名;
```
 添加一列

```sql
ALTER TABLE 表名 ADD 列名 数据类型;
```
修改数据类型
```sql
ALTER TABLE 表名 MODIFY 列名 新数据类型;
```
修改列名和数据类型
```sql
ALTER TABLE 表名 CHANGE 列名 新列名 新数据类型;
```
删除列
```sql
ALTER TABLE 表名 DROP 列名;
```
---

**4.删除（Delete）**
删除表
```sql
DROP TABLE 表名;
```

删除表时判断表是否存在
```sql
DROP TABLE IF EXISTS 表名;
```
---
### MySQL数据类型
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ce7d9c35f838453db729658f41141128.png)

doubel(总长度，小数点后面保留的位数)

char会用空格补齐空白空间   浪费空间   存储性能高
varchar(20)， 20为最长空间  节约空间   存储性能差

**常用**
int
double
date
char
varchar

---
## 5.DML（数据操作）
### 添加（insert）
给指定列添加数据
```sql
INSERT INTO 表名(列名1,列名2,…) VALUES(值1,值2,…);
```

给全部列添加数据
```sql
INSERT INTO 表名 VALUES(值1,值2,…);
```
批量添加数据
```sql
INSERT INTO 表名(列名1,列名2,…) VALUES(值1,值2,…),(值1,值2,…),(值1,值2,…)…;
INSERT INTO 表名 VALUES(值1,值2,…),(值1,值2,…),(值1,值2,…)…;
```
---
### 修改（update）
修改表数据
```sql
 UPDATE 表名 SET 列名1=值1,列名2=值2,… [WHERE 条件] ;
```
**注意：修改语句中如果不加条件，则将所有数据都修改！**

----
### 删除（delete）
删除数据
```sql
DELETE FROM 表名 [WHERE 条件] ;
```
**注意：删除语句中如果不加条件，则将所有数据都删除！**

---
## 6.DQL（数据查询）
### 基础查询
查询多个字段
```sql
SELECT 字段列表 FROM 表名;
SELECT * FROM 表名; -- 查询所有数据
```

去除重复记录
```sql
SELECT DISTINCT 字段列表 FROM 表名;
```
起别名
```sql
AS: AS 也可以省略
```
---
### 条件查询(WHERE）
条件查询语法
```sql
SELECT 字段列表 FROM 表名 WHERE 条件列表;
```
条件
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/851c38ed4e854d18a3389cc727796c21.png)

---
### 分组查询(GROUP BY) 
**1.聚合函数**
**概念：** 将一列数据作为一个整体，进行纵向计算

**聚合函数分类**
函数名|功能
-|-
count(列名)| 统计数量(一般选用不为null的列)
max(列名)|最大值
min(列名)| 最小值
sum(列名)|求和
avg(列名)|平均值
in(数据1，数据2,...)|查询某个范围内的数据。

**聚合函数语法**
```sql
SELECT 聚合函数名(列名) FROM 表;
```
**注意：null 值不参与所有聚合函数运算**

**2.分组查询语法**

```sql
SELECT 字段列表 FROM 表名 [WHERE 分组前条件限定] GROUP BY 分组字段名 [HAVING 分组后条件过滤];
```
**注意：分组之后，查询的字段为聚合函数和分组字段，查询其他字段无任何意义**


**where 和 having 区别**
1. 执行时机不一样：where 是分组之前进行限定，不满足where条件，则不参与分组，而having是分组之后对结果进行过滤
2. 可判断的条件不一样：where 不能对聚合函数进行判断，having 可以

**执行顺序： where > 聚合函数 > having**

---
### 排序查询(ORDER BY) 
排序查询语法
```sql
SELECT 字段列表 FROM 表名 ORDER BY 排序字段名1 [排序方式1],排序字段名2 [排序方式2] …;
```

**排序方式：**
**ASC：** 升序排列（默认值）
**DESC：** 降序排列

**注意：如果有多个排序条件，当前边的条件值一样时，才会根据第二条件进行排序**

---
### 分页查询(LIMIT)
分页查询语法
```sql
SELECT 字段列表 FROM 表名 LIMIT 起始索引 , 查询条目数;
```
起始索引：从0开始
计算公式：起始索引 = (当前页码-1) * 每页显示的条数

**tips：**
1. 分页查询 limit 是MySQL数据库的方言
2. Oracle 分页查询使用 rownumber
3. SQL Server分页查询使用 top



# 四、图形化客户端工具
**Navicat** 虽说市面上有很多数据库的图形化客户端工具 但还是十分推荐使用Navicat
个人感觉**Navicat** 十分简介 使用时方便快捷 页面看的十分舒适
官网： [http://www.navicat.com.cn](http://www.navicat.com.cn)
这套全面的前端工具为数据库管理、开发和维护提供了一款直观而强大的图形界面。
# 五、约束
## 1.概念和分类
**约束的概念**
1. 约束是作用于表中列上的规则，用于限制加入表的数据
2. 约束的存在保证了数据库中数据的正确性、有效性和完整性

**约束的分类**
约束名称|描述|关键字
--|--|--
非空约束|保证列中所有数据都不能有null|NOT NULL
唯一约束|保证列中所有数据各不相同|UNIQUE
主键约束|主键是一行数据的唯一标识，要求非空且唯一|PRIMARY KEY
默认约束|保存数据时，未指定值则采用默认值|DEFAULT
检查约束|保证列中的值满足某一条件|CHECK
外键约束|外键用来让两个表的数据之间建立链接，保证数据的一致性和完整性|FOREIGN KEY

**注意：MySQL不支持检查约束**

---
## 2.非空约束
**简介：**
非空约束用于保证列中所有数据不能有NULL值

**语法：**
 添加约束
 
创建表时添加非空约束
 ```sql
CREATE TABLE 表名(
列名 数据类型 NOT NULL,
…
);
```
建完表后添加非空约束
```sql
ALTER TABLE 表名 MODIFY 字段名 数据类型 NOT NULL;
```

删除约束
```sql
ALTER TABLE 表名 MODIFY 字段名 数据类型;
```
---
## 3.唯一约束
**简介**
唯一约束用于保证列中所有数据各不相同

添加约束

**语法**
```sql
-- 创建表时添加唯一约束
CREATE TABLE 表名(
列名 数据类型 UNIQUE [AUTO_INCREMENT],
-- AUTO_INCREMENT: 当不指定值时自动增长
…
);

CREATE TABLE 表名(
列名 数据类型,
…
[CONSTRAINT] [约束名称] UNIQUE(列名)
);

-- 建完表后添加唯一约束
ALTER TABLE 表名 MODIFY 字段名 数据类型 UNIQUE;

```

删除约束
```sql
ALTER TABLE 表名 DROP INDEX 字段名;
```
---
## 4.主键约束

**简介**
主键是一行数据的**唯一标识，**要求**非空且唯一**，**一张表只能有一个主键**

**语法：**
添加约束
```sql
-- 创建表时添加主键约束
CREATE TABLE 表名(
列名 数据类型 PRIMARY KEY [AUTO_INCREMENT],
…
);

CREATE TABLE 表名(
列名 数据类型,
[CONSTRAINT] [约束名称] PRIMARY KEY(列名)
);

-- 建完表后添加主键约束
ALTER TABLE 表名 ADD PRIMARY KEY(字段名);
```

删除约束
```sql
ALTER TABLE 表名 DROP PRIMARY KEY;
```
---
## 5.默认约束
**简介**
保存数据时，未指定值则采用默认值

**语法**
添加约束
```sql
-- 创建表时添加默认约束
CREATE TABLE 表名(
列名 数据类型 DEFAULT 默认值,
…
);

-- 建完表后添加默认约束
ALTER TABLE 表名 ALTER 列名 SET DEFAULT 默认值;
```

删除约束
```sql
ALTER TABLE 表名 ALTER 列名 DROP DEFAULT;
```
---
## 6.检查约束
**简介**
保证列中的值满足某一条件

**语法**
添加约束
```sql
CREATE TABLE 表名(
列名 数据类型,
…
CHECK(表达式),
);
```
修改约束
```sql
ALTER TABLE 表名 ADD CONSTRAINT 列名 CHECK(<检查约束>)
```

删除约束
```sql
ALTER TABLE 表名 DROP CONSTRAINT 约束名;
```
---
## 7.外键约束
**简介**
外键用来让两个表的数据之间建立链接，保证数据的一致性和完整性

**语法**
添加约束
```sql
-- 创建表时添加外键约束
CREATE TABLE 表名(
列名 数据类型,
…
[CONSTRAINT] [外键名称] FOREIGN KEY(外键列名) REFERENCES 主表(主表列名)
);
-- 建完表后添加外键约束
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);
```
删除约束
```sql
ALTER TABLE 表名 DROP FOREIGN KEY 外键名称; 
```
---
# 六、数据库设计
## 1.数据库设计简介
1. 数据库设计就是根据业务系统的具体需求，结合我们所选用的DBMS，为这个业务系统构造出最优的数据存储模型
2. 建立数据库中的表结构以及表与表之间的关联关系的过程。

**数据库设计的步骤**
1. 需求分析（数据是什么? 数据具有哪些属性? 数据与属性的特点是什么）
2. 逻辑分析（通过ER图对数据库进行逻辑建模，不需要考虑我们所选用的数据库管理系统）
3. 物理设计（根据数据库自身的特点把逻辑设计转换为物理设计）
4. 维护设计（1.对新的需求进行建表；2.表优化）

**表关系：**
1. 一对一 
2. 一对多(多对一）
3. 多对多

---
## 2.表关系之一对多

**例如：** 部门表和员工表，一个部门对应多个员工，一个员工对应一个部门
**实现方式：**在多的一方建立外键，指向一的一方的主键

![请添加图片描述](https://i-blog.csdnimg.cn/direct/eeb0850406b446b6a2b570e24f44fe9e.png)

---
## 3.表关系之多对多
**如：** 学生和课程，一个学生可以选择多个课程 一个课程也可以被多个学生选择
**实现方式**：建立第三张中间表，中间表至少包含两个外键，分别关联两方主键![请添加图片描述](https://i-blog.csdnimg.cn/direct/a5e2215971a24e239a27ac57bb01fac1.png)

---
## 4.表关系之一对一
**如：** 用户和用户详情，一对一关系用于表拆分，将一个实体中经常使用的字段放一张表，不经常使用的字段放另一张表，用于提升查询性能
**实现方式：** 在任意一方加入外键，关联另一方主键，并且设置外键为唯一(UNIQUE)

---
# 七、多表查询
## 1.多表查询简介
1. **笛卡尔积：** 取 A,B集合所有组合情况
2. **多表查询：** 从多张表查询数据
3. **连接查询：**
 内连接：相当于查询A B交集数据
 外连接：
	1. 左外连接：相当于查询A表所有数据和交集部分数据
	2. 右外连接：相当于查询B表所有数据和交集部分数据
4. **子查询：** 查询中嵌套查询，称嵌套查询为子查询
---
## 2.内连接
**内连接** 内连接相当于查询 A B 交集数据
**内连接查询语法：**
**隐式内连接：**
```sql
SELECT 字段列表 FROM 表1,表2… WHERE 条件;
```
**显示内连接:**
```sql
SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 条件;
```

## 3.外连接
**外连接查询语法：** 
**左外连接:** 相当于查询A表所有数据和交集部分数据
```sql
SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件;
```
 
**右外连接:**  相当于查询B表所有数据和交集部分数据
```sql
SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件;
```
 ---
## 4.子查询
**子查询概念：** 查询中嵌套查询，称嵌套查询为子查询
**子查询根据查询结果不同，作用不同：**
1. 单行单列
2. 多行单列
3. 多行多列


**单行单列：** 作为条件值，使用 = != > <等进行条件判断
```sql
SELECT 字段列表 FROM 表 WHERE 字段名 = (子查询);
```
**多行单列：** 作为条件值，使用 in 等关键字进行条件判断
```sql
SELECT 字段列表 FROM 表 WHERE 字段名 in (子查询);
```
**多行多列：**作为虚拟表
```sql
SELECT 字段列表 FROM (子查询) WHERE 条件;
```

## 5.多表查询分析步骤
 1. 分析数据分别来自于哪些表
 2. 分析这些表直接的关联关系
 3. 分析使用什么样的查询方式
---
# 八、事务
## 1.事务简介
1. 数据库的 **事务**（Transaction）是一种机制、一个操作序列，包含了一组数据库操作命令
2. 事务把所有的命令作为一个整体一起向系统提交或撤销操作请求，即这一组数据库命令要么同时成功，要么同时失败
3. 事务是一个不可分割的工作逻辑单元
## 2.事务操作
**开启事务**
```sql
START TRANSACTION;
BEGIN;
```
**提交事务**
```sql
COMMIT;
```
**回滚事务**
```sql
ROLLBACK;
```
## 3.事务四大特征(ACID)
**原子性（==A==tomicity**）: 事务是不可分割的最小操作单位，要么同时成功，要么同时失败
**一致性（==C==onsistency）** :事务完成时，必须使所有的数据都保持一致状态
**隔离性（==I==solation）** :多个事务之间，操作的可见性
**持久性（==D==urability）** :事务一旦提交或回滚，它对数据库中的数据的改变就是永久的


**MySQL事务默认自动提交:**
```sql
-- 查看事务的默认提交方式
SELECT @@autocommit;
-- 1 自动提交 0 手动提交
--修改事务提交方式
set @@autocommit = 0;
```
