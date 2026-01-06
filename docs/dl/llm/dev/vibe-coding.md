[toc]

## 工具

### cursor

```
curl i
```



### claude code


<https://claude.com/product/claude-code>


```bash
curl -fsSL https://claude.ai/install.sh | bash
```


```bash
claude --help

claude
```

### codex


### cline



## 模型


### deepseek

| type                         | price |
| :--------------------------- | :---- |
| 百万tokens输入（缓存命中）   | 0.2元 |
| 百万tokens输入（缓存未命中） | 2元   |
| 百万tokens输出               | 3元   |

<https://api-docs.deepseek.com/zh-cn/quick_start/pricing>

### glm

| 上下文（千tokens）                 | 输入（百万token） | 输出（百万token） | 缓存     | 缓存命中（百万token） | decode速度 |
| :--------------------------------- | :---------------- | :---------------- | :------- | :-------------------- | :--------- |
| 输入长度 [0, 32) 输出长度 [0, 0.2) | 2元               | 8元               | 限时免费 | 0.4元                 | 30-50      |
| 输入长度 [0, 32) 输出长度 [0.2+)   | 3元               | 14元              | 限时免费 | 0.6元                 | 30-50      |
| 输入长度 [32, 200)                 | 4元               | 16元              | 限时免费 | 0.8元                 | 30-50      |


<https://bigmodel.cn/pricing>



