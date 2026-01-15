[toc]

## RAG


RAG，全称Retrieval-Augmented Generation，中文译为检索增强生成。它是一种结合了信息检索和生成模型的技术，旨在让大语言模型（LLM）能够访问和利用**外部知识库**中的信息，从而生成更准确、更全面的回答。





bge-m3 已经非常强了
chunk 大小非常重要，建议从 400~600 token 开始调



## 为什么需要 RAG？

### 1️⃣ 大模型本身的局限
大语言模型存在以下问题：

- ❌ **不知道你的私有数据**
- ❌ **知识有时间截止**
- ❌ **容易胡编（Hallucination）**
- ❌ **无法溯源答案来源**

---

### 2️⃣ RAG 解决了什么？

| 问题 | RAG 的解决方式 |
|---|---|
| 不懂你的数据 | 用你的文档做检索 |
| 知识过时 | 数据随时可更新 |
| 胡编乱造 | 只基于检索内容回答 |
| 不可解释 | 可返回引用来源 |





## 应用场景

非常适合 私有数据
不能胡编
需要可解释答案

- 学术研究助手
- 法律/医疗咨询
- 个人知识库：管理读书笔记、日程规划（比如 “提醒我明天给妈妈打电话”）
- 公司客服机器人：让机器人记住产品手册，自动回答客户咨询（比如 “产品 A 怎么退款？”）
- 行业资料问答：上传 PDF 论文、行业报告，让大模型总结核心内容（比如 “这篇论文的研究方法是什么？”）
- 智能办公助手：上传公司规章制度，自动回答员工问题（比如 “年假怎么申请？”）



## RAG 的核心组成部分


| 组件           | 作用（大白话）                           | 通俗比喻        | 常用工具（小白友好）             |
| :------------- | :--------------------------------------- | :-------------- | :------------------------------- |
| 大模型         | 最终生成回答（核心大脑）                 | 学霸            | ChatGPT、文心一言、通义千问      |
| Embedding 模型 | 把文字转换成「数字向量」（让电脑能看懂） | 文字→数字翻译官 | OpenAI Embedding、通义 Embedding |
| 向量数据库     | 存储「数字向量」（长期记忆硬盘）         | 专属笔记文件夹  | Chroma（轻量）、Milvus（开源）   |
| 检索器         | 从向量数据库里找相关资料                 | 翻笔记的手      | LangChain（Python 框架）         |


rag步骤

| 步骤	    |  关键性	      |  为什么重要 |
|------------|---------------|---|
| 文本切分	| ⭐⭐⭐⭐⭐	  | 决定 chunk 的质量，影响检索和回答效果 |
| 嵌入模型	| ⭐⭐⭐⭐	    | 决定语义理解质量，影响相似度召回 |
| 问答逻辑	| ⭐⭐⭐	      |  决定 prompt 怎么写，能不能让 LLM 发挥最优 |
| 向量索引	| ⭐⭐	        |  标准化组件，只影响性能、可扩展性 |
| 文档加载	| ⭐	          |   一次性任务，只要能读出干净文本即可 |
| 调用模型	| ⭐	          |    技术细节，LLM 换哪个都行，影响较小 |




### 文本切分（Chunking）

大模型不能一次读完整文档，需要切分。

常见方式：
- 固定长度（如 500 字）
- 滑动窗口
- 按标题 / 语义切分

⚠️ **切得太大 → 找不准**  
⚠️ **切得太小 → 上下文不足**


### 嵌入模型

常用向量模型：
- OpenAI Embedding
- BGE
- text-embedding-3-large
- 本地模型（如 bge-large-zh）

<https://blog.csdn.net/weixin_43589681/article/details/139269119>

### 向量数据库（Vector Store）

用于存储和检索向量。

常见选择：
- FAISS（本地，轻量）
- Chroma（简单）
- Milvus（企业级）
- Qdrant


Pinecone：云服务，易用但收费
Chroma：开源，轻量级，适合本地开发
Milvus：开源，功能强大，适合生产环境
Faiss：Facebook开源，性能极高
Weaviate：开源，功能全面

📌 **学习阶段：FAISS / Chroma 就够了**


## 常见问题



1.检索不准确
问题：检索到的文档与问题无关
解决方案：

