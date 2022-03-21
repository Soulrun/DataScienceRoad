# SQL语法
> 这是一篇教程的感悟 Based on https://www.runoob.com/sql/sql-tutorial.html
>

## SQL 简介

Structured Query Language 结构化查询语言，是用于管理关系数据库管理系统（RDBMS)

SQL 范围包括数据插入、查询、更新和删除，数据库模式创建和修改，以及数据访问控制。


## SQL 语法 MySQL 环境

```sql
use RUNOOB; // 选择RUNOOB数据库
set names utf8; // 设置使用的字符集
SELECT * FROM websites; // 读取websites表的数据 
```

某些数据库系统要求在每条 SQL 语句的末端使用封号，是风格每条 SQL 语句的标准方法。

```sql
SELECT // 提取数据
UPDATE // 更新数据
DELETE // 删除数据
INSERT INTO // 插入新数据
CREATE DATABASE // 创建新数据库
ALTER DATABASE // 修改数据库
DROP TABLE // 删除表
DROP INDEX // 删除索引
```

## SELECT DISTINCT

用于返回唯一不同的值

```sql
SELECT DISTINCT country FROM Websites; // 选取唯一不同值
```

## WHERE

WHERE 子句用于提取那些满足指定条件的记录

```sql
SELECT * FROM Websites WHERE country = 'CN'; 
// 选取国家为CN的结果，文本字段用单引号
```

**WHERE 子句中的运算符有 BETWEEN LIKE IN**


## AND & OR 运算符

用于基于一个以上的条件对记录进行过滤

```sql
SELECT * FROM Websites WHERE coutry = 'CN' AND alexa > 50;
// 选取国家为CN且alexa大于50的结果
```

## ORDER BY 关键字

用于对结果集进行排序

```sql
SELECT * FROM Websites ORDER BY alexa;
// 升序排列所有结果
SELECT * FROM Websites ORDER BY alexa DESC;
// 降序排列所有结果
```

## INSERT INTO

```sql
INSERT INTO Websites (name, url, alexa)
VALULES ('百度','baidu.com','3');
```

## UPDATE 语句

用于更新表中已存在的记录

```sql
UPDATE Websites
SET alexa = '50', country = 'USA'
WHERE name = 'Google'
```

## LIMIT 

规定返回记录的数目，不同数据库有自己的写法 MySQL 适用LIMIT

```sql
SELECT * FROM Websites LIMIT 5;
```

## LIKE 操作符

用于**在WHERE子句中**搜索列中指定模式

```sql
SELECT * FROM Websites
WHERE name LIKE 'G%'
// 选取name中以G开头的结果
```

