基于 Transformer 架构，通过在海量数据上的“预训练”和“微调”，学习如何预测序列中的下一个概率最大的词。

1. 核心架构：Transformer
Transformer 是几乎所有现代 LLM 的“发动机”。它在 2017 年由 Google 提出，彻底改变了 AI 处理语言的方式。

并行处理： 传统的模型（如 RNN）必须按顺序一个词一个词地处理，而 Transformer 可以同时处理整个句子。这使得它能在大规模并行硬件（GPU）上高效训练。

位置编码（Positional Encoding）： 由于 Transformer 同时读入所有词，它需要一套数学标签来标记单词在句子中的顺序，从而理解“狗咬人”和“人咬狗”的区别。

2. 魔法核心：自注意力机制 (Self-Attention)

如果说 Transformer 是发动机，自注意力机制就是它的“眼睛”。它允许模型在处理一个词时，自动“关注”句子中与其相关的其他部分，从而捕捉上下文的深层含义。

其数学表达通常通过查询（Query）、键（Key）和值（Value）来实现，公式如下：

$$Attention(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

3. RLHF（人类反馈强化学习） 是如何让模型说话更有“人味”的？

4. Token（词元） 是如何影响模型对中文和英文的处理效率的？

