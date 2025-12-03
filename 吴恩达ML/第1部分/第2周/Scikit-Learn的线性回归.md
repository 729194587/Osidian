
Scikit-learn 有一个梯度下降回归模型 `sklearn.linear_model.SGDRegressor`。

引用包
```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import SGDRegressor
from sklearn.preprocessing import StandardScaler
from lab_utils_multi import  load_house_data
from lab_utils_common import dlc
np.set_printoptions(precision=2)
plt.style.use('./deeplearning.mplstyle')
```
加载数据集：
```python
X_train, y_train = load_house_data()
X_features = ['size(sqft)','bedrooms','floors','age']
```
### 对训练数据进行扩展/归一化
```python
scaler = StandardScaler()
X_norm = scaler.fit_transform(X_train)
print(f"Peak to Peak range by column in Raw        X:{np.ptp(X_train,axis=0)}")   
print(f"Peak to Peak range by column in Normalized X:{np.ptp(X_norm,axis=0)}")
#运行结果
#Peak to Peak range by column in Raw X:[2.41e+03 4.00e+00 1.00e+00 9.50e+01] 
#Peak to Peak range by column in Normalized X:[5.85 6.14 2.06 3.69]
```
### 创建并拟合回归模型
```python
sgdr = SGDRegressor(max_iter=1000)
sgdr.fit(X_norm, y_train)
print(sgdr)
print(f"number of iterations completed: {sgdr.n_iter_}, number of weight updates: {sgdr.t_}")
#SGDRegressor() number of iterations completed: 129, number of weight updates: 12772.0
#模型在随机梯度下降回归器SGDRegressor里迭代129次，权重被更新12772次。
```
### 查看参数
```python
b_norm = sgdr.intercept_
w_norm = sgdr.coef_
print(f"model parameters:                   w: {w_norm}, b:{b_norm}")
print( "model parameters from previous lab: w: [110.56 -21.27 -32.71 -37.97], b: 363.16")
#model parameters: w: [110.2 -21.11 -32.48 -38.04], b:[363.14] 
#model parameters from previous lab: w: [110.56 -21.27 -32.71 -37.97], b: 363.16
```
### 做出预测
```python
y_pred_sgd = sgdr.predict(X_norm)
# make a prediction using w,b. 
y_pred = np.dot(X_norm, w_norm) + b_norm  
print(f"prediction using np.dot() and sgdr.predict match: {(y_pred == y_pred_sgd).all()}")

print(f"Prediction on training set:\n{y_pred[:4]}" )
print(f"Target values \n{y_train[:4]}")
#prediction using np.dot() and sgdr.predict match: True 
#Prediction on training set: [295.13 485.92 389.59 492.08] 
#Target values [300. 509.8 394. 540. ]
```
可视化
```python
fig,ax=plt.subplots(1,4,figsize=(12,3),sharey=True)
for i in range(len(ax)):
    ax[i].scatter(X_train[:,i],y_train, label = 'target')
    ax[i].set_xlabel(X_features[i])
    ax[i].scatter(X_train[:,i],y_pred,color=dlc["dlorange"], label = 'predict')
ax[0].set_ylabel("Price"); ax[0].legend();
fig.suptitle("target versus prediction using z-score normalized model")
plt.show()
```
