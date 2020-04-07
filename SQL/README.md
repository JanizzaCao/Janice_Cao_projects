# Part 1 知识点总结

## 1.1 数据库基础

### [基础概念](#db_basic)
1. 数据库原理
2. 事务
3. SQL数据类型

### SQL语句

1. [数据查询语言DQL](#sql_dql)
2. [数据操作语言DML](#sql_dml)
3. [数据定义语言DDL](#sql_ddl)
4. [常用函数](0203general_query.md)
5. [视图、存储过程](#view_procedure)

## 1.2 高频知识点

### [DQL](#02DQL)
1. SQL执行顺序
2. 列转行&行转列
3. TopN问题
5. JOIN：
6. 日期数据相关函数

### [DML, DDL, DCL](#02dml_ddl_dcl)
1. Delete, truncate, drop区别
2. SQL数据类型

### [其他](#02other_keypoints)

1. 查询优化
2. 窗口函数：使用、实现原理
3. Null相关知识点
4. MySQL引擎区别
5. 索引相关：索引的作用、主键外键作用

# Part 2 练习
1. [基础查询练习](0201searching.md)
2. [窗口函数查询](0202window_function.md)
3. [通用型查询问题](0203general_query.md)
4. DDL、DML语句练习

---
---

# 数据库基础

## <span id = "db_basic">数据库基础概念</span>
### 数据库原理
1. 索引与内外键
2. 窗口函数原理
3. 数据库范式

### 事务
1. ACID属性

### SQL数据类型

## <span id = "sql_gramma">SQL语句</span>

### <span id = "sql_dql">数据查询语言DQL</span>
1. 总体执行过程
```
SELECT [distinct] col|expression [AS new_col_name]
FROM table|view
[WHERE row_limit_expression]
[GROUP BY col_names]
[HAVING group_limit_expression]
[ORDER BY col_names [DESC]] 
[LIMIT m, n] | [LIMIT m OFFSET n] --从第m+1条开始取n调数据
```
2. WHERE子句可用
    1. 算术运算符：>, >=, <, <=, BETWEEN AND (可用数值、字符串)
    2. 逻辑运算符：条件1 AND 条件2， 条件1 OR 条件2
    3. 集合：IN, NOT IN, IS NULL, IS NOT NULL
    4. 聚合函数：AVG, MIN, MAX, SUM, COUNT, ROUND(col_name, 小数位数)
    5. 字符串匹配：模糊查找LIKE, NOT LIKE, 通配符%(≥0个) _ (1个) [ ]在范围内 [! ]不在范围内; REGEXP正则
    6. 谓词：EXISTS, ALL, SOME, UNIQUE
    7. 另一个SELECT语句
3. 逻辑判断
    1. IF(条件表达式，TRUE时的值，FALSE时的值)
    2. CASE WHEN 条件1 THEN 显示内容1  
            WHEN 条件2 THEN 显示内容2
            ELSE 显示内容3
            END AS 显示列名
4. 子查询
    1. 独立子查询：独立运行，与外层无联系
    ```
    SELECT order_id, customer_id FROM sales.orders
    WHERE customer_id IN (SELECT customer_id FROM sales.customers WHERE city = "New York")
    ```
    2. 相关子查询：使用父查询结果；父查询执行一次，子查询才执行一次
    ```
    -- 引用o表的o.order_id结果
    SELECT order_id, (SELECT MAX(list_price) FROM sales_order_item AS i
                      WHERE i.order_id = o.order_id) AS max_list_price
    FROM sales_orders AS o;
    ```
    3. 子查询常见搭配
        1. 与IN运算符：子查询返回一个或多个值
        ```
        -- 查2017年有购买的顾客
        SELECT name FROM customers AS c
        WHERE c.cid IN (SELECT cid FROM orders WHERE YEAR(order_date)=2017);
        ```
        2. 与ANY运算符：>ANY() 大于任何一个子查询则为True
        3. 与ALL运算符：>ALL() 大于子查询所有结果则为True
        4. 与EXISTS, NOT EXISTS：代替IN提高速度
        ```
        -- 同IN例的查询目的：查2017年有购买的顾客
        SELECT name FROM customers AS c
        WHERE EXISTS (SELECT cid from orders AS o
                      WHERE o.cid = c.cid AND YEAR(order_date)=2017);
        ```
    4. 使用WITH AS
    ```
    WITH new_name AS (SELECT col FROM tb_a)
    SELECT a.col FROM tb_a AS a JOIN table_b ON ...
    ```
5. 联结JOIN  
    1. 各种JOIN使用场景
        1. INNER JOIN
        2. LEFT JOIN
        3. RIGHT JOIN
        4. FULL OUTER JOIN
        5. CROSS JOIN  
            <img src="pics/join.jfif" width="50%" align="middle">
    2. JOIN条件在ON和WHERE的区别
    3. LEFT JOIN配对条件的执行顺序
6. 高级查询运算词: 含ALL为不消除结果的重复行
    1. UNION, UNION ALL
    2. EXCEPT, EXCEPT ALL
    3. INTERSECT, INTERSECT ALL

### <span id = "sql_dml">数据操作语言DML</span>
1. 插入
    1. 基本语句：INSERT INTO table_a(col1, col2, ...) VALUES(...) (...)
    2. 从另一表导入:  
    t2存在：INSERT INTO t2(col1, col2, ...) SELECT col1, col2, .. FROM t1
    t2不存在：SELECT col1, col2, ... INTO t2 FROM t1
    3. MySQL
        1. 插入若不存在：INSERT IGNORE INTO table_a VALUES(....)
        2. 创建时从另一张表插入：CREATE TABLE table_a SELECT col1, col2, ... FROM table_b
2. 更新
    1. 基本语句：UPDATE table_a SET col1 = value1 WHERE ...
    2. 使用另一张表更新：  
    UPDATE table_a SET table_a.col1 = table_b.col1 FROM table_a, table_b WHERE table_a.id = table_b.id
3. 删除：DELETE FROM table_a WHERE ...

### <span id = "sql_ddl">数据定义语言DDL</span>
1. 创建
    1. 创建库：CREATE DATABASE db_name;
    2. 创建表：
    ```
    CREATE TABLE [IF NOT EXISTS] tb_name(
                  id INT IDENTITY(1,1) PRIMARY KEY,
                  name VARCHAR(10) UNIQUE NOT NULL,
                  num INT CHECK(num BETWEEN 12 AND 20),
                  note VARCHAR(20) DEFAULT "NA")
    ```
    3. 用已有表创建表：
    ```
    -- Method 1
    CREATE TABLE t2 LIKE t1;
    -- Method 2
    CREATE TABLE t2 AS SELECT col1, col2, ... FROM t1 DEFINITION ONLY;
    ```
2. 修改表
    1. 修改列
    ```
    ALTER TABLE t1 ADD COLUMN new_col VARCAHR(20);
    ALTER TABLE t1 DROP COLUMN col_name [CASCADE|RESTRICT]; --cascade视图等一起删除、restrict没有视图约束才能删除
    ALTER TABLE t1 MODIFY col_name VARCHART(20);
    ```
    2. 对已有表创建索引：索引不可以修改，更改要先删除再创建
    ```
    CREATE UNIQUE INDEX index_name ON table_a(col1);
    CREATE INDEX index_name ON table_a(col1);
    DROP INDEX index_name;
    ```
    3. 主键
    ```
    ALTER TABLE t1 ADD PRIMARY KEY(col_name);
    ALTER TABLE t2 DROP PRIMARY KEY(col_name);
    ```
3. 删除表：DROP TABLE table_name [CASCADE|RESTRICT];

### [常用函数](0203general_query.md)
1. 聚合函数
2. 时间函数
3. 数据清洗

### <span id = "view_procedure">视图、存储过程</span>

## 高频知识点
### <span id = "02DQL">DQL</span>
1. SQL执行顺序
2. 列转行&行转列
3. TopN问题
5. JOIN
6. 日期数据相关函数

### <span id = "02dml_ddl_dcl">DML, DDL, DCL</span>
1. Delete, truncate, drop区别
    1. DROP table_a 不再需要该表时使用，删除表，DDL语言
    2. DELETE: DML语言，慢，搭配where使用较灵活；  
       TRUNCATE：DDL语句，快，可理解为先DROP再CREATE  
       ||TRUNCATE|DELETE|
       |:-:|:-:|:-:|
       |回滚rollback|DDL语句，无回滚|DML语言，执行完可回滚|
       |返回值|0|被删除的行数
       |InnoDB支持一个表一个文件，此时|一次性删除，不激活触发器<br>快|一行一行删除，激活触发器<br>慢|
       |日志|不记录|记录|
       |有列为其他表外键时|失败|成功|
       |有自增列时|计数复原|计数继续|
2. SQL数据类型

### <span id = "02other_keypoints">其他</span>
1. 查询优化
2. 窗口函数：使用、实现原理
3. Null相关知识点
4. MySQL引擎区别
5. 索引相关：索引的作用、主键外键作用
