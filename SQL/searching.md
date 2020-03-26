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

3. 
