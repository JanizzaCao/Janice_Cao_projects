# 项目概述
    对巴西某美妆电商在2019年10月的410万笔销售数据进行分析，具体包括： 
1. SQL建库与查询：使用MySQL建立数据库并导入数据，使用Python pymysql库连接数据库查询数据
2. Python数据清洗、分析：
    1. 导入并转换数据格式，处理时间数据，分离日期、小时数据用于指标分析；
    2. 用户活跃度分析：分析每日、当月PV、UV的数据整体趋势与不同行为趋势，通过折线图、点线图展示周期特征；
    3. 营收分析：分析用户购买频率、每日付费用户占比、每日ARPU、ARPPU数据趋势，分析用户复购时间间隔，并通过环形图展示用户复购次数分布
    4. 用户转率漏斗模型建立：提取浏览、加入购物车、购买、移除购物车的行为数据，计算各阶段用户转化率数据
    5. 商品分析：分析品牌销量，对品牌销量top10的转化率进行比较分析，通过条形图进行可视化；统计数据验证品牌销量数据是否符合二八法则
3. 建立用户分层RFM模型：
    1. 人为提取用户RFM数据，通过两级指标、五级指标两种方式建立用户分层模型，指出各个用户层的特征与营销方向，分析各层用户的RFM指数均值
    2. 通过K-Means聚类算法优化用户分层模型的结果，通过散点图对聚类算法结果进行可视化

# 文件  
1. [分析报告](02_cosmetic_ecommerce/Ecommerce_events_history_in_cosmetics_shop.pdf)  
2. [Juypter notebook: Python代码与简要分析](02_cosmetic_ecommerce/Cosmetic_Ecommerce_Shop_User_Events_Analysis.ipynb)

# 数据 
    Dataset reference: https://www.kaggle.com/mkechinov/ecommerce-events-history-in-cosmetics-shop
