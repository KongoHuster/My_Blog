---
title: 第一部分 化学热力学基础
date: 2018-10-06 16:07:40
categories:
- SQL学习
tags:
- SQL
---

## SQL

### 一、SQL简介
#### 1.SQL 是什么？
- SQL，指结构化查询语言，全称是 Structured Query Language。
- SQL 让您可以访问和处理数据库。
- SQL 是一种 ANSI（American National Standards Institute 美国国家标准化组织）标准的计算机语言。
<!-- more -->
#### 2.SQL命令(对于大小写不敏感)
1.创建表
```
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
```
2.删除表
```
DROP TABLE COMPANY;
```
3.Insert语句
```
//方法一
INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (1, 'Paul', 32, 'California', 20000.00 );

//方法二
INSERT INTO COMPANY VALUES (7, 'James', 24, 'Houston', 10000.00 );
s
```
4.得到表的完整信息
```
.schema COMPANY //COMPANY是表的名称

```
5.输出表格的完整的信息
```
// 打印整个表格
SELECT * FROM table_name;

// 指定想要的字段打印
SELECT ID, NAME, SALARY FROM COMPANY;

// 设置输出列的宽度
sqlite>.width 10, 20, 10
sqlite>SELECT * FROM COMPANY;

//SELECT 语句列出了 SALARY 大于 50,000.00 的所有记录：
SELECT * FROM COMPANY WHERE SALARY > 50000;
```
用来设置正确格式化的输出

```
sqlite>.header on
sqlite>.mode column
sqlite> SELECT * FROM COMPANY;
```

6.Where 子句
```
SELECT * FROM COMPANY WHERE AGE >= 25 AND SALARY >= 65000;

SELECT * FROM COMPANY WHERE NAME LIKE 'Ki%';
```
7.UPDATE 语句
```
UPDATE COMPANY SET ADDRESS = 'Texas' WHERE ID = 6;
```
8. Delete 语句
```
DELETE FROM COMPANY WHERE ID = 7;

//如果您想要从 COMPANY 表中删除所有记录，则不需要使用 WHERE 子句，DELETE 查询如下：
ELETE FROM COMPANY;
```
9.Like 子句

SQLite 的 LIKE 运算符是用来匹配通配符指定模式的文本值。如果搜索表达式与模式表达式匹配，LIKE 运算符将返回真（true），也就是 1。这里有两个通配符与 LIKE 运算符一起使用：

- 百分号 （%）:代表零个、一个或多个数字或字符

- 下划线 （_）:代表一个单一的数字或字符。
```
SELECT * FROM COMPANY WHERE AGE  LIKE '2%';
```
10.Glob 子句

SQLite 的 GLOB 运算符是用来匹配通配符指定模式的文本值。如果搜索表达式与模式表达式匹配，GLOB 运算符将返回真（true），也就是 1。与 LIKE 运算符不同的是，GLOB 是大小写敏感的，对于下面的通配符，它遵循 UNIX 的语法。

- 星号 （*）：代表零个、一个或多个数字或字符。
- 问号 （?）：代表一个单一的数字或字符。
```
SELECT * FROM COMPANY WHERE AGE  GLOB '2*';
```

11.Limit 子句

SQLite 的 LIMIT 子句用于限制由 SELECT 语句返回的数据数量。

```
SELECT * FROM COMPANY LIMIT 6;

// 可能需要从一个特定的偏移开始提取记录。下面是一个实例，从第三位开始提取 3 个记录：

sqlite> SELECT * FROM COMPANY LIMIT 3 OFFSET 2;
```

11.Order By

SQLite 的 ORDER BY 子句是用来基于一个或多个列按升序或降序顺序排列数据。

```
SELECT * FROM COMPANY ORDER BY SALARY ASC;
```
12.Group By

SQLite 的 GROUP BY 子句用于与 SELECT 语句一起使用，来对相同的数据进行分组。
在 SELECT 语句中，GROUP BY 子句放在 WHERE 子句之后，放在 ORDER BY 子句之前。

```
NAME, SUM(SALARY) FROM COMPANY GROUP BY NAME;
```

13.Having 子句

HAVING 子句允许指定条件来过滤将出现在最终结果中的分组结果。
WHERE 子句在所选列上设置条件，而 HAVING 子句则在由 GROUP BY 子句创建的分组上设置条件。

```
SELECT * FROM COMPANY GROUP BY name HAVING count(name) < 2;
```

14.Distinct 关键字


SQLite 的 DISTINCT 关键字与 SELECT 语句一起使用，来消除所有重复的记录，并只获取唯一一次记录。
有可能出现一种情况，在一个表中有多个重复的记录。当提取这样的记录时，DISTINCT 关键字就显得特别有意义，它只获取唯一一次记录，而不是获取重复记录。
```
sqlite> SELECT DISTINCT name FROM COMPANY;

```

15.退出sqlite3
```
.quit
```

