
## 什么是监督学习？
监督学习是机器学习中最常见的学习类型，其核心思想是利用带有标签的训练数据来训练模型。在监督学习中，我们给算法提供输入数据（特征）和对应的正确输出（标签），让算法学习两者之间的映射关系。

特点：有「标准答案（label）」
目标：学一个函数：X → y


## 典型问题

### 回归

预测连续值（如房价、温度）

#### 线性回归

线性回归用于建立输入特征（X）和连续目标变量（y）之间的线性关系模型。它假设特征和目标之间存在线性关系。


**数学表达**：
$$
\hat{y} = w_0 + w_1x_1 + w_2x_2 + \dots + w_nx_n
$$
其中 $w_0$ 是偏置（截距），$w_1 \sim w_n$ 是权重。

**损失函数**：均方误差（MSE）
$$
MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2
$$


模型公式：$y = \beta_0 + \beta_1x_1 + \beta_2x_2 + \cdots + \beta_nx_n + \varepsilon$

损失函数：$\text{MSE} = \frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{y}_i)^2$

最小二乘解：$\hat{\beta} = (X^TX)^{-1}X^Ty$


```python

import numpy as np
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error, r2_score

# 创建示例数据
X = np.array([[1, 1], [1, 2], [2, 2], [2, 3]])
y = np.dot(X, np.array([1, 2])) + 3  # y = 1*x₁ + 2*x₂ + 3

# 划分训练集和测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 创建并训练模型
model = LinearRegression()
model.fit(X_train, y_train)

# 预测
y_pred = model.predict(X_test)

# 评估
print(f"系数: {model.coef_}")
print(f"截距: {model.intercept_}")
print(f"MSE: {mean_squared_error(y_test, y_pred)}")
print(f"R²分数: {r2_score(y_test, y_pred)}")
```


### 分类

预测离散类别（如垃圾邮件/非垃圾邮件、疾病诊断）

#### 逻辑回归

主要用于二分类问题。它通过sigmoid函数将线性回归的输出映射到(0,1)区间，表示概率。

Sigmoid函数：$p = \frac{1}{1 + e^{-z}}$，其中$z = \beta_0 + \beta_1x_1 + \cdots + \beta_nx_n$

损失函数：$\text{Loss} = -\frac{1}{n}\sum_{i=1}^{n}[y_i\log(p_i) + (1-y_i)\log(1-p_i)]$

对数损失（Log Loss / Cross-Entropy Loss）

Softmax函数：$p(y=k|x) = \frac{e^{z_k}}{\sum_{j=1}^{K}e^{z_j}}$


```python
import numpy as np
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, confusion_matrix, classification_report

# 创建示例数据（二分类）
from sklearn.datasets import make_classification
X, y = make_classification(n_samples=1000, n_features=4, n_informative=2, 
                           n_redundant=0, random_state=42)

# 划分训练集和测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 创建并训练模型
model = LogisticRegression()
model.fit(X_train, y_train)

# 预测
y_pred = model.predict(X_test)
y_pred_proba = model.predict_proba(X_test)[:, 1]  # 获取正类的概率

# 评估
print(f"准确率: {accuracy_score(y_test, y_pred)}")
print("混淆矩阵:")
print(confusion_matrix(y_test, y_pred))
print("分类报告:")
print(classification_report(y_test, y_pred))
```

#### 决策树

基本概念:

决策树通过一系列if-then规则对数据进行分割，构建树形结构。每个节点表示一个特征测试，每个分支代表测试结果，每个叶节点代表类别或值。


基尼不纯度：$\text{Gini} = 1 - \sum_{i=1}^{C}p_i^2$

信息增益：$\text{信息增益} = \text{父节点熵} - \sum_{j=1}^{m}\frac{N_j}{N}\times\text{子节点j熵}$

熵：$\text{熵} = -\sum_{i=1}^{C}p_i\log_2(p_i)$


分割准则：

- 分类树：基尼不纯度（Gini impurity）、信息增益（Information gain）
- 回归树：均方误差（MSE）


```python
import numpy as np
from sklearn.tree import DecisionTreeClassifier, DecisionTreeRegressor, plot_tree
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, mean_squared_error
import matplotlib.pyplot as plt

# 分类树示例
from sklearn.datasets import load_iris
iris = load_iris()
X, y = iris.data, iris.target

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 创建分类决策树
clf = DecisionTreeClassifier(max_depth=3, random_state=42)
clf.fit(X_train, y_train)

# 预测和评估
y_pred = clf.predict(X_test)
print(f"分类准确率: {accuracy_score(y_test, y_pred)}")

# 可视化决策树
plt.figure(figsize=(12, 8))
plot_tree(clf, feature_names=iris.feature_names, 
          class_names=iris.target_names, filled=True)
plt.title("决策树可视化")
plt.show()

# 回归树示例
from sklearn.datasets import make_regression
X_reg, y_reg = make_regression(n_samples=200, n_features=2, noise=10, random_state=42)

X_train_reg, X_test_reg, y_train_reg, y_test_reg = train_test_split(
    X_reg, y_reg, test_size=0.2, random_state=42)

reg = DecisionTreeRegressor(max_depth=4, random_state=42)
reg.fit(X_train_reg, y_train_reg)

y_pred_reg = reg.predict(X_test_reg)
print(f"回归树MSE: {mean_squared_error(y_test_reg, y_pred_reg)}")
```

####  随机森林


回归预测：$\hat{y} = \frac{1}{T}\sum_{t=1}^{T}f_t(x)$

分类预测：$\hat{y} = \text{mode}{f_1(x), f_2(x), \ldots, f_T(x)}$

梯度提升
残差计算：$r_{it} = -\left[\frac{\partial L(y_i, f(x_i))}{\partial f(x_i)}\right]{f(x)=f{t-1}(x)}$

模型更新：$f_t(x) = f_{t-1}(x) + \eta h_t(x)$


#### Gradient Boosting（XGBoost/LightGBM/CatBoost）

XGBoost目标函数：$\text{Obj}(\theta) = \sum_{i=1}^{n}L(y_i, \hat{y}i) + \sum{k=1}^{K}\Omega(f_k)$


**优点**：
- 精度极高，常在结构化数据竞赛中碾压深度学习
- 支持缺失值、类别特征（尤其是CatBoost）
- 训练速度快（尤其是LightGBM）

**缺点**：
- 容易过拟合（需仔细调参）
- 训练时不可并行（顺序训练）



| 特性     | XGBoost                | LightGBM               | CatBoost               |
| :------- | :--------------------- | :--------------------- | :--------------------- |
| 开发机构 | 陈天奇（华盛顿大学）   | 微软                   | Yandex                 |
| 主要优点 | 性能稳定，功能全面     | 训练速度快，内存占用低 | 类别特征处理优秀       |
| 排序算法 | 精确贪婪算法           | 基于梯度的单边采样     | 对称树结构             |
| 类别特征 | 需要编码               | 需要编码               | 原生支持               |
| 缺失值   | 自动处理               | 自动处理               | 自动处理               |
| 适用场景 | 中小型数据，精度要求高 | 大数据集，需快速训练   | 包含大量类别特征的数据 |



## 逻辑回归 vs 决策树

"逻辑回归算概率，决策树列规则；要量化用回归，要解释用树！"



场景：是否批准贷款

### 逻辑回归

```
信用分 × 0.6 + 收入 × 0.3 - 负债 × 0.4 > 阈值
```
“整体加权评分”


### 决策树

```
if 信用分 > 700:
    批准
elif 收入 > 30万 and 负债 < 20万:
    批准
else:
    拒绝
```

“规则判断”


