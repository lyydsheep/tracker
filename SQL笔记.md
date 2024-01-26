# 数据库学习（1）

## 一、数据库和SQL

将大量<u>数据</u>保存起来，通过计算机加工而成的可以进行高效访问的数据集合称为<u>数据库</u>。

数据库管理系统（DBMS）具有==提供操作大量数据所需格式、操作简单、能应对突发事件==等优点，

### 1.1 数据库的结构

> RDAMS常见系统结构

使用RDAMS时，最常见的系统结构就是客户端/服务器类型（C/S类型）

![image-20240114202444372](https://gitee.com/lyydsheep/pic/raw/master/202401142024481.png)

> 表的结构

在RDB中用以管理数据的行列二维表结构成为表，==一个数据库中可以有多个表==。

服务端依据SQL语句读取的数据必须以二维表的形式返回给客户端。

在一个表中，列称为字段，表明数据项目；行称为记录，相当于一条数据，在读写数据库时，==必须以行为单位进行操作==。

![image-20240114205714599](https://gitee.com/lyydsheep/pic/raw/master/202401142057676.png)

行列交错形成一个个的单元格，==一个单元格内只能输入一个数据。==

### 1.2 SQL概要

> SQL语句及其种类

SQL语句是由关键字、表明、字段组合而成的一条语句。

SQL语句可分为三大类：

- DDL（Data Define Language）
  - CREATE：创建数据库和表等对象
  - DROP：删除数据库和表等对象
  - ALTER：修改数据库和表的结构
- DML（Data Manipulation Language）
  - SELECT：查询表中的数据
  - INSERT：向表中插入数据
  - UPDATE：更新表中的数据
  - DELETE：删除表中的数据
- DCL（Data Control Language）
  - COMMIT：确认对数据库中的数据进行变更
  - ROLLABCK：取消对数据库中的数据进行变更
  - GRANT：赋予用户操作权限
  - REVOKE：取消用户操作权限

> SQL基本书写规则

1. 以;结尾
2. SQL语句不区分大小写
   1. 关键字大写
   2. 表明首字母大写
   3. 其余小写

> 数据库的创建

```sql
CREATE DATABASE <数据库名称>;
```

> 表的创建

```SQL
CREATE TABLE <表名>
(<列名1> <数据类型> <该列的约束>,
<列名2> <数据类型> <该列的约束>,
...
<该表的约束1> <该表的约束2>);
```

> 命名规则

- 数据库名称、表名称和列名等可以使用下列三种字符：
  - 半角英文
  - 半角数字
  - 下划线_
- 名称必须以半角英文字母作为开头（非强制）
- 名称不能重复

> 变量类型

- INTEGER
  - 用来指定存储整数的列的数据类型，不能存储小数
- CHAR
  - 用指定存储字符串的列的数据类型，CHAR(10)限定字符串长度为10，不足时用半角空格补全
  - 是定长字符串
- VARCHAR
  - 用来指定存储字符串的列的数据类型，VARCHAR(10)限定字符串长度为10
  - 变长字符串，不足时不进行补全
- DATE
  - 用来指定存储日期的列的数据类型

> 约束的设置

在数据类型后面'NOT NULL'是对该字段的约束，表明该字段的数据不能为空；

![image-20240115212332706](https://gitee.com/lyydsheep/pic/raw/master/202401152123816.png)

‘PRIMARY KEY'是给product_id设置的主键约束，表明product_id是可以唯一确定行的字段，即该字段数据不允许重复。

### 1.3 表的删除和更新

> 表的删除（DROP TABLE)

```sql
DROP TABLE <表名>;
#对表进行删除，并且删除后无法恢复！
```

> 表的更新(ALTER TABLE)

```SQL
ALTER TABLE <表名> ADD COLUMN <列的定义>;#添加列
ALTER TABLE <表名> DROP COLUMN <列名>;#删除列
ALTER TABLE <表名> MODIFY COLUMN <列名> data_type;#修改字段数据类型
RENAME TABLE <旧表名> TO <新表名>;
```

> 向表中插入数据

```sql
START TRANSACTION;#开始插入行语句
INSERT INTO <表名> VALUES (XXX, XXX, XXX);
COMMIT;#确定插入行语句
```

## 二、查询基础

### 2.1 SELECT语句基础

> ==列的查询==

从表中选取数据时需要使用SELECT语句，通过SELECT语句查询并选取出必要的数据称为**匹配查询**或**查询**。

```sql
SELECT <列名1>, <列名2> FROM <表名>;
```

==查询结果中字段的顺序和SELCET子句中字段顺序相同==

> ==查询表中所有的列==

```sql
SELECT *
	FROM <表名>;# *表示所有的列
```

> ==为列设置别名==

通过AS关键字可以为列设置别名

```SQL
SELECT <列名> AS <别名>
	FROM <表名>;
```

别名不仅可以设置为英文，还能设置为中文，但==使用中文时需要用<u>双引号</u>括起来==

![image-20240116115633330](https://gitee.com/lyydsheep/pic/raw/master/202401161159122.png)

> ==常数的查询==

SELECT子句中不仅可以书写列名，还可以书写常数

![image-20240116145110166](https://gitee.com/lyydsheep/pic/raw/master/202401161451204.png)

> ==从结果中删除重复行（数据）==

在第一个字段前添加关键字DISTINCT可以删除重复行

```sql
SELECT DISTINCT <字段1>, <字段2>, <字段3>
	FROM <表名>;
```

![image-20240116145415608](https://gitee.com/lyydsheep/pic/raw/master/202401161454634.png)

![image-20240116145655410](https://gitee.com/lyydsheep/pic/raw/master/202401161456445.png)

在使用DISTINCT删除重复行时，NULL被视为同一类数据，如果字段中有多个NULL，最终会保留一个NULL。

> ==根据WHERE语句来选择记录==

SELECT语句通过WHERE子句限定查询条件，WHERE语句后面紧跟着条件表达式。执行SQL语句时，首先通过WHERE子句筛选出符合指定条件的记录，然后在选出SELECT语句指定的列。

![image-20240116150903888](https://gitee.com/lyydsheep/pic/raw/master/202401161509923.png)

SQL中子句的顺序是固定的，不能随意更改，WHERE子句要紧跟在FROM子句后面。

### 2.2算术运算符和比较运算符

> ==算术运算符==

SELECT子句中不仅可以进行常数查询还能使用运算表达式，并且运算符是以行为单位执行的

![image-20240116151911910](https://gitee.com/lyydsheep/pic/raw/master/202401161519937.png)

> ==需要注意NULL==

所有包含NULL的计算，结果肯定是NULL

> ==比较运算符==

```sql
=	和~相等
<>	和~不相等
>	大于~
<	小于~
>=	大于等于~
<=	小于等于~
```

定长/边长字符串按照==<b>字典序</b>==进行排序比较，不能与字面值的大小顺序混淆

> ==不能对NULL使用比较运算符==

要查询某字段数据（不）为NULL的记录时，应使用==IS (NOT) NULL==运算符

![image-20240116154529434](https://gitee.com/lyydsheep/pic/raw/master/202401161545471.png)

### 2.3逻辑运算符

> ==NOT运算符==

NOT运算符不能单独使用，必须和其他条件组合起来使用，但是不能滥用。

> ==AND运算符和OR运算符==

在WHERE子句中使用AND运算符和OR运算符可以实现多个条件查询的组合，从而实现在多重条件下的查询。

AND运算符的优先级高于OR运算符，欲使优先执行OR运算符，可以通过括号强化优先级。	

> ==含有NULL时的真值==

当NULL值充当比较运算符的操作数时，结果既不是TRUE也不是FALSE，而是UNKNOW。在SQL中，逻辑不是二值，而是三值：TRUE、FALSE、UNKNOW，因此任何（比较运算符）条件查询语句会将无法对NULL值选择。

NULL，只能用<b>IS (NOT) NULL</b>进行判断。

## 三、聚合与排序

### 3.1对目标进行聚合查询

> ==聚合函数==

用于汇总的函数就是聚合函数。聚合函数能将多行汇总成一行，即多个输入一个输出。

```SQL
COUNT:	计算表中记录数
SUM:	计算表中各字段数值之和
AVG:	计算表中各字段数值平均值
MAX:	计算表中各字段数值最大值
MIN:	计算表中各字段数值最小值
```

> ==计算表中的行数==

使用count()函数对表中行数进行统计

```SQL
SELECT
	COUNT(参数)
FROM 
	<表名>;
```

COUNT()函数的参数可以是*（表示全部列），也可以是具体的某一字段。参数列不同计算的结果也可能不同。

![image-20240117145021793](https://gitee.com/lyydsheep/pic/raw/master/202401171450899.png)

当参数列为*（所有列）时，会统计包括NULL的记录；当参数列为具体字段时，只统计非NULL的记录。

> ==SUM、AVG、MIN、MAX==

SUM和AVG函数只适用于数值类型，MIN和MAX函数几乎适用于所有的数值类型。

所有的聚合函数都会排除NULL的记录，除了COUNT(*)。

> ==使用聚合函数删除重复值(DISTINCT)==

在聚合函数的参数列中使用DISTINCT关键字，可以删除重复数据

![image-20240117150913165](https://gitee.com/lyydsheep/pic/raw/master/202401171509197.png)

### 3.2对表进行分组

> GROUP BY子句

GROUP BY子句根据相应的字段对表进行切分，且NULL值也为一类（不确定类），具有相同数值的作为一类，共有一个数值。

GROUP BY子句依据的字段称为聚合键

```SQL
SELECT <字段1>, <字段2>
FROM <表名>
WHERE <条件查询>
GROUP BY <聚合键1>, <聚合键2>;
```

执行顺序：

WHERE ---> GROUP BY ---> FROM ---> SELECT

> ==聚合函数和GROUP BY子句有关的常见错误==

1. SELECT子句中书写了多余的列：<u>SELECT子句不能书写除聚合键之外的字段</u>，因为聚合键和其他字段值并非是一对一的关系
2. GROUP BY子句中用了列的别名：根据SQL语句的执行顺序可知，GROUP BY在SELECT执行之前，因此<u>不能使用SELECT子句中列的别名</u>
3. GROUP BY子句能排序吗：使用GROUP BY呈现出来的结果是<u>无序的</u>
4. 在WHERE子句中使用聚合函数：只有SELECT子句和HAVING子句中才能使用聚合函数

### 3.3为聚合结果指定条件

> ==HAVING子句==

​	WHERE子句只能用来指定行条件，要想对组指定条件，就需要使用HAVING子句

```sql
SELECT <列名1>, <列名2>
FROM <表名>
GROUP BY <聚合键>
HAVING <分组结果对应的条件>;
```



与WHERE子句相反，HAVING子句最后执行。

> ==HAVING子句的构成要素==

HAVING子句中能够使用的要素有：

- 常数
- 聚合函数
- 聚合键

> ==更适合写在WHERE子句中的条件==

对于GROUP BY子句中聚合键的约束条件，既可以写在WHERE子句中，也可以写在HAVING子句中，但其更适合写在WHERE子句中

- WHERE子句和HAVING子句作用不同：WHERE指定行所对应的条件，单位是行（记录）；HAVING子句指定组所对应的条件，单位是组
- 执行速度不同

### 3.4对查询结果进行排序

> ORDER BY子句

在SELECT子句末尾添加ORDER BY子句明确排序顺序

若有多个排序键，则从左至右依次起效，如果排序键中数值为null，那么为null的记录要么在最前，要么在最后

```sql
SELECT <列名>
FROM <表名>
ORDER BY <排序键1>, <排序键2> ASC(DESC)
```

目前为止，子句的书写顺序

SELECT  -->  FROM  -->  WHERE  -->  GROUP BY  -->  HAVING  -->  ORDER BY;

执行顺序：

FROM  -->  WHERE  -->  GROUP BY  -->  HAVING  -->  SELECT  -->  ORDER BY

从执行顺序中可以看出，HAVING子句和GROUP BY子句中不能使用SELECT子句中的别名，而ORDER BY子句中可以使用SELECT子句中的别名

> ==ORDER BY子句中可以使用的字段==

ORDER BY子句中可以使用SELECT子句中未出现的字段，也可以使用聚合函数

## 四、数据更新

### 4.1数据的插入

> INSERT语句基本语法

```sql
INSERT INTO <表名> (列名1， 列名2， 列名3...) VALUES (值1，值2，值3...)
```

列清单必须和值清单一致，否则插入失败

当对表进行全列插入时，列清单可以省略

不仅是INSERT，DELETE和UPDATE等更新语句执行失败时都不会对表中数据造成影响

> 插入默认值

如果表中字段具有DEFAULT约束，那么可以插入默认值

- 显式插入

![image-20240122201044740](https://gitee.com/lyydsheep/pic/raw/master/202401222010153.png)

- 隐式插入：省略被DEFAULT约束的字段即可

在列清单中，被省略的字段如果==有DEFAULT约束的字段设定为该列的默认值==，没有DEFAULT约束则设定为==NULL==

> 从其他表中复制数据

通过使用INSERT语句中的SELECT语句实现从其他表中复制数据

SELECT语句将从其他表中选取出指定数据，这些数据通过INSERT语句插入到目标表中

```SQL
INSERT INTO <目标表> (<字段1>, <字段2>, <字段3>) SELECT <字段1>, <字段2>, <字段3> FROM <源表> WHERE <查询条件> GROUP BY <关键码>
```

INSERT语句中的SELECT语句可以使用WHERE子句或者GROUP BY子句等任何SQL语法（但使用ORDER BY子句不会产生任何效果）

==注意：INSERT语句中使用SELECT语句时，不需要VALUES关键字，但是需要字段清单，而SELECT语句中不需要清单==

### 4.2数据的删除

> DROP TABLE语句和DELETE语句

```sql
DROP TABLE <表名>; #删除表
```

```sql
DELETE FROM <表名> #删除表中所有的记录，但保留容器
```

DELETE语句删除对象是==记录（数据行）==

> 搜索型DELETE语句（指定删除部分数据）

在DELETE语句中使用WHERE子句指定删除数据的条件，达到部分删除的目的

```SQL
DELETE FROM <表名> WHERE <条件>;
```

![](https://gitee.com/lyydsheep/pic/raw/master/202401251600030.png)

![](https://gitee.com/lyydsheep/pic/raw/master/202401251557535.png)

### 4.3数据的更新

> UPDATE语句的基本语法

```sql
UPDATE <表名> SET <字段> = expression
```

UPDATE语句也是一条DML语句，对数据进行更新操作

![image-20240125160834532](https://gitee.com/lyydsheep/pic/raw/master/202401251608650.png)

> 指定条件的UPDATE语句

UPDATE语句也可以向DELETE语句一样指定条件，对限定的记录进行更新

```SQL
UPDATE <表名> SET <字段> = expression WHERE <条件>
```

![image-20240125161334527](https://gitee.com/lyydsheep/pic/raw/master/202401251613642.png)

> 将列值设置为NULL

UPDATE语句中可以将没有NOT NULL约束的字段SET为NULL

![image-20240125173221141](https://gitee.com/lyydsheep/pic/raw/master/202401251732297.png)

> 多列更新

UPDATE语句进行多列更新有两种写法：

第一种方法是用逗号隔开每一个列等值

第二种方法则是采用列、值清单

```SQL 
UPDATE <表名> SET <字段1> = EXP1, <字段2> = EXP2, <字段3> = EXP3;

UPDATE <表名> SET (列清单) = (值清单);
```

![image-20240125174145350](https://gitee.com/lyydsheep/pic/raw/master/202401251741467.png)

### 4.4事务

事务就是需要在同一个处理单元内执行的DML语句的集合。

创建事务只需将待执行的DML语句放在事务开始和结束语句之间即可

```SQL
START TRANSACTION;
DML1;
DML2;
DML3;
COMMIT/ROLLBACK;
```

COMMIT	---	提交处理

ROLLBACK	---	取消处理

> ACID特性

Atomicity 原子性：原子性是指事务结束后，其中的DML语句要么全被执行，要么都不执行， DBMS肯定不会只执行其中的若干条。==注意，执行不代表成功==

Consistency 一致性：事务中的处理需要满足数据库提前设置的约束（例如NOT NULL等）

Isolation 隔离性：事务之间互不干扰，在一个事务结束（COMMIT/ROLLBACK）前，其他事务看不到其作用

Durability 持久性：也称耐久性，指一个事务结束后，DBMS能保证该时间点的数据状态被保存的特性

## 五、视图

视图：从SOL角度看，视图就是一张表

视图的优点：

- 视图无需保存数据：表中存储的是实际数据，而视图中保存的是从表中取出数据的SELECT语句
- 可以将频繁使用的SELECT语句保存成视图

> 创建视图

```SQL
CREATE VIEW name(<视图列名1>, <视图列名2>, <视图列名3>) AS <SELECT语句>
```

由于视图保存的就是SELECT语句，因此视图列名要和SELECT语句中的字段名一一对应
