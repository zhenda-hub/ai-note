## links

- <https://openclaw.ai/>
- <https://docs.openclaw.ai/start/getting-started>
- <https://docs.openclaw.ai/tools/browser>
- <https://docs.openclaw.ai/tools/skills>

## 开始使用

```bash
# gateway启动
openclaw gateway status
openclaw gateway restart

# update
openclaw update

# 修bug
openclaw doctor --fix

# 安全检查
openclaw security audit --deep
openclaw security audit --fix
```

配置文件: ~/.openclaw/openclaw.json

## 疑难杂症

<--- JS stacktrace --->

FATAL ERROR: Reached heap limit Allocation failed - JavaScript heap out of memory
----- Native stack trace -----

## 具体使用

### 斜杠命令

<https://clawd.org.cn/tools/slash-commands.html>

/help
/commands
/status（显示当前状态； 包括当前模型提供商的用量/配额（如果可用））

/compact 压缩上下文（保留要点，大幅减少 Token 消耗） ⭐⭐⭐
/stop
/restart

/model 查看当前正在使用的模型
/skills 查看当前已加载的所有技能
/sessions list 列出所有历史会话

聊天斜杠命令详解

一、会话管理（最常用 ⭐⭐⭐）

命令 功能说明 使用频率
/new 开始全新会话（清除上下文，节省 Token） ⭐⭐⭐
/new 任务描述 新建会话并直接带上任务 ⭐⭐⭐
/compact 压缩上下文（保留要点，大幅减少 Token 消耗） ⭐⭐⭐
/status 查看当前会话状态（Token 用量、模型信息） ⭐⭐
/help 或 /commands 显示所有可用斜杠命令 ⭐⭐

💡 使用技巧：

长对话后 Token 消耗剧增？输入 /compact 立即瘦身！
想换个话题？/new 一键清空，比手动删除快 10 倍！

二、模型切换（多模型对比测试）

命令 功能说明
/model 查看当前正在使用的模型
/model <模型名> 实时切换到指定模型

示例：

/model claude-3-5-sonnet
/model gpt-4o
/model qwen-max

三、会话历史管理

命令 功能说明
/sessions list 列出所有历史会话
/sessions history 查看当前会话的详细历史
/sessions send <会话ID> <消息> 向指定会话发送消息（跨会话通信）
/sessions spawn <任务> 创建子会话执行独立任务

四、工具与执行控制

命令 功能说明 使用场景
/approve 批准待确认的操作 Agent 请求权限时确认
/deny 拒绝待确认的操作 阻止危险操作
/cancel 取消当前执行中的任务 任务卡死或想中断时
/undo 撤销上一步操作 误操作回滚

五、技能与记忆管理

命令 功能说明
/skills 查看当前已加载的所有技能
/memory 查看 AI 的记忆内容（长期记忆）
/forget <内容> 删除指定的记忆条目

六、信息查询

命令 功能说明
/cost 查看本次会话的 Token 消耗和费用估算
/version 查看当前 OpenClaw 版本
/ping 测试与网关的连接是否正常

### 浏览器自动化

使用OpenClaw原生浏览器功能

### 联网搜索

自己可以搜索

<https://api-dashboard.search.brave.com/app/keys>

### skills

- https://help.apiyi.com/openclaw-skill-recommendations-2026.html
- https://clawhub.ai/

- tavily-search网络搜索
- summarize对 URL 或文件进行汇总
- agent-browser模拟浏览器行为
- find-skills发现并安装 Skill
- github与 GitHub 进行交互
- obsidian同步 Obsidian
- notion管理页面、数据库和区块
- weather获取天气和天气预报
- tencentcloud-lighthouse-skill管理腾讯云 Lighthouse
- tencent-docs腾讯文档操作能力

#### 自定义skill

当一个功能做了很久终于实现, 让它总结为一个skill.方便下次使用

### 用手机连接

1. 飞书

<https://bytedance.larkoffice.com/docx/MFK7dDFLFoVlOGxWCv5cTXKmnMh>

```bash
npx -y @larksuite/openclaw-lark install

openclaw config set channels.feishu.streaming true
```

2. qq

https://cloud.tencent.com/developer/article/2626045

体验不好,不能选择文字copy

#### 发图片

#### 定时任务

### 没有记忆功能

## linux部署的OpenClaw 迁移步骤

### export

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

### import
