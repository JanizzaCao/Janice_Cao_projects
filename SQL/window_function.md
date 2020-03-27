1. 表user_goods_table: user_name 用户名，goods_kind 订购外卖品类  
   输出用户名，该用户购买最多的品类  
```
    SELECT user_name, goods_kind  
    FROM (SELECT user_name, goods_kind, RANK() OVER (  
          PARTITION BY user_name ORDER BY count(goods_kind)) AS rank  
          FROM user_goods_table)  
    WHERE rank = 1;   
```

2. 表user_sales_table: user_name用户名, pay_amount 用户支付额度  
    求用户支付金额在前20%的用户
```
    SELECT user_name 
    FROM (SELECT user_name, NTILE(5) OVER (
      ORDER BY SUM(pay_amount) DESC) as percent
      FROM user_sales_talbe
      GROUP BY user_name)
    WHERE percent = 1;
```

3. 表user_login_table: user_name用户名，date用户登录时间
    输出连续七天都登陆的用户名
```
    -- 假设每个用户每天只有一条登录记录
    SELECT user_name 
    FROM (SELECT user_name, date, LEAD(date, 7) 
       OVER(PARTITION BY user_name ORDER BY date) as seven_day
       FROM user_login_table)
    WHERE datediff(seven_day, date) = 7;
```
```
    --假设同上，筛选使用DATE_SUB; CAST()函数转换字符串为时间
    SELECT user_name 
    FROM (SELECT user_name, date, LEAD(date, 7) 
       OVER(PARTITION BY user_name ORDER BY date) as seven_day
       FROM user_login_table)
    WHERE DATE_SUB(CAST(seven_day AS date), INTERVAL 7 DAY)) = CAST(date AS date);
```

4. 表game: 求每个用户结束一场游戏后，平均多久开始下一场游戏，若只玩过一次游戏则不计算

|user_id|start_time|end_time|  
|:---:|:---:|:---:|  
|a|2019/01/01 00:00:00|2019/01/01 00:20:00|
|a|2019/01/01 01:15:00|2019/01/01 01:30:00|
|a|2019/01/01 02:00:00|2019/01/01 02:15:00
|b|2019/01/01 01:15:00|2019/01/01 01:30:00|

```
      SELECT user_id, AVG(gap)
      FROM (SELECT user_id, TIMEDIFF(next_time, end_time) as gap
         FROM (SELECT user_id, end_time, lead(start_time, 1) OVER (PARTITION BY user_id ORDER BY end_time) AS next_time
               FROM game
               WHERE user_id IN (SELECT user_id FROM game GROUP BY user_id HAVING COUNT(start_time)>1)
         WHERE next_time IS NOT NULL)
      GROUP BY user_id; 
```
```
     -- 使用ROW_NUMBER
     WITH ranked AS(
         SELECT driver_id, start_time, end_time, ROW_NUMBER() OVER(PARTITION BY driver_id ORDER BY start_time) AS rank FROM game)
     SELECT driver_id, AVG(time_diff) FROM (
         SELECT driver_id, TIMEDIFF(r2.start_time, r1.end_time) AS time_diff
         FROM ranked AS r1 JOIN ranked AS r2
         ON r1.driver_id = r2.driver_id AND r1.rank = r2.rank-1
         WHERE r2.rank IS NOT NULL)
     GROUP BY driver_id;
```

5. 销售表a：logday销售日期, sku_id商品SKU，sale_amt销售额  
    商品表b: sku_id, bu_name类目, brand_name品牌, user_name负责人
    1. 统计小明负责品牌2017年销售最高的三天及对应销售额
    ```
    
    ```
    2. 统计小明负责品牌2017年连续三天增长>50%的日期及销售额
    ```
    ```
