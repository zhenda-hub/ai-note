
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

```

我帮你为每个 agent 生成详细的描述。这些描述可以直接用于配置 AI agents：

---

## **Code Reviewer**

**What it does:**
This agent conducts thorough code reviews focusing on code quality, best practices, and maintainability. It examines code for logical errors, potential bugs, adherence to coding standards, proper error handling, and overall code structure. The agent provides constructive feedback with specific suggestions for improvement, identifies anti-patterns, checks for proper documentation and comments, evaluates naming conventions, assesses code readability and maintainability, and ensures consistency with the project's existing codebase style.

**When to use:**
- Before merging pull requests or commits into main branches
- After completing a feature implementation or bug fix
- When refactoring existing code to ensure quality improvements
- During onboarding new team members to establish code quality standards
- When introducing new architectural patterns or libraries
- Before code releases to catch potential issues early
- When reviewing code written by junior developers who need mentorship
- After implementing complex algorithms or business logic that requires validation
- When you want a second opinion on your implementation approach

---

## **Code Simplifier**

**What it does:**
This agent specializes in reducing code complexity and improving readability without changing functionality. It identifies overly complex functions, nested conditionals, and convoluted logic, then suggests simpler alternatives. The agent looks for opportunities to extract reusable functions, eliminate code duplication (DRY principle), reduce cognitive load, simplify conditional logic using early returns or guard clauses, break down large functions into smaller, focused units, remove unnecessary abstractions or over-engineering, suggest more intuitive variable and function names, and identify areas where modern language features could simplify code.

**When to use:**
- When code becomes difficult to understand or maintain
- After rapid prototyping that resulted in messy implementation
- When onboarding new developers who struggle with complex code
- Before adding new features to legacy code that needs simplification first
- When technical debt has accumulated and refactoring is needed
- After code reviews that flag complexity issues
- When preparing code for handoff to other teams or open source release
- When performance is adequate but code clarity is poor
- When functions exceed reasonable line counts (e.g., 50+ lines)
- Before writing tests for complex code that would benefit from simplification first

---

## **Security Reviewer**

**What it does:**
This agent performs comprehensive security analysis to identify vulnerabilities, security anti-patterns, and potential attack vectors. It checks for common security issues including SQL injection vulnerabilities, XSS (Cross-Site Scripting) risks, CSRF protection, authentication and authorization flaws, insecure data storage or transmission, hardcoded secrets or credentials, improper input validation and sanitization, insecure dependencies or outdated libraries, exposure of sensitive data in logs or error messages, insufficient rate limiting or DoS protection, insecure cryptographic implementations, and compliance with security standards (OWASP Top 10, etc.). The agent provides remediation guidance with secure code examples.

**When to use:**
- Before deploying any code that handles user data or authentication
- When implementing payment processing or financial transactions
- After adding new API endpoints or external integrations
- Before releasing features that handle sensitive information (PII, health data, etc.)
- When updating dependencies or third-party libraries
- After code changes to authentication or authorization logic
- When implementing file uploads or user-generated content features
- Before security audits or compliance assessments
- When receiving security-related bug reports
- Regularly as part of CI/CD pipeline for continuous security scanning
- When integrating with external services or APIs
- Before open-sourcing internal code

---

## **Tech Lead**

**What it does:**
This agent provides high-level technical guidance from an architectural and strategic perspective. It evaluates whether the implementation aligns with project goals and system architecture, assesses scalability and performance implications, reviews technology choices and their appropriateness, identifies potential architectural issues or technical debt, ensures consistency with established patterns and conventions, evaluates the impact on system reliability and maintainability, provides guidance on cross-cutting concerns (logging, monitoring, error handling), assesses whether the solution is over-engineered or under-engineered, reviews API design and interface contracts, and considers long-term maintenance implications and team knowledge distribution.

**When to use:**
- When designing new features or major system components
- Before making significant architectural decisions or technology choices
- When evaluating multiple implementation approaches
- When planning technical roadmaps or sprint priorities
- After completing major features to assess if they meet architectural standards
- When technical decisions have business or product implications
- When dealing with performance or scalability concerns
- Before committing to significant refactoring efforts
- When introducing new frameworks, libraries, or tools to the stack
- When resolving technical disagreements within the team
- When estimating complexity and effort for upcoming work
- When reviewing proposals for infrastructure changes
- Before making breaking API changes

---

## **UX Reviewer**

**What it does:**
This agent evaluates code from a user experience perspective, focusing on how implementation decisions affect end-user interaction and satisfaction. It reviews UI/UX implementation quality, accessibility compliance (WCAG standards, ARIA labels, keyboard navigation), responsive design and cross-device compatibility, performance impact on user experience (load times, rendering), error messages and user feedback clarity, form validation and user input handling, loading states and async operation feedback, consistency with design systems and UI patterns, user flow logic and navigation intuitiveness, and mobile-first or touch-friendly interactions. The agent identifies friction points in user journeys and suggests improvements that balance technical constraints with user needs.

**When to use:**
- When implementing new user-facing features or interfaces
- Before releasing features that significantly change user workflows
- When fixing bugs that affect user experience
- After receiving user complaints or negative feedback
- When implementing forms, wizards, or multi-step processes
- Before accessibility audits or compliance reviews
- When optimizing page load times or perceived performance
- When implementing error handling or validation logic
- When designing mobile or responsive layouts
- After usability testing reveals issues
- When adding animations, transitions, or interactive elements
- When implementing features for users with disabilities
- Before major UI refactoring or design system migrations
- When localizing or internationalizing user interfaces

---

**使用建议：**

你可以根据实际需求组合使用这些 agents：


1. 写完代码 → Code Simplifier（先简化）
2. 简化后 → Code Reviewer（检查质量）
3. 质量OK → Security Reviewer（安全扫描）
4. 涉及架构 → Tech Lead（战略审查）
5. 用户界面 → UX Reviewer（体验检查）


这些描述足够详细，可以让 AI 理解每个 agent 的具体职责和使用时机。你可以根据团队实际情况调整或补充细节。



```

## web search

配置mcp: brave,tavily,linkup
配置文件: ~/.claude/settings.json


<https://docs.firecrawl.dev/zh/developer-guides/mcp-setup-guides/claude-code>

```bash
claude mcp add firecrawl -e FIRECRAWL_API_KEY=fc-4fca7660b3434136a2548dc94c2e176f -- npx -y firecrawl-mcp
claude mcp add firecrawl --url https://mcp.firecrawl.dev/your-api-key/v2/mcp
```

<https://docs.tavily.com/documentation/mcp>

```bash
claude mcp add --transport http tavily https://mcp.tavily.com/mcp/?tavilyApiKey=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
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
