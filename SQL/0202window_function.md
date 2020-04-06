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
    CREATE VIEW brand_sale AS(
      SELECT brand_name, logday, SUM(sale_amt) AS total_sale,
      FROM a JOIN b ON a.sku_id = b.sku_i
      WHERE year(a.logday) = 2017 AND b.user_name = "小明"
      GROUP BY 1,2)
    SELECT brand_name, logday, sale_amt
    FROM (SELECT brand_name, logday, total_sale, 
          ROW_NUMBER() OVER(PARTITION BY brand_name ORDER BY total_sale DESC) AS rank
          FROM brand_sale)
    WHERE rank <= 3;
    ```
    2. 统计小明负责品牌2017年连续三天增长>50%的日期及销售额
    ```
     CREATE VIEW brand_sale AS(
      SELECT brand_name, logday, SUM(sale_amt) AS total_sale,
      FROM a JOIN b ON a.sku_id = b.sku_i
      WHERE year(a.logday) = 2017 AND b.user_name = "小明"
      GROUP BY 1,2)
    SELECT brand_name, log_day, s1
    FROM (SELECT brand_name, logday, total_sale as s1
          LEAD(total_sale,1) OVER(PARTITION BY brand_name ORDER BY logday) AS s2,
          LEAD(total_sale, 2) OVER(PARTITION BY brand_name ORDER BY logday) AS s3,
          LEAD(total_sale, 3) OVER(PARTITION BY brand_name ORDER BY logday) AS s4
          FROM brand_sale)
    WHERE s2/s1>1.5 AND s3/s2>1.5 AND s4/s3>1.5;
    ```

6. 用户签到表user_att: date日期，user_id用户编号, is_sign_in是否签到（0否1是）
    1. 计算截止今天每个用户已经连续签到的天数：输出当天签到的所有用户、签到天数
    ```
    -- 先判定用户今天是否签到再计算
    SELECT user_id, DATEDIFF(NOW(), nearest_not) AS signin_times
    FROM
       (SELECT user_id, MAX(date) as nearest_not
       FROM user_att
       WHERE user_id IN (SELECT user_id FROM user_att 
                         WHERE date = CURDATE() AND is_sign_in = 1)
       AND is_sign_in = 0
       GROUP BY user_id);
    ```
    ```
    -- 使用signin_times>0筛选今天没有签到的用户
    SELECT user_id, DATEDIFF(NOW(), nearest_not) AS signin_times
    FROM
       (SELECT user_id, MAX(date) as nearest_not
       FROM user_att
       WHERE is_sign_in = 0
       GROUP BY user_id)
    WHERE signin_times > 0;
    ```
    2. 计算每个用户历史最大连续签到天数：输出所有出现过的用户，其最大连续签到天数
    ```
    -- 使用mysql变量
    SELECT user_id, MAX(signin_continue)
    FROM (SELECT user_id, 
         @num: = IF(is_sign_in=1 and @user_id=user_id, @num+1, 1) AS signin_continue, --判断如果签到且是上一笔相同的用户，则连续签到日加1，否则重置为1
         @user_id:=user_id --获取本笔记录的用户id
         FROM user_att, (SELECT @num:=0, @user_id:=null) temp
         ORDER BY user_id, date)
    GROUP BY user_id; --选出每个用户最大的连续签到日
    ```
    注：题目来源：https://mp.weixin.qq.com/s/dfzfC__vk4ESzOjOBJG3-w 原答案第一问似乎没有筛选掉当天未签到的用户；原答案第二问使用Oracel WM_CONCAT()函数，此处使用mySql变量解决

7. 表grade: sname学生名, score分数, cid科目, 求出所有科目都>80分的学生名
   ```
   SELECT sname FROM (
      SELECT sname, MIN(score) OVER (PARTITION BY sname) AS min_score
      FROM grade)
   WHERE min_score > 80;
   ```

8. 表orders: uid, start_time, end_time  
   取出开始时间、结束时间间隔差5分钟uid，取出第二个订单的uid  

9. 表s: uid学号, cname课程名, score课程成绩  
    1. 求有两门课成绩<60的学生学号，及其所有课平均成绩在整体平均的排名  
    ```
    ```
    2. 求所有人在"k"课程的成绩排名  
    ```
    ```

10. 订单表orders: userid, cost下单金额，brand购买品牌，goods商品名称, time下单时间   
   求最近30天用户消费金额的排名并给出该用户购买了多少种品牌的产品（品牌数不是商品数）
    ```
    ```

11. 表user_log: uid, log_page 登陆页编码, log_time登陆时间  
    找出所有的登陆过A、...B、...(非C)...D的用户，就是先登陆A网页，之后登陆B网页，之间可以登陆其他所有网页，再登陆D网页，B和D网页之间可以登陆任何非C网页的其他网页  
    思路：用left join，先找出用户A的时间，然后left join到所有A之后的B，再找到所有B之后的D，根据此表，对应一个C网页的时间表，只要用户C网页时间在任意B和D之间没有就输出。  
    ```
    
    ```

12. 表orders: oid, uid, time  
    统计每周都有购买的uid数量  
    思路：使用CASE WHEN记录当周购买为1，未购买为0，相加>4
    ```
    
    ```
