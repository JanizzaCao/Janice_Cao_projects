# 

## 1 函数用法
### 时间函数
1. Timestamp, datetime之间转化:
    1. 基于Mysql:   
    ```
    SELECT UNIX_TIMESTAMP("2018-11-17 23:59:59"); --输出 1542470399单位为秒
    SELECT FROM_UNIXTIME(1542470399, '%Y-%m-%d'); --输出 2018-11-17
    ```
    时间修饰符：%M月全名，%b月3字母缩写，%W星期全名  
    %Y4位年，%y2位年；%d日期数字，%m月数字，%c月数字不补零；  
    %j 一年中天数  
    %H 小时（24制） %h小时（12制）%S秒 %p AM或PM %U年中第几周  
2. DATE_ADD(date, INTERVAl n DAY): n可以为负数
    DATE_SUB()  
3. DATEDIFF(date1, date2), TIMEDIFF(time1, time2)  
4. day(), month(), year() 取事件中日、月、年  
5. Mysql: NOW() 当前日期和时间，CURDATE() CURTIME() 当前日期、时间；DATE() 获取日期部分

### 字符串函数（基于Mysql）
1. Concat函数:   
    CONCAT("1", "+2") 拼接    
    CONCAT_WS(separator, str1, str2) 根据连接符连接字符串  
2. 字符串截取函数：
    LEFT(col, 1), RIGHT(), MID
    SUBSTR(str, start_pos, len)截取返回  
    TRIM()  
    LTRIM(str), RTIRM(str) 去空格  
    REPLACE(str, from_str, to_str)  
3. 长度：LENGTH() CHAR_LENGTH  
4. LOWER(str), UPPER(str)  
5. 查找：LOCATE("k", col) 返回第一个k的位置

### 聚合函数
1. MAX(), MIN(), SUM(), AVG()  
    ROUND(data, 小数位数)
2. COUNT(): COUNT(*) 统计含null，COUNT(1), COUNT(DISTINCT col1) 不含null

### CAST(col AS type) 转换类型
    类型可以为：BINARY, CHAR[(N)], DATE, DATETIME, DECIMAL, TIME, SIGNED有符号int型, UNSIGNED无符号int型

## 2 指定数值计算
1. 取众数、四分位数、中位数
    1. 众数  
    思路：使用group by + having=max()
    2. 四分位数  
    使用count(*)%2 = 1判断总个数奇偶，使用NTILE(4)分四份，从小到大排，取第一组最后一个、第二组第一个平均为Q1
    3. 中位数：表orders: order_id, mall_id, goods_id, sale_number, amount 求每个商店的商品价格中位数  
2. 随机选择1%的id  
    SELECT * FROM table ORDER BY RAND() LIMIT 10; 
3. Null值存在对计算的影响  
    NULL在计算AVG时，不被算在分母个数中

## 指定需求
1. 行转列:  
    1. CASE WHEN
    2. PIVOT_TABLE() 
2. 单独一个数据列去重
3. 取每月最后一天的最后三笔订单且代码需要可复现（tricky点在于如何找每月最后一天）
4. 计算留存率、LTV
5. 计算各班成绩排名第二同学的成绩平均值
6. 计算日累计销售金额（两种方法）
7. 筛选不同品牌商品2017年销售总额、2018年销售总额、2018年销售额增长率
8. 找到近7天、30天、90天的登陆人数（不能用U）
