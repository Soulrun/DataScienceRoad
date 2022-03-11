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
