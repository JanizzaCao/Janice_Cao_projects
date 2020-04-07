# Part 1 知识点总结

## 1.1 数据库基础

### [基础概念](#db_basic)

### SQL语句

1. [数据查询语言DQL](#sql_dql)
2. [数据操作语言DML](#sql_dml)
3. [数据定义语言DDL](#sql_ddl)
4. [常用函数](#sql_function)

## 1.2 高频知识点

### [DQL](0121DQL.md)

1. SQL执行顺序
2. 列转行&行转列
3. TopN问题
5. JOIN：
6. 日期数据相关函数
### [DML, DDL, DCL](0122dml_ddl_dcl.md)

1. Delete, truncate, drop区别
2. SQL数据类型
### [其他](0123other_keypoints.md)

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
    1. 独立子查询
    2. 相关子查询
    3. 子查询常见搭配
        1. 与IN运算符：
        2. 与ANY运算符：
        3. 与ALL运算符
        4. 与EXISTS, NOT EXISTS
    4. 使用WITH AS
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
6. 高级查询运算词

### <span id = "sql_dml">数据操作语言DMLL</span>

### <span id = "sql_ddl">数据定义语言DDL</span>

### <span id = "sql_function">常用函数</span>

