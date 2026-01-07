[toc]

## 编程工具

### ide

#### cursor

```
curl i
```

$20


#### cline

开源免费

### cli

#### claude code


<https://claude.com/product/claude-code>


```bash
claude --help

claude
```

自定义接入的模型

```bash
# 编辑或新增 Claude Code 配置文件 `~/.claude/settings.json`
{
    "env": {
        "ANTHROPIC_AUTH_TOKEN": "your_zhipu_api_key",
        "ANTHROPIC_BASE_URL": "https://open.bigmodel.cn/api/anthropic",
        "API_TIMEOUT_MS": "3000000",
        "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": 1
    }
}


{
    "env": {
        "ANTHROPIC_AUTH_TOKEN": "<ARK_API_KEY>",
        "ANTHROPIC_BASE_URL": "https://ark.cn-beijing.volces.com/api/coding",
        "ANTHROPIC_MODEL": "<Model>"
    }
}

```


```bash
export ANTHROPIC_BASE_URL=
export ANTHROPIC_AUTH_TOKEN=
export ANTHROPIC_MODEL=
export API_TIMEOUT_MS=600000
```


#### codex


#### opencode

开源免费

```bash
opencode auth login
opencode
```

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



### doubao-seed-code


coding plan


| 上下文（千tokens）  | 输入（百万token） | 输出（百万token） | 缓存     | 缓存命中（百万token） | decode速度 |
| :------------------ | :---------------- | :---------------- | :------- | :-------------------- | :--------- |
| 输入长度 [0, 32)    | 1.2元             | 8元               | 限时免费 | 0.4元                 | 30-50      |
| 输入长度 [32, 128)  | 1.4元               | 12元              | 限时免费 | 0.6元                 | 30-50      |
| 输入长度 [128, inf) | 2.8元               | 16元              | 限时免费 | 0.8元                 | 30-50      |

<https://console.volcengine.com/ark/region:ark+cn-beijing/model/detail?Id=doubao-seed-code>