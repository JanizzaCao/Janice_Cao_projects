# Part 1 知识点总结

## 1.1 数据库基础

### [基础概念](#db_basic)
1. 数据库原理
2. 事务
3. SQL数据类型
4. MySQL存储引擎

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
    1. 索引：DBMS中一个排序的数据结构，协助快速查询、更新数据库表中数据；没有索引则需要遍历整张表。原理为：
        1. 将创建了缩印的列排序
        2. 对排序结果生成倒排表
        3. 在倒排表内容上拼上数据地址链
        4. 在查询时，先拿到倒排表，再取出数据地址连，取到具体数据
    2. 优点：加快数据检索速度  
        缺点：创建、维护耗费时间，降低增改删效率；占据物理空间
    3. 使用场景
        1. WHERE：使用索引列筛选
        2. ORDER BY：使用索引列排序时，索引本身有序，只需要按索引顺序逐条读出
        3. JOIN：匹配（ON）时使用索引列
        4. 索引覆盖：当查询字段都建立过索引时，引擎直接查询索引表而非原始数据；因此SELECT时只查必要查询字段，增加索引覆盖的几率
    4. 索引类型：
        1. 主键索引：数据列不允许重复、不允许为NULL，一个表只能有一个主键
        2. 唯一索引：数据列不允许重复，可以为NULL，一个表可有多个列创建唯一索引
        ```
        -- 创建唯一索引
        ALTER TABLE table_name ADD UNIQUE (col_name);
        -- 创建唯一组合索引
        ALTER TABLE table_name ADD UNIQUE (col1, col2);
        ```
        3. 普通索引：没有唯一性限制，允许为NULL
        ```
        ALTER TABLE table_name ADD INDEX index_name (col_name);
        ALTER TABLE table_name ADD INDEX index_name (col1, col2, col3);
        ```
        4. 全文索引：用于搜索引擎
        ```
        ALTER TABLE table_name ADD FULLTEXT (col_name);
        ```
    5. 索引创建原则：
        1. 最左前缀匹配原则：创建多列索引时，WHERE最频繁使用的列放在最左边；MySQL在查询时一直向右匹配到遇到范围查询（< > between like）
        2. 频繁查询的字段建立索引；频繁更新的字段不适合建立索引；重复值多的数据列不适合索引（如性别）
        3. 有外键的数据列一定要建立索引
        4. 定义为TEXT, IMAGE, BIT的数据类型不要建索引
    6. 索引的数据结构：
        1. B树：可用于比较=, <, >, <=, >=, between and, like（不以通配符开头）
        2. HASH：在绝大多数需求为单条记录查询时使用，查询性能快；只用于对等比较=

2. 数据库范式
    1. 第一范式：每个列都不可以再拆分
    2. 第二范式：在第一范式的基础上，非主键列完全依赖于主键，而不能是依赖于主键的一部分
    3. 第三范式：在第二范式的基础上，非主键列只依赖于主键，不依赖于其他非主键
3. 窗口函数原理

### 事务
1. 事务：一组操作，要么都执行，要么都不执行（例：银行转账）
2. ACID属性：
    1. 原子性：事务是最小的执行单位，不允许分割。事务的原子性确保动作要么全部完成，要么完全不起作用；
    2. 一致性：执行事务前后，数据保持一致，多个事务对同一个数据读取的结果是相同的；
    3. 隔离性：并发访问数据库时，一个用户的事务不被其他事务所干扰，各并发事务之间数据库是独立的；
    4. 持久性：一个事务被提交之后。它对数据库中数据的改变是持久的，即使数据库发生故障也不应该对其有任何影响。
3. 事物的隔离级别：
    1. 脏读：某个事务已更新一份数据，另一个事务在此时读取了同一份数据，由于某些原因，前一个RollBack了操作，则后一个事务所读取的数据就会是不正确的。
    2. 不可重复读：在一个事务的两次查询之中数据不一致，这可能是两次查询过程中间插入了一个事务更新的原有的数据。
    3. 幻读：在一个事务的两次查询中数据笔数不一致，例如有一个事务查询了几列(Row)数据，而另一个事务却在此时插入了新的几列数据，先前的事务在接下来的查询中，就会发现有几列数据是它先前所没有的。

