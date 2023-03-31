# 具有 Azure 机器学习的分类模型 Tensorflow 2.4

> 原文：<https://medium.com/analytics-vidhya/classification-model-tensorflow-2-4-with-azure-machine-learning-6ecb2f5f9fdf?source=collection_archive---------23----------------------->

![](img/18e63e5f4ffa6c61d63e31bbaeaf20ae.png)

# 用例

*   检查 tensorflow 2.4 的版本并运行示例代码进行验证。
*   创建二元分类模型
*   规则表格数据集上的深度学习分类

# 要求

*   Azure 帐户
*   Azure 机器学习帐户
*   创建计算实例
*   minst 数据集

# 密码

```
import tensorflow as tf; 
print(tf.__version__)import tensorflow as tf 
from tensorflow.keras.layers import Dense, Flatten, Conv2D 
from tensorflow.keras import Model 
from tensorflow.keras.layers import Input, Dense, Activation,Dropout from tensorflow.keras.models import Model
```

*   读取数据集
*   文件夹中有数据集

```
diabetes = pd.read_csv(r'diabetes.csv')
diabetes.head()
```

*   分割数据集
*   特征的 x 数据
*   y 代表标签

```
y = diabetes.iloc[:,10] 
X = diabetes.iloc[:,:10]from sklearn.model_selection import train_test_split X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.20, random_state=42)input_layer = Input(shape=(X.shape[1],))
dense_layer_1 = Dense(15, activation='relu')(input_layer)
dense_layer_2 = Dense(10, activation='relu')(dense_layer_1)
output = Dense(1, activation='softmax')(dense_layer_2)

model = Model(inputs=input_layer, outputs=output)
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['acc'])print(model.summary())history = model.fit(X_train, y_train, batch_size=8, epochs=50, verbose=1, validation_split=0.2)score = model.evaluate(X_test, y_test, verbose=1)

print("Test Score:", score[0])
print("Test Accuracy:", score[1])
```

*最初发表于*[*【https://github.com】*](https://github.com/balakreshnan/mlops/blob/master/tensorflow/tf2classification.md)*。*