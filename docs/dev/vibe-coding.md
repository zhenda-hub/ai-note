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

| 单步原始准确率 | 是否允许一次修改 | 单步最终成功率 | 10 步总体成功率 |
| -------------- | ---------------- | -------------- | --------------- |
| 95%            | ❌               | 95%            | ≈ 60.0%         |
| 95%            | ✅               | 99.75%         | ≈ 97.5%         |
| **90%**        | ❌               | **90%**        | **≈ 34.9%**     |
| **90%**        | **✅**           | **99%**        | **≈ 90.4%**     |

#### System Prompt（系统提示）也就是memory（记忆）功能

长期的默认的提示内容

例如claude code 里的 CLAUDE.md： 它的核心作用是在每次对话开始时自动被 Claude Code 读取，用来给 Claude 提供关于你项目的上下文和偏好设置。

可以生成系统提示：

```txt
# 添加代码设计原则

# 编码前必须向用户确认分支，确认是否使用worktree， 确认是否使用编写测试用例，确认测试用例的详细程度
# 严禁自动merge代码，一定要向用户确认。
```

prompt:

```txt
创建用户级别的CLAUDE.md
更新 用户级别的CLAUDE.md 文档，避免下次犯同样的错误
你是否已经读取了用户级别的CLAUDE.md？

现在需要开发功能：xxx。使用worktree,切换新分支.梳理计划。
根据plan，编写测试用例，确保测试用例覆盖所有需求点。
根据plan，编写代码实现功能，通过测试用例。

提交代码，合并代码到main分支，清理这个分支和worktree

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

workflow: plan -> branch ->src -> test(op) ->merge ->cleanup

#### openspec

```bash
openspec init

/opsx:explore
/opsx:new       Start a new change
/opsx:continue  Create the next artifact
/opsx:ff        ff
/opsx:apply     Implement tasks
/opsx:verify    Verify the change
/opsx:archive   Archive this change
```

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
#### cc

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

### minimax

<https://platform.minimaxi.com/subscribe/token-plan>

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
| 输入长度 [32, 128)  | 1.4元             | 12元              | 限时免费 | 0.6元                 | 30-50      |
| 输入长度 [128, inf) | 2.8元             | 16元              | 限时免费 | 0.8元                 | 30-50      |

<https://console.volcengine.com/ark/region:ark+cn-beijing/model/detail?Id=doubao-seed-code>

## mcp

office
browser
