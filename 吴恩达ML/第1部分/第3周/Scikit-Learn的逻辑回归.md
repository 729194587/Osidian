
构建数据集

```python
import numpy as np

X = np.array([[0.5, 1.5], [1,1], [1.5, 0.5], [3, 0.5], [2, 2], [1, 2.5]])
y = np.array([0, 0, 0, 1, 1, 1])
```
导入scikit-learn的逻辑回归模型
```python
from sklearn.linear_model import LogisticRegression

lr_model = LogisticRegression()
lr_model.fit(X, y)
```
![[导入逻辑回归模型结果.png]]
做出预测：
```python
y_pred = lr_model.predict(X)

print("Prediction on training set:", y_pred)
##输出：Prediction on training set: [0 0 0 1 1 1]
```
计算准确性：
```python
print("Accuracy on training set:", lr_model.score(X, y))
#Accuracy on training set: 1.0
```

短小精悍。