其中'G%'是[通配符](siyuan://blocks/20220308114250-enzm59h)

## IN 操作符

允许在**WHERE子句**中规定多个值

```sql
SELECT * FROM Websites
WHERE name IN ('Google','百度');
```

## BETWEEN 操作符

用于选取介于两个字之间的数据范围内的值

```sql
SELECT * FROM Websites
WHERE alexa BETWEEN 5 AND 50;

SELECT * FROM Websites
WHERE (alexa BETWEEN 5 AND 50) AND country NOT IN ('USA','IND')
// 选取alexa值在5到50之间且国家不是USA和IND的结果
```

## SQL别名

使用SQL可以为表名称或列名称指定别名，目的是让列名称的可读性更强

```sql
SELECT NAME AS NEW_NAME
FROM DATABASE;

SELECT name, CONCAT(url,',',alexa,',',country) AS site_info
FROM Websites;
```

## SQL 链接 JOIN

JOIN子句用于把来自两个或多个的表的行结合起来，基于这些表之间的共同字段。

INNER JOIN：如果表中有至少一个匹配，则返回行  
LEFT JOIN：即使右表中没有匹配，也从左表返回所有的行  
RIGHT JOIN：即使左表中没有匹配，也从右表返回所有的行  
FULL JOIN：只要其中一个表中存在匹配，则返回行


> 在使用LEFT JOIN时，ON和WHERE条件的区别
>
> 数据库在通过连接两张或多张表来返回记录时，都会生成一张中间的临时表，然后再将这张临时表返回给用户
>
> * ON条件是在生成临时表时使用的条件，不管ON中的条件是否为真，都会返回左边表中的记录。
> * WHERE条件是在临时表生成好后，再对临时表进行过滤的条件。这时候已经没有LEFT JOIN的含义了，条件不为真的就全部过滤掉
>
>   SEE https://www.runoob.com/w3cnote/sql-join-the-different-of-on-and-where.html
>
>   这些区别的原因是不同JOIN的特殊性，INNER JOIN没有这个特殊性
>

## UNION 操作符

合并两个或多个SELECT语句的结果，每个SELECT语句必须拥有相同数量的列和数据类型，UNION只会选取不同的值

```sql
SELECT country FROM Websites
UNION
SELECT country FROM apps
ORDER BY country;
// 选取不同的country的值

SELECT country FROM Websites
UNION ALL
SELECT country FROM apps
ORDER BY country;
```
## SELECT INTO语句
使得数据从一个表复制并插入到另一个新表中
> MySQL不支持SELECT ...INTO 语句，但支持INSERT INTO ... SELECT
> 或者使用 CREATE TABLE ... AS ... SELECT * FROM

``` sql 
    SELECT * 
    INTO NEW_TABLE
    FROM COPIED_TABLE;
```
## INSERT INTO SELECT 语句
同样的将一个表的数据复制并插入到一个已存在的表中
``` SQL
INSERT INTO NEW_TABLE
SELECT * FROM COPIED_TABLE;
或者我们可以只复制选定的列插入到另一个表中
INSERT INTO NEW_TABLE(COLUME1,COLUME2)
SELECT COLUME1,COLUME2 FROM COPIED_TABLE;
```

## CREATE DATABASE 语句
用于创建数据库
``` SQL
CREATE DATABASE NEW_DB;
```

## CREATE TABLE 语句
用于创建数据库中的表
``` SQL
CREATE TABLE NEW_TABLE
(
    COLUMN_NAME1 DATA_TYPE(SIZE),
    COLUMN_NAME2 DATA_TYPE(SIZE),
    ....
);

比如创建一个名为'person'的表，包含三列:ID,Name,City
ID为纯数字，可为整型 int; Name、city为字符，可为varchar，即

CREATE TABLE PERSON
(
    ID INT,
    NAME VARCHAR(255),
    CITY VARCHAR(255)
);
```

## SQL CONSTRAINTS SQL约束
用于规定表中的数据规则，可在创建表时规定（通过CREATE TABLE语句），或者之后规定(通过ALTER TABLE语句)

```SQL
CREATE TABLE TABLE_NAME
(
    COLUMN_NAME1 DATA_TYPE(SIZE) CONSTRAINT_NAME,
    COLUMN_NAME2 DATA_TYPE(SIZE) CONSTRAINT_NAME,
    ...
);

SQL中约束有：
- NOT NULL //不能存储NULL值，即必填
- UNIQUE //每一行必须有唯一值
- PRIMARY KEY // NOT NULL & UNIQUE 的结合，确保这列有唯一表示
- FOREIGN KEY //保证一个表中的数
- CHECK // 保证列中的值符合指定的条件
- DEFAULT // 规定没有给列赋值时的默认值
```

### NOT NUL

```sql
ALTER TABLE PERSON
MODIFY AGE INT NOT NULL;

在一个已创建的表'AGE'字段中添加NOT NULL约束

ALTER TABLE PERSON 
MODIFY AGE INT NULL;
删除已创建表'AGE'字段的NOT NULL约束
```

### FOREIGN KEY 

```SQL
CREATE TABLE PERSON
(
    ID INT NOT NULL,
    NAME VARCHAR(255),
    TYPE INT NOT NULL,
    PRIMARY KEY(ID),
    FOREIGN KEY (TYPE) REFERENCES ORDER(TYPE)
)

ALTER TABLE ORDER
DROP FOREIGN KEY fk_TYPE;
撤销外键约束
```
### CHECK
```SQL
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CHECK (P_Id>0)
)


ALTER TABLE PERSONS
ADD CHECK (P_Id <0)

ALTER TABLE PERSONS
DROP CONSTRAINT chk_PERSON
```

## CREATE INDEX 语句

用于在表中创建索引
在不读取整个表的情况下，索引使得数据库应用程序可以更快地查找数据

``` sql
CREATE INDEX INDEX_NAME
ON TABLE_NAME(COLUMN_NAME);

DROP INDEX TABLE_NAME.INDEX_NAME
删除索引

DROP TABLE TABLE_NAME

DROP DATABASE DATABASE_NAME

TRUNCATE TABLE TABLE_NAME
仅仅删除表内数据不删除表本身
```

## ALTER TABLE
用于在已有的表中添加、删除或修改列。
``` SQL
添加列 - ADD
ALTER TABLE TABLE_NAME
ADD COLUMN_NAME DATATYPE;

删除列 - DROP
ALTER TABLE TABLE_NAME
DROP COLUMN COLUMN_NAME

改变列数据类型 - MODIFY
ALTER TABLE TABLE_NAME
MODIFY COLUMN COLUMN_NAME DATATYPE
```

## AUTO INCREMENT 字段
可以在新纪录插入表中时生成一个唯一的数字

``` SQL
CREATE TABLE PERSON
(
    ID INT NOT NULL AUTO_INCREMENT,
    NAME VARCHAR(255),
    CITY VARCHAR(255),
    PRIMARY KEY(ID)
);
将person表中的id列定义为auto-increment主键字段
```
  在**SQL SERVER**中的语法用IDENTITY 关键字来执行AUTO-INCREMENT任务
```SQL 
CREATE TABLE Persons
(
ID int IDENTITY(1,1) PRIMARY KEY,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
其中IDENTITY(1,1)代表以1开始，递增1
```
## SQL views 视图
视图是可视化的表,隐藏了底层的表结构，简化了数据访问操作；加强了安全性，使得用户只看到了VIEW所显示的数据
``` SQL
CREATE VIEW VIEW_NAME AS 
SELECT COLUMN_NAME
FROM TABLE_NAME
WHERE CONDITION;
```
查询现在正在使用的产品语句
```sql
CREATE VIEW [CURRENT PRODUCT LIST] AS 
SELECT PRODUCTI_ID, PRODUCTNAME
FROM PRODUCTS
WHERE DISCONTINUED = NO
```
更新视图 CREATE OR REPLACE VIEW 语法
``` sql
CRAETE OR REPLACE VIEW VIEW_NAME AS 
SELECT COLUMN_NAME
FROM TABLE_NAME
WHERE CONDITION
```
## DATE 函数
SQL内建日期函数
> MYSQL
> - NOW( ) 返回当前的日期和时间
> - CURDATE( ) 返回当前的日期
> - CURTIME( ) 返回当前的时间
> - DATE( ) 提取日期或时间表达式的日期部分
> - EXTRACT( ) 返回日期时间的单独部分
> - DATE_ADD( ) 向日期添加指定的时间间隔
> - DATE_SUB( ) 从日期减去指定的时间间隔
> - DATEDIFF( ) 返回两个日期之间的天数
> - DATE_FORMATE( ) 用不同的格式显示日期时间

## NULL值
代表遗漏的未知数据
>工作中不使用NULL的原因：
> - 不利于代码的可读性和可维护性
> - 存在NULL值，会影响计数(COUNT)和不等(!=)查询、统计、运算结果

### NULL函数
MYSQL  - **IFNULL()**
``` SQL
SELECT PNAME,PRICE* (STOCK + IFNULL(ORDER,0))
FROM PRODUCT
如果ORDER是NULL，则返回0
```
微软同样功能的函数为**ISNULL( )**

ORACLE的相应函数为**NVL( )**

## SQL 通用数据类型
数据库表中的每个列都要求有名称和数据类型
> @ https://www.runoob.com/sql/sql-datatypes-general.html

# SQL 函数
> SQL 有很多用于计算和计数的内建函数

## SQL Aggregate 函数
从列中取得的值，返回一个单一值
```SQL
AVG( ) - 返回平均值
COUNT( ) - 返回行数
FIRST( ) - 返回第一个记录的值
LAST( )
MAX( )
MIN( )
SUM( )
```
## SQL SCALAR函数
基于输入值，返回一个单一值
``` SQL
UCASE( ) - 将某个字段转换为大写
LCASE( ) - 将某个字段转换为小写
MID( ) - 从某个文本字段提取字符,MySQL
SubString(字段,1,end) - 从某个文本字段提取字符
LEN( ) - 返回字段长度
ROUND( ) - 对数值进行制定小数位的四舍五入
NOW( ) - 返回当前的系统日期和时间
FORMAT( ) - 格式化某个字段的显示方式
```

## SQL GROUP BY 语句
GROUP BY 语句用于结合聚合函数，根据一个或多个列对结果集进行分组。

``` SQL
SELECT COLUMN_NAME, AGGREGATE_FUNCTION(COLUMN_NAME)
FROM TABLE_NAME
WHERE COLUMN_NAME OPERATOR VALUE
GROUP BY COLUMN_NAME;
```
## SQL HAVING 子句
在SQL增加HAVING子句的原因是，

**WHERE 关键字无法与聚合函数一起使用。** HAVING子句可以让我们筛选分组后的各组数据。

``` SQL
SELECT WEB.NAME, WEB.URL, SUM(LOG.COUNT) AS NUMS FROM (LOG INNER JOIN WEB ON LOG.SITE_ID = WEB.ID)
GROUP BY WEB.NAME
HAVING SUM(LOG.COUNT) > 200;
查找总访问量大于200的网站，其中SUM()是聚合函数，只可以用于HAVING后

SELECT WEB.NAME, SUM(LOG.COUNT) AS NUMS FROM (LOG INNER JOIN WEB ON LOG.SITE_ID = WEB.ID)
WHERE WEB.ALEXA < 200
GROUP BY WEB.NAME
HAVING SUM(LOG.COUNT) > 200;
寻找ALEXA小于200且访问量大于200，WHERE在 GROUP BY 前，HAVING在 GROUP BY 后。同时，聚合函数只能出现在HAVING后。
```
## SQL EXISTS 实例
EXISTS运算符用于判断查询子句是否有记录，如果有一条或多条记录存在返回TRUE，否则返回FALSE
``` SQL
SELECT COLUMN_NAME
FROM TABLE_NAME
WHERE EXISTS
(SELECT COLUMN_NAME FROM TABLE_NAME WHERE CONDITION);

查询总访问量大于200的网站是否存在
SELECT WEB.NAME , WEB.URL
FROM WEB
WHERE EXISTS (SELECT COUNT FROM LOG WHERE WEB.ID = LOG.ID AND COUNT > 200);

同时EXISTS可以与NOT一同使用，查找出不符合查询语句的记录
SELECT WEB.NAME, WEB.URL
FROM WEB
WHERE NOT EXISTS( SELECT COUNT FROM LOG WHERE WEB.ID = LOG.ID AND COUNT > 200)
```


