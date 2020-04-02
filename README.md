# Janice Cao Projects

    以下为我在向数据科学的发展过程中的一些成果和学习路径，内容在逐步补充中；  
    我希望能从数据分析的工作开始，逐步学习更多的机器学习算法方面的知识并用到真实的工作分析中。  
    我目前在应届毕业求职阶段，希望能有机会获得数据分析相关的工作机会。  
    我熟练使用SQL进行增删改查，有Python、Excel进行数据分析与可视化，使用Tableau进行数据可视化的经验。  
    我对我的快速学习与分析能力很有信心，曾三天内学习并使用SketchUp建立展览间的3D模型。  
    如果您有相关的工作或参与项目的机会，请联系我 cjy31@163.com，非常感谢。

*部分内容为以前学习过程中整理，当时未记录来源，如有不当请提出，如果之后找到reference会及时补充*

## 目次

1. [数据分析统计基础（更新中）](statistical_probability)
2. 数据分析项目实践
    1. [Airbnb Beijing rental data analysis](#airbnb2)
    2. [Cosmetics Shop eCommerce Events analysis](#cosmetics2)  
    3. [San Francisco Library Patrons and Collection Usage analysis](#sf2)  
3. [数据库(更新中)](SQL)
    1. SQL知识点
    2. SQL练习
4. [业务相关问题分析](senario_analysis)
5. [机器学习（更新中）](Machine_learning)


### Update Record: 

|日期|内容|
|:-:|:-|
|2020.04.2|美妆电商分析案例增加留存率分析部分、增加前半部分分析文字说明|
|2020.04.1|1. SQL窗口函数题目更新（含mysql变量解决问题） <br> 2. [增加主要知识点、问题梳理](https://shimo.im/docs/yxTCRKGqdwGCkgKW/)|  
|2020.03.28|1. SQL知识点大纲与部分内容 <br> 2. 添加决策树部分内容|  
|2020.03.27|1. SQL练习题目同步完成 <br> 2. 增加销售额、留存率异动的拆分思路 <br> 3. 添加kNN算法内容|  
|2020.03.26|1. 旧金山图书馆项目：增加读者类型预测模型、读者借阅次数假设检验分析 <br> 2. 添加SQL练习题目|  
|2020.03.24|1. 增加中文说明 <br> 2. 添加机器学习与数据分析基础知识的架构|  

## <span id = "airbnb2">爱彼迎北京房源信息分析</span>  

1. Introduction: 基于爱彼迎（Airbnb）北京的房源资料，对爱彼迎北京的房源数量、类型、价格分布针对不同区进行了分析比较；对房东的信息，包括拥有多个房源房东的信息进行了分析；根据房源标题，使用jieba库进行分词并绘制词云图。
2. 使用技能Technical skills used:  
    1. Python: 使用Pandas, Numpy库对数据进行清洗、分析, 使用Matplotlib, Seaborn库进行数据可视化，包括饼图、换图、直方图、条形图、堆积柱状图, 使用jieba库对房源标题进行分词, 并使用WordCloud绘制词云图  
    2. PPT：自制PPT模板并针对主要数据进行报告
    3. Tableau：根据资料中的经纬度，绘制可交互的地图，展示具体的房源价格、描述，并制作仪表板
3. 报告:
    1. [PPT](01_airbnb_beijing/Airbnb_Beijing_presentation.pdf)  
    [PPT online (Google端)](https://drive.google.com/open?id=1Ll-_WxqQtc6lezQsmIO94QS7JWmb0dWU)
    2. [Juypter notebook: Python代码与简要分析](01_airbnb_beijing/airbnb_beijing_python.ipynb)
4. 数据来源：  
  Dataset reference: http://insideairbnb.com/get-the-data.html  

## <span id = "cosmetics2">巴西美妆电商月度经营状况分析</span>

1. 介绍: 对巴西某美妆电商在2019年10月的410万笔销售数据进行分析，具体包括： 
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
2. 报告:  
    1. [分析报告](02_cosmetic_ecommerce/Ecommerce_events_history_in_cosmetics_shop.pdf)  
    2. [Juypter notebook: Python代码与简要分析](02_cosmetic_ecommerce/Cosmetic_Ecommerce_Shop_User_Events_Analysis.ipynb)
3. 数据来源:  
    Dataset reference: https://www.kaggle.com/mkechinov/ecommerce-events-history-in-cosmetics-shop

## <span id = "sf2">旧金山图书馆使用数据分析</span>

1. 介绍：对旧金山公共图书馆2003年到2016年的42万位读者数据，使用Python分析读者信息、馆藏使用与图书馆工作人员异动并撰写英文分析报告
    1. 数据统计：  
        1. 分析旧金山公共图书馆不同类型的读者增长、年龄分布、借阅资料频率差异，通过matplotlib库绘制条形图、散点图、堆叠柱状图比较  
        2. 分析27个图书馆分馆的业务差异，通过使用seaborn绘制热力图展示各分馆的读者情况
    2. 异常分析：通过数据推断读者注册时年龄默认值引起的年龄异常；通过数据分组统计，发现部分读者借阅量过高及数字资料借阅量过低，提出数据采集过程中的可能异常  
    3. 预测读者类型：通过sklearn对处理分类特征，使用决策树、随机森林算法根据读者年龄、注册馆、活跃日期、居住地情况、通知方式等维度，训练预测读者类型的模型并预测，使用不同树深、不同决策树优化结果  
    4. 假设检验：使用方差检验、Tukey检验分析不同读者年龄段间的借阅数据差异、来自不同分馆的读者之间的借阅数差异  
3. 报告:  
    1. [分析报告（Eng Ver.）](03_sf_library/San_Francisco_Library_Usage_analysis_report.pdf)
    2. [Juypter notebook: Python代码与简要分析](03_sf_library/SF_library_usage_analysis.ipynb)
4. 数据来源:  
https://data.sfgov.org/Culture-and-Recreation/Library-Usage/qzz6-2jup  
https://www.kaggle.com/datasf/sf-library-usage-data#Library_Usage.csv

# Eng Ver.

The practical projects I've done or doing to improve my understanding of data analysis and relevent skills.  

## Table of Contents  

1. Data Analysis
    1. [Airbnb Beijing rental data analysis](#airbnb)
    2. [Cosmetics Shop eCommerce Events analysis](#cosmetics)  
    3. [San Francisco Library Patrons and Collection Usage analysis](#sf)  

## <span id = "airbnb">Airbnb Beijing rental data analysis</span>  
1. Introduction: Based on the Airbnb Beijing accommodation information list, the report analyses the information that relates to price range, the amount of acommodation, the number of host and reviews.
2. Technical skills used:  
    1. Python: Pandas, Numpy, Matplotlib, Seaborn, jieba, WordCloud  
    2. PPT
    3. Tableau
3. Report:
    1. [Presentation slides](01_airbnb_beijing/Airbnb_Beijing_presentation.pdf)
    2. [Python code for analysis](01_airbnb_beijing/airbnb_beijing_python.ipynb)
4. Dataset  
  Dataset reference: http://insideairbnb.com/get-the-data.html  

## <span id = "cosmetics">Cosmetics Shop eCommerce Events analysis</span>
1. Introduction: Based on the cosmetics shop eCommerce Event in October 2019, the analysis providess an overview the users bahavior with multiple indicators. Customer segments is also defined based on RFM model and some short suggestions for future marketing activities is provided.    
2. Knowledge points:
    1. Analysis indicators: PV, UV, ARPU, ARPPU, RFM model, conversion rate  
    2. Technical: Python
3. Report:  
    1. [Report document](02_cosmetic_ecommerce/Ecommerce_events_history_in_cosmetics_shop.pdf)  
    2. [Python code for analysis](02_cosmetic_ecommerce/Cosmetic_Ecommerce_Shop_User_Events_Analysis.ipynb)
4. Dataset:  
    Dataset reference: https://www.kaggle.com/mkechinov/ecommerce-events-history-in-cosmetics-shop

## <span id = "sf">San Francisco Library Usage</span>
1. Introduction: By using the library usage data in San Francisco from 2003 to 2016, the analysis concludes the overview of patron information, checkout and renewal data among different patron types and a short summary about staff information.
2. Technical skills used:
    1. Python
    2. Excel
3. Report:  
    1. [Report document](03_sf_library/San_Francisco_Library_Usage_analysis_report.pdf)
    2. [Python code for analysis](03_sf_library/SF_library_usage_analysis.ipynb)
4. dataset:  
https://data.sfgov.org/Culture-and-Recreation/Library-Usage/qzz6-2jup  
https://www.kaggle.com/datasf/sf-library-usage-data#Library_Usage.csv


