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

##### 使用方法

<https://code.claude.com/docs/zh-CN/>

生成测试框架
添加有意义的测试用例
运行并验证测试


Claude Code 并行开发的核心是一个终端一个分支。

```bash
/init

/resume

```

shift tab 切换模式




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

```bash
# 典型工作流
1. 写完代码 → Code Simplifier（先简化）
2. 简化后 → Code Reviewer（检查质量）
3. 质量OK → Security Reviewer（安全扫描）
4. 涉及架构 → Tech Lead（战略审查）
5. 用户界面 → UX Reviewer（体验检查）
```

这些描述足够详细，可以让 AI 理解每个 agent 的具体职责和使用时机。你可以根据团队实际情况调整或补充细节。



```


web search

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





TODO:


agent
mcp
vibe-coding

office
browser

