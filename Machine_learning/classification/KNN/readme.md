# k-Nearest-Neighbors (kNN) classification algorithm
## 基础理论
1. 思想简述：每个样本可以用离他最近的k个邻居来代表
2. 步骤：
    1. 计算测试对象到训练集中每个对象的距离
    2. 按距离远近排序
    3. 选取与当前测试对象最近的k个训练对象，作为该测试对象的邻居
    4. 统计k个邻居的类别频次
    5. K个邻居例频次最高的类别为测试对象的类别

## 实现
1. [自实现代码](knn_code.ipynb)
2. [sklearn.classification.KNeighborsClassifier](knn_sklearn.ipynb)  
    ```
    from sklearn.neighbors import KNeighborsClassifier
    kNN_classifier = KNeighborsClassifier(n_neighbors=6, weights=”uniform”, algorithm=”auto”, leaf_size, p=1, metric=’minkowski’, metric_params=None, n_jobs=None) weights权重可选distance, 或传入数组；algorithm可选ball_tree, kd_tree, brute; p=1为曼哈顿距离，2为欧几里得距离
    kNN_classifier.fit(X_train, y_train)
    y_predict = kNN_classifier.predict(x.reshape(1,-1)) # kNN进行预测predict，需要传入一个矩阵，而不是数组。reshape()成一个二维数组，第一个参数是1表示只有一个数据，第二个参数-1，numpy自动决定第二维度有多少
    ```
    |方法|说明|
    |-|-|
    |fit(X,y)|X为训练数据，y为分类结果，拟合模型|
    |get_param(\[deep\])|获取估值器参数|
    |neighbors(\[X, n_neighbors, return_deistance\])|查找一个或几个点的k个邻居|
    |kneighbors_graph(\[X,n_neighbors, mode\])|计算在X数组中每个点的k的邻居的（权重）图|
    |predict(\[X\])|提供数据预测对应的分类|
    |score(X, y\[, sample_weight\])|返回测试数据、标签的平均准确值|
    |set_params(\*\*params)|设置估值器参数|

## 优缺点
|优点|缺点|
|-|-|
|1. 理论简单，可以做分类、回归，可解决多分类问题；<br> 2. 与朴素贝叶斯等算法相比，对数据没有假设，对异常点不敏感 <br> 3. 对于类域交叉、重叠较多的待分样本更合适|1. 计算量大，效率低 <br> 2. 高度数就相关、样本不平衡时，对稀有类别预测准确率低 <br> 3. 与决策树相比，可解释性不强 <br> 4. 维度灾难:随着维度增加，看似相近的两个点距离越来越大（例：100\*100像素的黑白灰图片有一万维）|

## 模型优化与发展
### KD树(待补充)
