# IT技能

## Python
1. 基础知识点
2. [练习](#python_practice)


## 数据结构
1. 顺序表
2. 链表
3. 栈与队列
4. 树
5. 图

## 算法
1. 排序算法
2. 分治算法：二分查找
3. 树、图搜索
    1. 广度优先搜索
    2. 深度优先搜索
4. 贪婪算法、动态规划

---

## Python
### 基础知识点
1. 数据类型
2. 内置数据结构
### <span id="python_practice">练习</span>
#### Python数据分析
1. 有逾期表df，格式为pandas.DataFrame:  
(注：来源：https://mp.weixin.qq.com/s/ajZMcot74xPg8bjKNu5JXg) 
    1. 将gender列中“男”“女”分别替换为数值1、0
    2. 将age列的缺失值用age列的均值代替
    3. 计算各省的平均逾期率
    4. 计算广东省男性用户的逾期率
    5. 在df新增一列overdue_grade，其中overdue_days<15时为A，>=15时为B
    6. 将类别型变量education转为哑变量(Dummy variables)，并与原df在axis=1上合并，并删除初始education列

| |order_no|province|gender|age|education|overdue_days|info_label|  
|:-:|------:|-------:|------:|-:|--------:|------------:|--------:|
|1|order_18213|山东|女|29|本科|0|0|
|2|order_16061|四川|女|27|研究生|17|1|
|3|order_18701|广东|男|NaN|大专|12|1|

```
import pandas as pd
import numpy as np
#1 性别转数值：3种方法
df["gender"] = df["gender"].map({"男":1, "女":0})
df["gender"] = df["gender"].replace(["男", "女"], [1,0])
df.loc[df["gender"]=="男","gender"]=1
df.loc[df["gender"]=="女","gender"]=0

#2 age列缺失值填补均值: 使用pandas或sklearn
df.fillna(value={"age":df.age.mean()}, inplace=True)
from sklearn.preprocessing import Imputer
imputer = Imputer(missing_values="NaN", strategy="mean", axis=1)
imputer = Imputer.fit(df.loc[:, "age"])
df.loc[:, "age"] = imputer.transform(df.loc[:, "age"])

#3 计算各省的平均逾期率 = 逾期客户/全部客户
df_overdue = df.groupby("province")["info_label"].sum().reset_index()
df_overdue.columns = ["province", "overdue_cnt"]
df_all = df.groupby('province')['info_label'].count().reset_index()
df_all.columns=['province','all_cnt']
df1 = pd.merge(df_overdue, df_all, on = ['province'], how = 'left')
df1['overdue_pec'] = df1['overdue_cnt']/df1['all_cnt']

#4 广东男性逾期率
overdue_pec_gd = df[(df['province']=='广东') & (df['gender'] == 1)]['info_label'].sum()/df[(df['province']=='广东') & (df['gender'] == 1)]['info_label'].count()

#5 新增overdue_grade列
df['overdue_grade'] = df['overdue_days'].apply(lambda x: 'A' if x<15 else 'B')

#6 education转为dummy variables
df=pd.concat((df,pd.get_dummies(df['education'])),axis=1)
df.drop(['education'],axis=1)
```