|隔离级别|脏读|不可重复读|幻影读|说明|
|:-:|:-|:-|:-|:-|
|READ-UNCOMMITTED|O|O|O|最低的隔离级别，允许读取尚未提交的数据变更|
|READ-COMMITTED|X|O|O|允许读取并发事务已经提交的数据|
|REPEATABLE-READ|X|X|O|对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改|
|SERIALIZABLE|X|X|X|最高的隔离级别，完全服从ACID的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰|

    

### SQL数据类型
基于MySQL：  
1. 整数：都可加入UNSINGED属性表示无符号 INT(integer), TINYINT, SMALLINT, MEDIUMINT  
2. 小数： FLOAT, DOUBLE, DECIMAL(m, d)  
3. 日期： YEAR, TIME, DATE, DATETIME, TIMESTAMP
4. 文本、二进制：CHAR(m), VARCHAR(m), TINYBLOB, BLOB, MEDIUMBLOB, LONGBLOB, TINYTEXT, TEXT, MEDIUMTEXT, LONGTEXT, VARBINARY(m), BINARY(m)
5. 枚举类型：ENUM 存储不重复的数据为一个预定义的集合

### MySQL存储引擎
1. 主要区别
    1. InnoDB：支持ACID事务，提供行级锁、外键约束；用于处理大数据容量；用于更新删除频率高，保证数据完整性，高并发
    2. MyISAM：不支持事务、行级锁、外键；用于读写插入为主的应用程序
    3. MEMEORY：数据存在内存中，快速，安全性低
2. 比较表

||MyISAM|InnoDB|
|:-:|:-|:-|
|存储结构|每张表被存放在三个文件：frm-表格定义、MYD(MYData)-数据文件、MYI(MYIndex)-索引文件|所有的表都保存在同一个数据文件中（也可能是多个文件，或者是独立的表空间文件），InnoDB表的大小只受限于操作系统文件的大小，一般为2GB|
|存储空间|可被压缩，存储空间较小|需要更多的内存和存储，会在主内存中建立其专用的缓冲池用于高速缓冲数据和索引|
|可移植性、备份及恢复|数据以文件的形式存储，在跨平台的数据转移中很方便 <br> 备份、恢复时可单独针对某个表进行操作|免费的方案可以是拷贝数据文件、备份 binlog，或者用 mysqldump，在数据量达到几十G的时候就相对痛苦了|
|文件格式|数据和索引是分别存储的，数据.MYD，索引.MYI|数据和索引是集中存储的，.ibd|
|记录存储顺序|按记录插入顺序保存|按主键大小有序插入|
|外键|不支持|支持|
|事务|不支持|支持|
|锁支持（锁是避免资源争用的一个机制，MySQL锁对用户几乎是透明的）|表级锁定|行级锁定、表级锁定，锁定力度小并发能力高|
|SELECT|MyISAM更优||
|INSERT、UPDATE、DELETE||InnoDB更优|
|select count(\*)|更快，其内部维护了一个计数器，可以直接调取||
|索引的实现方式|B+树索引，是堆表|B+树索引，是索引组织表|
|哈希索引|不支持|支持|
|全文索引|支持|不支持|

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
1. 视图
    1. 概念：本质为虚拟表
    2. 特点：
        1. 列可以来自不同的表；是由不同的实表（基本表）产生的虚表
        2. 视图建立、删除不影响基本表
        3. 对视图内容的更新（增删改）直接影响基本表；当内容来自多个基本表时不允许添加、删除数据
    3. 使用场景：复用SQL语句，简化SQL查询操作；使用表的部分内容；保护数据（减少授权）；更改数据格式和表示
    4. 优点：简化查询；数据安全；增加数据逻辑独立性  
        缺点：查询性能降低、修改内容受限制（对于有UNIQUE操作符、GROUP BY子句、聚合函数、DISTINCT、JOIN的视图不可修改内容）
    5. 游标
2. 存储过程
    1. 概念：存储过程是预编译的SQL语句，可以重复调用这一个模块的SQL语句
    2. 优点：执行效率高；安全性高；可重复使用  
        缺点：调试麻烦；重新编译；维护
3. 触发器
    1. 概念：定义在关系表上的一类由事件驱动的特殊的存储过程；当触发某个事件时，自动执行
    2. MySQL支持的触发器：BEFORE INSERT, AFTER INSERT, BEFORE UPDATE, AFTER UPDATE, BEFORE DELETE, AFTER DELETE

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
