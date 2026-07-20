
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
export CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1
```

## 使用方法

<https://code.claude.com/docs/zh-CN/>

```
!后面接命令
#后面写入CLAUDE.md
```

##### 并行开发

只有一个本地仓库， 让claude code多终端并行开发， 一个终端一个文件夹

缺点：开发完了，还需要重新安装依赖才能运行。。。

worktree

```bash
# 列出所有 worktree
git worktree list

git worktree add ../make-qr2-c1 c1

# 删除 worktree（用完后）
git worktree remove ../make-qr2-c1

# 清理过期的 worktree
git worktree prune

```

```bash
/init
/resume
/clear
```

shift tab 切换模式

##### skills

<https://claude.com/plugins>

<https://github.com/anthropics/skills>

```bash
/plugin marketplace add anthropics/claude-plugins-official
/plugin marketplace add anthropics/claude-code
/plugin marketplace add anthropics/skills


/plugin install document-skills@anthropic-agent-skills
/plugin install example-skills@anthropic-agent-skills
```

## notification

| 方式                     | 依赖终端        | 技术机制              | 能做什么                     | 跨终端性      | 适合谁                    |
| ------------------------ | --------------- | --------------------- | ---------------------------- | ------------- | ------------------------- |
| **iTerm2 (OSC 9)**       | iTerm2（macOS） | OSC 9 escape sequence | 系统级通知（macOS 通知中心） | ❌ 仅 iTerm2  | macOS + iTerm2 重度用户   |
| **Terminal Bell (`\a`)** | 几乎所有终端    | ASCII Bell 字符       | 发出“叮”一声 / 闪屏          | ✅ 极强       | 通用、最保底              |
| **iTerm2 w/ Bell**       | iTerm2          | Bell + iTerm2 增强    | 声音 + 通知/高亮             | ❌ 仅 iTerm2  | 想要「声音+可见提醒」     |
| **Kitty (OSC 99)**       | Kitty           | OSC 99 扩展           | 桌面通知                     | ❌ 仅 Kitty   | Linux / 跨平台 Kitty 用户 |
| **Ghostty (OSC 777)**    | Ghostty         | OSC 777 扩展          | 原生系统通知                 | ❌ 仅 Ghostty | 新潮终端玩家 😄           |

iTerm2、Kitty 和 Ghostty 都是 macOS（以及部分 Linux）上非常流行的终端模拟器（terminal emulator）

## subagents

自带的

- 写完代码 → Code Simplifier（先简化）
- 涉及架构 → Tech Lead（战略审查）

- 简化后 → Code Reviewer（检查质量）
- 质量OK → Security Reviewer（安全扫描）
- 用户界面 → UX Reviewer（体验检查）




/agents 能看到全部 agent 列表



## web search

配置mcp: brave,tavily,linkup
配置文件: ~/.claude/settings.json


<https://docs.firecrawl.dev/zh/developer-guides/mcp-setup-guides/claude-code>

```bash
claude mcp add firecrawl -e FIRECRAWL_API_KEY=fc-4fca76xxxxxxxxxxxxxxx -- npx -y firecrawl-mcp
claude mcp add firecrawl --url https://mcp.firecrawl.dev/your-api-key/v2/mcp
```

<https://docs.tavily.com/documentation/mcp>

```bash
claude mcp add --transport http tavily https://mcp.tavily.com/mcp/?tavilyApiKey=tvly-dev-xxxxxxxxxxxxxxxxxxz
```




todo: 手机指挥干活

## 更新cc:

```bash
claude update
```

## 体验

交互很多
会做todo
对话管理

慢，但是出来基本一遍或两边过
