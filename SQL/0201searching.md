1. 表order_make: 若某一天中，司机完成≥5单且5单总金额>50，输出日期、司机ID  

|order_id|driver_id|amount|date|  
|:---:|:---:|:---:|:---:|  
|112|A|10|2020/01/01|  
|223|A|50|2020/01/01|  
```
    SELECT date, driver_id FROM order_make
    GROUP BY date, driver_id
    HAVING count(order_id)>=5 AND SUM(amount)>50;
```
  
2. 表active: 用户在当天活跃后，若在未来第2到30天活跃过，则称为当天的30天内留存用户；输出日期、当日的活跃用户数、30天内留存用户数：

|user_id|date|  
|:---:|:---:|  
|a|2019/01/01|
|a|2019/01/03|
|b|2019/01/01|
|b|2019/05/01|
```
    SELECT date, count(distinct user_id), count(distinct user_id_30)
    FROM (SELECT a.user_id, a.date, b.user_id as user_id_30
      FROM active AS a LEFT JOIN active AS b
      ON a.user_id = b.user_id and b.date BETWEEN a.date AND DATE_ADD(a.date, 30)
    GROUP BY user_id;
```
  
3. 活动运营数据分析：  
    订单表orders: user_id 用户编号，order_pay 订单金额，order_time 下单时间  
    活动报名表act_apply: act_id 活动编号，user_id报名用户，act_time报名时间
    1. 统计每个活动对应所有用户在报名后产生的总订单金额，总订单数（设每个用户限报名1个活动，默认用户报名后产生的订单均为参加活动的订单）
    ```
        SELECT a.act_id, SUM(order_pay), COUNT(o.user_id)
        FROM act_id AS a JOIN orders AS o
        ON a.user_id = o.user_id
        WHERE act_time >= order_time
        GROUP BY 1;
    ```
    2. 统计每个活动从开始后到今天平均每天产生的订单数，活动开始时间定义为最早有用户报名的时间（设关于时间数据类型均为datetime）
    ```
        SELECT act_id, num/DATEDIFF(NOW(), start_time)
        FROM (SELECT a.act_id AS act_id, min(a.act_time) AS start_time, count(o.user_id) AS num
              FROM act_apply AS a JOIN orders AS o
              ON a.user_id = o.user_id AND o.order_time >= a.act_time
              GROUP BY act_id);
    ```
    ```
        -- 使用窗口函数MIN()添加最小时间列
        SELECT a.act_id, COUNT(o.order_time)/DATEDIFF(NOW(), a.begin_time)
        FROM (SELECT act_id, user_id, act_time, MIN(act_time) OVER
            (PARTITION BY act_id) AS begin_time FROM act_apply) AS a
        JOIN (SELECT user_idl, order_time FROM orders) AS o
        ON a.user_id = o.user_id
        WHERE a.act_time BETWEEN a.begin_time AND NOW()
        AND o.order_time > a.act_time
        GROUP BY a.act_id; 
    ```
  
4. 用户行为分析：  
    用户行为表tracking_log: user_id用户编号，opr_id操作编号，log_time操作时间  
    1. 计算每天的访客数和他们的平均操作次数
    ```
    SELECT log_date, count(user_id), AVG(times)
    FROM (SELECT date(log_time) AS log_date, user_id, COUNT(opr_id) AS times
          FROM tracking_log
          GROUP BY 1,2)
    GROUP BY 1;
    ```
    2. 统计每天符合以下条件的用户数：A操作后是B操作，AB操作必须相邻
    ```
    SELECT date(log_date), count(user_id)
    FROM (SELECT log_date, user_id, opr_id, LEAD(opr_id, 1) OVER 
          (PARTITION BY user_id ORDER BY log_time) AS opr_id_2
          FROM tracking_log
          WHERE opr_id = "A")
    WHERE opr_id_2 = "B"
    GROUP BY date(log_date);
    ```

5. 用户新增留存分析：用户登录表user_log: user_id用户编号, log_time登陆时间
    求每天新增用户数，第二天、第30天回访比例
    ```
    -- 或使用date(new.reg_time) = date(day2.log_time)-1
    SELECT DATE(new.reg_time) AS day, COUNT(DISTINCT new.user_id) AS new_user,
    COUNT(DISTINCT day2.user_id) AS day2_stay,
    COUNT(DISTINCT day30.user_id) AS day30_stay
    FROM (SELECT user_id, MIN(log_time) AS reg_time FROM user_log
          GROUP BY user_id) AS new
    LEFT JOIN user_log AS day2 ON new.user_id = day2.user_id AND DATEDIFF(day2.log_time, new.reg_time) = 1
    LEFT JOIN user_log AS day30 ON new.user_id = day30.user_id AND DATEDIFF(day30.log_time, new.reg_time) = 29
    GROUP BY 1; 
    ```

6. 用户表A：userid， date注册时间
    充值表B：userid，money充值金额，date充值时间
    1. 同时查处2016年12月注册的用户在注册30天内的付费人数、付费金额
    ```
    SELECT date(A.date), COUNT(DISTINCT B.userid) AS pay_num, SUM(B.money) AS total_pay
    FROM A JOIN B ON A.userid = B.userid AND DATEDIFF(B.date, A.date)<=30
    WHERE YEAR(A.date)=2016 AND MONTH(A.date)=12
    GROUP BY 1;
    ```
    2. 查2016年后付费金额<500, ≥500&＜5000，≥5000的付费人数、付费金额
    ```
    SELECT level, COUNT(DISTINCT userid), SUM(total)
    FROM (SELECT userid, SUM(money) AS total, CASE WHEN total <500 THEN level1 WHEN total <5000 THEN level2 ELSE level3 END AS level
         WHERE YEAR(date)>=2016
         GROUP BY userid)
    GROUP BY level;
    ```

7. 以下两表，求2018年1月用户小明提交母婴类“花王”品牌好评率
    评价表a: id评价编号，create_time创建时间，user_name用户名，goods_id商品编号,sub_time提交时间,set_name评价类型（“好评”、“中评”、“差评”）
    商品表b: good_id, bu_name类目, brand_name品牌
    ```
    SELECT SUM(CASE WHEN a.set_name = "好评" THEN 1 ELSE 0 END)/COUNT(a.id) AS rate
    FROM a JOIN b ON a.good_id = b.good_id
    WHERE a.user_name = "小明" AND b.bu_name = "母婴" AND b.brand_name = "花王"
    AND a.sub_tiem BETWEEN "2018-01-01 00:00:00" AND "2018-01-31 23:59:59"
    ```

8. 成绩表A：student_id, course_name, score
    若每门课>60不及格，统计每门课及格、不及格人数
    ```
    SELECT course_name, SUM(CASE WHEN score<60 THEN 1 ELSE 0 END) AS fail, 
    SUM(CASE WHEN score>=60 THEN 1 ELSE 0 END) AS pass
    FROM A
    GROUP BY course_name;
    ```

9. orders表：
    1. 取出catid下，userid和itemid购买对，格式：c1,uid1.item1, uid2.item2
    2. 解析第一问中的购买对，求出uid1 uid2购买相同商品的次数

|id|userid|catid|itemid|item_num|paytime|  
|:---:|:---:|:---:|:---:|:---:|:---:|    
|1|001|AAA|201|1|2018-09-01 00:00:01|
|1|001|AAA|202|1|2018-09-01 12:00:01|
|2|002|BBB|203|1|2018-09-01 00:00:01|

    ```
    --第一问
    ```
    ```
    --第二问
    ```

10. https://mp.weixin.qq.com/s/dsFUjRLSt3Ts2YPlzeOVcg
