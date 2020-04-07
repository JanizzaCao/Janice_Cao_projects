# 分类算法总结
|算法|步骤|优点|缺点|
|:-:|:-|:-|:-|
|[KNN](#knn)|1.计算测试样本到训练集中每个样本距离，从小到大排序 <br> 2. 统计与测试样本距离最近的k个邻居的类别频次 <br> 3. 邻居中频次最高的类为测试对象的类|1. 简单容易实现 <br> 2. 在线技术：新数据可直接加入不用重新训练 <br> 3. 训练时间复杂度O(n)，适用大样本 <br> 4. 适用于类域重叠、多交叉的样本 <br> 5. 对数据没有假设，对outlier不敏感 |1. 计算量大，可解释性差，每次分类要重新计算。改进：KD树 <br> 2. k值大小要预先选择，不能自适应 <br> 3. 不平衡样本、稀有类别的预测准确率低 <br> 4. 维度灾难：随着维度快速提高，看似相近的两点距离快速增大 |
|逻辑回归 Logistic Regression|1. <br> 2. <br> 3. | | |
|决策树 Decision Tree|1. <br> 2. <br> 3. | | |
|随机森林 Random Forest|1. <br> 2. <br> 3. | | |
|SVM|1. <br> 2. <br> 3. | | |
|朴素贝叶斯|1. <br> 2. <br> 3. | | |

## <span id="knn">KNN</span>
1. 算法补充：
    1. 距离计算
    2. k值选择
    3. 用KNN做回归
2. Python调用：from sklearn.neighbors import KNeighborsClassifier, KNeighborsRegressor, KDTree

## 
