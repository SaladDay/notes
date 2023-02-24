### Sklearn



#### 1. Sklearn的转换器(transformer)和估计器(estimator)

##### 1.1 Transformer

> 特征工程的父类（API），所有特征工程子类都需实现其接口

1. 实例化
2. 使用fit_transforme(fit用于计算样本数据，transform用于转化数据得到需要的结果)

##### 1.2 Estimator

> sklearn机器学习算法的实现API，所有的算法需实现其接口

1. 实例化
2. model = estimator.fit(x_train,y_train)，计算并返回模型
3. accuracy = estimator.score(x_text,y_text) ,模型评估

