## lmstudio


## ollama

### 导入lmstudio的gguf模型


```txt
FROM D:\models\lmstudio-community\Qwen3.5-9B-GGUF\Qwen3.5-9B-Q4_K_M.gguf
PARAMETER num_ctx 8192
SYSTEM You are a helpful AI assistant.
```

```pwsh

ollama create qwen35_9b -f D:\models\modelfiles\qwen35_9b.modelfile
```


```
http://localhost:11434/v1
```
