[toc]
## 原理

让AI做复杂项目失败率高，做简单功能失败率低。为什么？如何提交AI做复杂项目的成功率？

### 原因

- 上下文窗口 ≠ 长期记忆 没有记住早期决策
- 隐性前提没有给AI说清楚：未来要扩展, 技术栈偏好等问题
- 误差累积效应 (Compound Error)：
如果每步准确率是 0.95，那么经过 10 个连续环节后，整体成功率将降至 0.5987。
一旦中间某一步生成的代码或逻辑有误，后续所有工作都会基于这个“错误的基石”构建，导致最终崩盘。
- 缺乏全局架构思维：AI 本质上是**概率预测引擎**，它擅长“续写”局部代码，但不擅长“规划”软件架构。它不知道如何平衡可扩展性、低耦合，可维护，可测试。往往会给出“能跑通但很混乱”的方案。

### 解决方案

提升成功率的关键：让模型对抗，以模治模！！！！！！！！！！！！！！

Q: 单步准确率是90%, 完成一步测试一步，有异常仅限修改一次， 修改的准确率也是90%。 一共有10步，求总体准确率?

| 单步原始准确率 | 是否允许一次修改 | 单步最终成功率 | 10 步总体成功率   |
| ------- | -------- | ------- | ----------- |
| 95%     | ❌        | 95%     | ≈ 60.0%     |
| 95%     | ✅        | 99.75%  | ≈ 97.5%     |
| **90%** | ❌        | **90%** | **≈ 34.9%** |
| **90%** | **✅**    | **99%** | **≈ 90.4%** |


#### System Prompt（系统提示）也就是memory（记忆）功能

长期的默认的提示内容

例如claude code 里的 CLAUDE.md： 它的核心作用是在每次对话开始时自动被 Claude Code 读取，用来给 Claude 提供关于你项目的上下文和偏好设置。

可以生成系统提示：

```txt
# 添加代码设计原则；软件开发流程采用TDD；编码前，先使用worktree切换新分支
```


##### 代码设计原则

```txt
KISS 原则（Keep It Simple, Stupid）
单一职责原则（Single Responsibility Principle，SRP）
Explicit is Better Than Implicit (显式优于隐式)
```

##### 软件开发流程

测试驱动开发(TDD)： 先让 AI 写测试用例，确保测试用例覆盖了所有需求点。再写功能代码，通过测试用例。
文档驱动开发 (Documentation-Driven Development)

workflow: branch-> plan -> test ->src ->review ->merge ->cleanup


```txt
现在需要开发功能：xxx。使用worktree,切换新分支.梳理计划。
根据plan，编写测试用例，确保测试用例覆盖所有需求点。
根据plan，编写代码实现功能，通过测试用例。 

提交代码，合并代码到main分支，清理这个worktree和分支
```

#### openspec

todo...

最后还是需要人类监督和验证。AI 本质上是**概率预测引擎**，没有百分百的确定性


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

#### cline

```bash

npm install -g cline
cline auth
cline
```

频繁提问

#### kilo code

```bash
npm install -g @kilocode/cli
kilocode
```

闷头干，停不下来

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
export CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1
```

##### 使用方法

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
<https://github.com/anthropics/skills>

```bash
/plugin marketplace add anthropics/skills

/plugin install document-skills@anthropic-agent-skills
/plugin install example-skills@anthropic-agent-skills
```

##### notification



| 方式                       | 依赖终端          | 技术机制                  | 能做什么              | 跨终端性        | 适合谁                  |
| ------------------------ | ------------- | --------------------- | ----------------- | ----------- | -------------------- |
| **iTerm2 (OSC 9)**       | iTerm2（macOS） | OSC 9 escape sequence | 系统级通知（macOS 通知中心） | ❌ 仅 iTerm2  | macOS + iTerm2 重度用户  |
| **Terminal Bell (`\a`)** | 几乎所有终端        | ASCII Bell 字符         | 发出“叮”一声 / 闪屏      | ✅ 极强        | 通用、最保底               |
| **iTerm2 w/ Bell**       | iTerm2        | Bell + iTerm2 增强      | 声音 + 通知/高亮        | ❌ 仅 iTerm2  | 想要「声音+可见提醒」          |
| **Kitty (OSC 99)**       | Kitty         | OSC 99 扩展             | 桌面通知              | ❌ 仅 Kitty   | Linux / 跨平台 Kitty 用户 |
| **Ghostty (OSC 777)**    | Ghostty       | OSC 777 扩展            | 原生系统通知            | ❌ 仅 Ghostty | 新潮终端玩家 😄            |




iTerm2、Kitty 和 Ghostty 都是 macOS（以及部分 Linux）上非常流行的终端模拟器（terminal emulator）




##### subagents

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


- plan
- web search

mcp
手机指挥干活


##### 体验

交互很多
会做todo
对话管理


慢，但是出来基本一遍或两边过

#### codex


#### opencode

开源免费

```bash
opencode auth login
opencode

```


<https://opencode.ai/docs/troubleshooting/#copypaste-not-working-on-linux>
```bash
# fix: linux copypaste
sudo apt install -y wl-clipboard
```

##### 使用方法

<https://opencode.ai/docs>

/init
生成 AGENTS.md

Plan mode 


设置prompt

```
You are a senior software engineer.

Before writing code:
1. Analyze the existing codebase
2. Identify the minimal change required
3. Explain your plan briefly

When writing code:
- Only modify relevant files
- Do not refactor unrelated code
- Do not introduce new dependencies
- Ensure code is runnable

If unsure:
- Ask for clarification instead of guessing

```
##### 体验

交互少
会做todo，
有对话记录

有测试代码
但写出来总是有问题， 需要反复三五次的来回改

可以查看token使用量

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






## mcp

office
browser
