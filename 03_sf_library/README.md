# 项目介绍
1. 概述：对旧金山公共图书馆2003年到2016年的42万位读者数据，使用Python分析读者信息、馆藏使用与图书馆工作人员异动并撰写英文分析报告
2. 数据统计：  
    1. 分析旧金山公共图书馆不同类型的读者增长、年龄分布、借阅资料频率差异，通过matplotlib库绘制条形图、散点图、堆叠柱状图比较  
    2. 分析27个图书馆分馆的业务差异，通过使用seaborn绘制热力图展示各分馆的读者情况
3. 异常分析：通过数据推断读者注册时年龄默认值引起的年龄异常；通过数据分组统计，发现部分读者借阅量过高及数字资料借阅量过低，提出数据采集过程中的可能异常  
4. 预测读者类型：通过sklearn对处理分类特征，使用决策树、随机森林算法根据读者年龄、注册馆、活跃日期、居住地情况、通知方式等维度，训练预测读者类型的模型并预测，使用不同树深、不同决策树优化结果  
5. 假设检验：使用方差检验、Tukey检验分析不同读者年龄段间的借阅数据差异、来自不同分馆的读者之间的借阅数差异  

# 相关文件
1. [Python code and explanation in Jupyter notebook](03_sf_library/SF_library_usage_analysis.ipynb)
2. [Summary report](03_sf_library/San_Francisco_Library_Usage_analysis_report.pdf)

# 数据源
1. [data](03_sf_library/LIB-0003_DataDictionary_library-usage.xlsx)
2. source website: https://data.sfgov.org/Culture-and-Recreation/Library-Usage/qzz6-2jup 
