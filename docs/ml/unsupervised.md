




特点：没有标准答案
目标：发现结构、模式、分布



## 典型问题



### 聚类


#### K-Means： 

K-Means是一种聚类算法，将数据点划分为K个互不重叠的簇，使得同一簇内的数据点尽可能相似，不同簇的数据点尽可能不同。

需要提前指定 K


```python
# K-Means 示例代码
import numpy as np
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans
from sklearn.datasets import make_blobs

# 生成示例数据
X, y = make_blobs(n_samples=300, centers=4, cluster_std=0.60, random_state=0)

# 应用K-Means
kmeans = KMeans(n_clusters=4, random_state=0)
y_kmeans = kmeans.fit_predict(X)

# 可视化结果
plt.scatter(X[:, 0], X[:, 1], c=y_kmeans, s=50, cmap='viridis')
centers = kmeans.cluster_centers_
plt.scatter(centers[:, 0], centers[:, 1], c='red', s=200, alpha=0.75, marker='X')
plt.title("K-Means 聚类结果")
plt.xlabel("特征 1")
plt.ylabel("特征 2")
plt.show()
```

### 降维: 用更少的维度，表达尽量多的信息

#### PCA（Principal Component Analysis）主成分分析

**核心思想**：
找到数据方差最大的方向（主成分），把高维数据投影到低维空间，同时尽可能保留原始信息。

```python
# 导入依赖库
import numpy as np
import matplotlib.pyplot as plt
from sklearn.datasets import load_iris
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA

# 1. 加载数据集（鸢尾花数据集，4维特征）
iris = load_iris()
X = iris.data
y = iris.target
feature_names = iris.feature_names
target_names = iris.target_names

# 2. 数据标准化
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# 3. 训练PCA模型，降维到2维
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X_scaled)

# 4. 可视化降维结果
plt.figure(figsize=(8, 6))
for target, color, name in zip([0, 1, 2], ['r', 'g', 'b'], target_names):
    plt.scatter(X_pca[y == target, 0], X_pca[y == target, 1], c=color, label=name)
plt.title('PCA: Iris Dataset (4D → 2D)')
plt.xlabel('Principal Component 1')
plt.ylabel('Principal Component 2')
plt.legend()
plt.show()

# 5. 查看主成分的方差解释率
print(f"方差解释率（前2主成分）: {pca.explained_variance_ratio_.sum():.2f}")
print(f"各主成分方差解释率: {pca.explained_variance_ratio_}")
```


```python
# PCA 示例代码
import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA
from sklearn.datasets import load_iris

# 加载数据
iris = load_iris()
X = iris.data
y = iris.target

# 应用PCA
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X)

# 可视化降维结果
plt.figure(figsize=(8, 6))
colors = ['navy', 'turquoise', 'darkorange']
target_names = iris.target_names

for color, i, target_name in zip(colors, [0, 1, 2], target_names):
    plt.scatter(X_pca[y == i, 0], X_pca[y == i, 1], 
                color=color, alpha=.8, lw=2, label=target_name)

plt.legend(loc='best', shadow=False, scatterpoints=1)
plt.title('PCA: 鸢尾花数据集 (4D -> 2D)')
plt.xlabel('第一主成分 (解释方差: %.2f%%)' % (pca.explained_variance_ratio_[0] * 100))
plt.ylabel('第二主成分 (解释方差: %.2f%%)' % (pca.explained_variance_ratio_[1] * 100))
plt.show()


# 查看每个主成分解释的方差比例
print("各主成分解释方差比例:", pca.explained_variance_ratio_)
print("累计解释方差比例:", np.cumsum(pca.explained_variance_ratio_))

# 可视化累计解释方差
plt.figure(figsize=(10, 4))

plt.subplot(1, 2, 1)
plt.bar(range(1, len(pca.explained_variance_ratio_) + 1), 
        pca.explained_variance_ratio_ * 100)
plt.xlabel('主成分')
plt.ylabel('解释方差比例 (%)')
plt.title('各主成分解释方差')

plt.subplot(1, 2, 2)
plt.plot(range(1, len(pca.explained_variance_ratio_) + 1), 
         np.cumsum(pca.explained_variance_ratio_) * 100, 'bo-')
plt.axhline(y=95, color='r', linestyle='--', alpha=0.5, label='95% 方差')
plt.xlabel('主成分数量')
plt.ylabel('累计解释方差 (%)')
plt.title('累计解释方差')
plt.legend()
plt.tight_layout()
plt.show()
```


### Embedding + 相似度（现代最常用）

**核心思想**：
1. 把高维稀疏数据（文字 / 图片 / 音频、用户行为、商品等）映射成低维稠密的向量（Embedding）
2. 在向量空间中，语义/行为相似的对象距离很近
3. 用相似度（如余弦相似度）衡量两个向量之间的相似程度


余弦相似度（Cosine Similarity）：
$$
\cos(\theta) = \frac{A \cdot B}{\|A\| \|B\|}
$$
取值范围 [-1, 1]，值越大越相似。



```python
# 安装依赖：pip install sentence-transformers scikit-learn
from sentence_transformers import SentenceTransformer
from sklearn.metrics.pairwise import cosine_similarity

# 1. 加载预训练的Sentence-BERT模型
model = SentenceTransformer('all-MiniLM-L6-v2')

# 2. 定义待处理的文本
sentences = [
    "人工智能是未来科技的核心",
    "AI技术正在改变各行各业",
    "苹果的最新手机搭载了A17芯片",
    "机器学习是人工智能的一个分支",
    "华为发布了新款折叠屏手机"
]

# 3. 生成文本Embedding
embeddings = model.encode(sentences)
print(f"Embedding向量维度: {embeddings.shape}")  # (5, 384)

# 4. 计算余弦相似度矩阵
similarity_matrix = cosine_similarity(embeddings)
print("余弦相似度矩阵：")
print(similarity_matrix.round(2))

# 5. 查找与目标句子最相似的文本
target_sentence = "人工智能的发展离不开机器学习"
target_embedding = model.encode([target_sentence])
similarities = cosine_similarity(target_embedding, embeddings)[0]
most_similar_idx = similarities.argmax()
print(f"\n与「{target_sentence}」最相似的文本：{sentences[most_similar_idx]}")
print(f"相似度值：{similarities[most_similar_idx]:.2f}")
```

