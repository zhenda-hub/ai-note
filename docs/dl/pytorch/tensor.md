
### 数据

#### Tensor

张量

神经网络的基础单元

| tensor          | 概念                       | 特点                                                  |
| --------------- | -------------------------- | ----------------------------------------------------- |
| leaf tensor     | 直接创建的张量             | grad_fn 为 None, 在反向传播后会保留 .grad 属性        |
| non-leaf tensor | 通过操作其他张量生成的张量 | grad_fn 为 操作函数 , 在反向传播时不会保留 .grad 属性 |

forward: leaf -> root
backward: root -> leaf

```python
# 增删改查

# 升降纬度
torch.squeeze()
torch.unsqueeze()
torch.permute() # 指定纬度顺序
```

#### Dataset 和 DataLoader

DataLoader 提供灵活加载Dataset的设置

数据划分

| 数据集                   | 作用                                                         | 占比    |
| ------------------------ | ------------------------------------------------------------ | ------- |
| 训练集（Training Set）   | 模型通过训练集学习特征和模式                                 | 70%-80% |
| 验证集（Validation Set） | 用于调整超参数、选择模型结构和监控模型在未见过的数据上的表现 | 10%-15% |
| 测试集（Test Set）       | 用于评估模型的最终性能                                       | 10%-15% |


#### 批量处理

神经网络的损失函数通常是一个非凸函数,也就是说它可能存在多个局部最优解

批量处理的随机性有助于模型跳出局部最小值，找到全局最小值。
减少内存占用

鞍点

噪声较大








### 损失函数

作用: 计算模型准确度的工具, (预测结果与真实值之间的差异)

不同任务类型的需求：

- 分类问题通常使用交叉熵损失Cross-Entropy Loss）:

  softmax: (0,1), 和为1
  $\text{Softmax}(x_{i}) = \frac{\exp(x_i)}{\sum_j \exp(x_j)}$

- 回归问题常用 均方误差（Mean Squared Error, MSE, L2损失）或 平均绝对误差（Mean Absolute Error, MAE, L1损失）

- 推荐系统可能使用排序损失等


### 优化器

autograd 和 Computation Graph

Computation Graph是一种有向无环图（DAG），用于表示张量之间的数学运算关系. 记录了张量之间的操作和依赖关系，支持自动求导功能

多次计算梯度时, 需要设置 retain_graph=True


更新模型参数

