## 模型训练流程

| 概念         | 类比         | 功能                                                                     | 数学表示                                        | 常见类型/示例 |
| ------------ | ------------ | ------------------------------------------------------------------------ | ----------------------------------------------- | ------------- |
| **前向传播** | 信息流动     | 从输入到输出的计算过程                                                   | -                                               | -             |
| **损失函数** | 误差衡量     | 衡量预测值与真实值之间的差异, 训练的目标是**最小化损失函数**             | 均方误差(MSE), 交叉熵损失(Cross-Entropy Loss)等 | -             |
| **反向传播** | 计算梯度     | 应用链式法则进行求导的过程, 最终目的是计算出**损失函数对每层参数的梯度** | -                                               | -             |
| **优化器**   | 参数更新工具 | 根据梯度**更新参数**                                                     | **SGD, Adam, RMSprop, Adagrad**等               | -             |


前向传播,得出结果 -> 对比答案, 计算损失 -> 反向传播, 获得梯度 -> 使用optimizer, 更新模型参数 

```python
model = Net()
optimizer = optim.Adam(model.parameters())

for epoch in range(epochs):
    # 1. 前向传播
    output = model(data)
    loss = criterion(output, target)
    
    # 2. 清零梯度
    optimizer.zero_grad()
    
    # 3. 反向传播: 计算梯度
    loss.backward()  
    # output.backward(torch.ones_like(output))
    
    # 4. 优化器: 更新参数
    optimizer.step()
```

## gpu训练

```python
net = Net()
if torch.cuda.is_available():
    net = net.cuda()

loss_fn = nn.CrossEntropyLoss()
if torch.cuda.is_available():
    loss_fn = loss_fn.cuda()


if torch.cuda.is_available():
    inputs = inputs.cuda()
    labels = labels.cuda()


torch.cuda.device_count()

```

## 模型保存加载

```python
import torch
import torchvision

# method1
vgg16 = torchvision.models.vgg16(weights=True)
torch.save(vgg16, '/app/output/vgg16.pth')

model = torch.load('/app/output/vgg16.pth', weights_only=False)
print(model)

# method2
torch.save(net.state_dict(), 'mlp.params')

clone = MLP()
clone.load_state_dict(torch.load('mlp.params'))
```
