# Janice Cao Projects
以下为我在向数据科学的发展过程中的一些成果和学习路径，内容在逐步补充中；  
我希望能从数据分析的工作开始，逐步学习更多的机器学习算法方面的知识并用到真实的工作分析中。  
我目前在应届毕业求职阶段，希望能有机会获得数据分析相关的工作机会，我熟练使用SQL进行增删改查，有Python、Excel进行数据分析与可视化，使用Tableau进行数据可视化的经验。我对我的快速学习与分析能力很有信心，曾三天内学习并使用SketchUp建立展览间的3D模型。  
如果您有相关的工作或参与项目的机会，请联系我 cjy31@163.com，非常感谢。

2020.03.24 UPDATE: 增加中文说明；添加机器学习部分学习内容  

## 目次
1. 数据分析
    1. 基础知识
        1. 基础统计
        2. 概率
        3. 参数估计
        4. 假设检验
    1. 项目
        1. [Airbnb Beijing rental data analysis](#airbnb2)
        2. [Cosmetics Shop eCommerce Events analysis](#cosmetics2)  
        3. [San Francisco Library Patrons and Collection Usage analysis](#sf2)  
2. 机器学习
    1. 数据预处理
        1. 数据清洗
        2. 特征缩放
        3. 降维
    1. 分类算法
        1. KNN
        2. 逻辑回归 Logistic Regression
        3. 决策树
        4. 随机森林 Random Forest
        5. SVM
    2. 聚类算法
        1. K-Means
        2. EM
    3. 关联分析
        1. Apriori
    4. 回归
        1. 简单线性回归
        2. 多元线性回归
        3. 多项式回归
    5. 模型提升
        1. 交叉验证cross validation
        2. 网格搜索grid search

## <span id = "airbnb2">爱彼迎北京房源信息分析</span>  
1. Introduction: 基于爱彼迎（Airbnb）北京的房源资料，对爱彼迎北京的房源数量、类型、价格分布针对不同区进行了分析比较；对房东的信息，包括拥有多个房源房东的信息进行了分析；根据房源标题，使用jieba库进行分词并绘制词云图。
2. 使用技能Technical skills used:  
    1. Python: 使用Pandas, Numpy库对数据进行清洗、分析, 使用Matplotlib, Seaborn库进行数据可视化，包括饼图、换图、直方图、条形图、堆积柱状图, 使用jieba库对房源标题进行分词, 并使用WordCloud绘制词云图  
    2. PPT：自制PPT模板并针对主要数据进行报告
    3. Tableau：根据资料中的经纬度，绘制可交互的地图，展示具体的房源价格、描述，并制作仪表板
3. 报告:
    1. [PPT](01_airbnb_beijing/Airbnb_Beijing_presentation.pdf)
    2. [Juypter notebook: Python代码与简要分析](01_airbnb_beijing/airbnb_beijing_python.ipynb)
4. 数据来源：  
  Dataset reference: http://insideairbnb.com/get-the-data.html  

## <span id = "cosmetics2">Cosmetics Shop eCommerce Events analysis</span>
1. Introduction: Based on the cosmetics shop eCommerce Event in October 2019, the analysis providess an overview the users bahavior with multiple indicators. Customer segments is also defined based on RFM model and some short suggestions for future marketing activities is provided.    
2. Knowledge points:
    1. Analysis indicators: PV, UV, ARPU, ARPPU, RFM model, conversion rate  
    2. Technical: Python
3. Report:  
    1. [分析报告](02_cosmetic_ecommerce/Ecommerce_events_history_in_cosmetics_shop.pdf)  
    2. [Juypter notebook: Python代码与简要分析](02_cosmetic_ecommerce/Cosmetic_Ecommerce_Shop_User_Events_Analysis.ipynb)
4. Dataset:  
    Dataset reference: https://www.kaggle.com/mkechinov/ecommerce-events-history-in-cosmetics-shop

## <span id = "sf2">San Francisco Library Usage</span>
1. Introduction: By using the library usage data in San Francisco from 2003 to 2016, the analysis concludes the overview of patron information, checkout and renewal data among different patron types and a short summary about staff information.
2. Technical skills used:
    1. Python
    2. Excel
3. Report:  
    1. [分析报告（Eng Ver.）](03_sf_library/San_Francisco_Library_Usage_analysis_report.pdf)
    2. [Juypter notebook: Python代码与简要分析](03_sf_library/SF_library_usage_analysis.ipynb)
4. dataset:  
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


