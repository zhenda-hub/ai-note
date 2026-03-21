## links

- <https://openclaw.ai/>
- <https://docs.openclaw.ai/start/getting-started>
- <https://docs.openclaw.ai/tools/browser>
- <https://docs.openclaw.ai/tools/skills>

## 配置

~/.openclaw/openclaw.json

```bash
openclaw gateway status
openclaw gateway restart

```

## 浏览器自动化

使用OpenClaw原生浏览器功能

## 联网搜索

自己可以搜索

<https://api-dashboard.search.brave.com/app/keys>

## skills

https://help.apiyi.com/openclaw-skill-recommendations-2026.html

## 用手机连接

飞书

<https://bytedance.larkoffice.com/docx/MFK7dDFLFoVlOGxWCv5cTXKmnMh>

```bash
npx -y @larksuite/openclaw-lark install

openclaw config set channels.feishu.streaming true
```

### 发图片

### 定时任务

## 没有记忆功能

## 自定义skill

## linux部署的OpenClaw 迁移步骤

1. 导出配置和数据

```
# 备份配置
cat ~/.openclaw/config.yaml > ~/openclaw-config-backup.yaml
# 备份整个工作区（最重要）
tar -czf ~/openclaw-workspace-backup.tar.gz ~/.openclaw/workspace
```

2. 在新设备上安装 OpenClaw

```
pnpm add -g openclaw
```

3. 恢复配置和 workspace

```
# 恢复配置
cp ~/openclaw-config-backup.yaml ~/.openclaw/config.yaml

# 恢复 workspace
tar -xzf ~/openclaw-workspace-backup.tar.gz -C ~/
```

4. 重启 Gateway

```
openclaw gateway restart
```

关键点：

- Workspace 包含所有记忆文件（SOUL.md、USER.md、MEMORY.md 等），这是你的"灵魂"
- 配置文件包含 API 密钥和集成设置
- 建议定期备份 workspace，特别是在重要对话后