优化文本分割策略
使用更好的Embedding模型
尝试混合检索
增加检索结果数量后再精选

1. 答案不基于检索内容
问题：模型忽视检索内容，自己编造答案
解决方案：

在提示词中强调"仅基于参考信息"
使用更遵循指令的模型
要求模型引用来源

3. 性能问题
问题：响应速度慢
解决方案：

使用更快的向量数据库（如Faiss）
减少检索结果数量
使用更小的Embedding模型
考虑缓存常见问题

4. 长文档处理
问题：单个文档太长，超出模型上下文窗口
解决方案：

分层检索（先找章节，再找段落）
Map-Reduce方法（分块总结，再汇总）
使用支持长上下文的模型



```python
# 安装依赖
# pip install openai chromadb

import openai
import chromadb

# 1. 初始化向量数据库
client = chromadb.Client()
collection = client.create_collection("my_docs")

# 2. 添加文档到知识库
documents = [
    "Python是一种高级编程语言，语法简洁易学。",
    "RAG技术结合了检索和生成，能让AI获取最新知识。",
    "向量数据库用于存储和检索文本的向量表示。"
]

# 自动生成ID和向量
collection.add(
    documents=documents,
    ids=["doc1", "doc2", "doc3"]
)

# 3. 检索相关文档
def retrieve(query, n_results=2):
    results = collection.query(
        query_texts=[query],
        n_results=n_results
    )
    return results['documents'][0]

# 4. 生成答案
def rag_query(question):
    # 检索相关文档
    context_docs = retrieve(question)
    context = "\n".join(context_docs)
    
    # 构建提示词
    prompt = f"""基于以下参考信息回答问题：

参考信息：
{context}

问题：{question}

请基于参考信息准确回答，如果参考信息中没有相关内容，请说明。"""
    
    # 调用大模型
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": prompt}]
    )
    
    return response.choices[0].message.content

# 5. 使用
answer = rag_query("什么是RAG？")
print(answer)
```


```python
# 1. 导入需要的工具
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.vectorstores import Chroma
from langchain.chains import RetrievalQA
from langchain.llms import OpenAI
from dotenv import load_dotenv
import os

# 2. 加载API密钥（从.env文件中读取）
load_dotenv()
openai_api_key = os.getenv("OPENAI_API_KEY")

# 3. 准备「专属笔记」（你的私有数据）
# 这里可以替换成你的个人笔记、公司文档等文字内容
my_data = [
    "我叫小明，今年25岁，在互联网公司做产品经理",
    "我的生日是10月1日，喜欢打篮球和看电影",
    "我最近在学习RAG技术，想搭建一个个人知识库",
    "我的手机号是13800138000（仅用于示例）"
]

# 4. 初始化Embedding模型（文字→向量翻译官）
embeddings = OpenAIEmbeddings(openai_api_key=openai_api_key)

# 5. 初始化向量数据库（长期记忆硬盘），并把笔记存进去
# persist_directory：向量数据存在本地的目录（自动创建）
db = Chroma.from_texts(
    texts=my_data,
    embedding=embeddings,
    persist_directory="./chroma_db"  # 数据存在当前目录的chroma_db文件夹
)
db.persist()  # 保存数据（持久化，下次运行不会丢失）

# 6. 初始化检索器（翻笔记的手）
retriever = db.as_retriever(
    search_kwargs={"k": 2}  # 每次检索最多返回2条最相关的资料
)

# 7. 组合检索器和大模型，搭建RAG完整流程
rag_chain = RetrievalQA.from_chain_type(
    llm=OpenAI(openai_api_key=openai_api_key, temperature=0),  # temperature=0：回答更准确，不胡说
    chain_type="stuff",  # 简单理解：把检索到的资料直接传给大模型
    retriever=retriever,
    return_source_documents=True  # 让系统返回回答的参考资料（方便验证）
)

# 8. 测试RAG系统
def ask_rag(question):
    result = rag_chain({"query": question})
    print("回答：", result["result"])
    print("参考资料：", [doc.page_content for doc in result["source_documents"]])
    print("-" * 50)

# 测试几个问题
ask_rag("我叫什么名字？")
ask_rag("我的生日是什么时候？")
ask_rag("我最近在学习什么技术？")

```