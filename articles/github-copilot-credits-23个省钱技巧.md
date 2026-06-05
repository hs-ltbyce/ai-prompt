# GitHub Copilot AI Credits 时代的 23 个省钱技巧

> 原文链接：https://zenn.dev/yuzu_krs/articles/cb7ea9d1dbb02d
>
> 作者：yuzu-krs
>
> 发布日期：2026/06/03

---

## 前言

GitHub Copilot 已从以往以「Premium Request」为核心的机制，转变为基于 GitHub AI Credits 的按量计费模式。

从今往后，成本不再仅取决于「使用次数」，还会受以下因素影响：

- 使用了哪个模型
- 读取了多少输入内容
- 生成了多少输出内容
- Agent 使用了多少文件和工具
- 包含了多长的聊天记录和多大的文件

海外也有不少反馈，比如「几天就消耗了大量积分」「让 Agent 自由探索会很贵」「经常使用高性能模型，积分消耗飞快」。

本文整理了在使用 GitHub Copilot / GitHub Copilot Chat / VS Code Copilot 时，避免浪费 AI Credits 的实践规则。

---

## 结论

GitHub Copilot 省钱的核心在于以下四点：

```
不让它多读
不让它多说
不滥用高能力模型
不让 Agent 过度自由行动
```

重点不是不使用 AI，而是**控制好传递给 AI 的信息范围**。

---

## 最终检查清单

首先是供自己参考的检查清单：

```
优先使用 Ask 模式
使用轻量模型
使用 Auto 模式
不要过度提高 Thinking effort
指定目标文件
不要随意使用 #codebase
每开始一个新任务就新建聊天
同一任务过长时使用 /compact
尝试其他方案使用 /fork
仅在必要时使用 Agent
关闭不需要的 tools / MCP
通过 .gitignore / files.exclude 排除巨大文件
在 memory.md 中简要总结，供下次聊天读取
先用 M365 Copilot / ChatGPT 等工具打磨好 prompt
善用代码补全和 Next Edit Suggestions
不要频繁使用 Copilot Code Review
为 Custom Agent 配置轻量模型和最少必要 tools
用 /context 确认导致沉重的源头
注意 prompt cache，使用固定模板
安装 gh CLI，减少 MCP tools 的浪费
CLI review 前切换到轻量模型
尝试 /chronicle:cost-tips
通过 Agent Debug Logs 排查沉重原因
```

无需每次都全部执行。

尤其有效的是以下几个：

```
指定目标文件
新建聊天
仅在必要时使用 Agent
善用轻量模型 / Auto 模式
用 memory.md 简短交接
```

---

## 1. 优先使用 Ask 模式

如果是单次提问或轻度咨询，使用 Ask 模式而非 Agent 模式。

Ask 模式足以应对的工作包括：

- 询问错误的含义
- 咨询实现方案
- 请 AI 解释部分代码
- 确认 SQL 或正则表达式
- 征求小函数的改进建议

Agent 模式会进行文件搜索、文件编辑、命令执行、工具调用等操作。

因此，用 Agent 模式处理简单问题，多余的上下文（文件、工具输出等）容易造成 AI Credits 浪费。

**反面示例：**

```
用 Agent 调查这个错误的原因
```

**正面示例：**

```
Ask mode。
简短说明一下这个错误的原因。
暂时不要编辑文件。
```

---

## 2. 使用轻量模型

日常工作中，轻量模型或 mini 系列通常已经足够。

| 工作内容 | 推荐模型 |
|---|---|
| 修改 README | 轻量模型 |
| 添加注释 | 轻量模型 |
| UI 微调 | 轻量模型 |
| 小函数实现 | 轻量 ~ mini |
| 添加测试 | mini |
| CRUD 实现 | mini |
| 认证·支付·数据库设计 | 中等 ~ 高能力模型 |
| 安全审查 | 高能力模型 |
| 原因不明的疑难 bug | 高能力模型 |

不要把所有任务都丢给最强模型，像下面这样分工更容易省钱：

```
轻量模型 → 实现
高能力模型 → 设计 · 审查
```

---

## 3. 使用 Auto 模式

如果觉得手动选择模型麻烦，使用 Auto 模式也是一个选择。

Auto 模式会根据任务自动选择平衡性较好的模型，日常使用比较稳妥。

推荐的分工如下：

```
日常：Auto
明显简单：固定使用轻量模型
复杂设计·调试：固定使用高能力模型
```

不过，企业用户可能存在「公司内部固定了可用模型」的情况。

此时，安全的做法是按公司策略使用允许的模型，而非使用 Auto。

---

## 4. 不要过度提高 Thinking effort

提高 Thinking effort 或 Reasoning effort，模型会进行更深度的思考，相应地 token 消耗和等待时间也会增加。

日常使用默认值就够了。

建议仅在以下场景提高：

- 架构设计
- 安全设计
- 复杂 bug 调查
- 跨多个文件的难度重构
- 支付、认证等不容失败的修改

从省钱角度来看，平时就设为 high 不太推荐。

---

## 5. 指定目标文件

这一点非常重要。

**反面示例：**

```
看一下整个仓库，把问题修掉
```

**正面示例：**

```
Target only: src/auth/login.ts
只修复登录失败时的错误处理。
不要碰其他文件。
```

更安全的做法是，先只让 AI 输出计划：

```
先只输出修改计划。
暂时不要编辑。
```

基本原则是，不要一上来就让 AI 看整个仓库，而是只指定需要的文件。

---

## 6. 不要随意使用 `#codebase`

在 VS Code Copilot 中，可以通过 `#codebase` 或文件指定来传递上下文。

虽然方便，但随意使用 `#codebase`，可能导致读取多余的文件。

**反面示例：**

```
#codebase 全体を見ていい感じに改善して
```

**正面示例：**

```
只看 #src/auth/login.ts
需要时参考 #src/auth/session.ts
```

基本原则是：

```
在给 AI 看全部内容之前，先缩小目标文件范围
```

---

## 7. 每开始一个新任务就新建聊天

这一条非常有效。

聊天变长后，过去的对话、文件内容、工具输出等都会作为上下文累积。

同一任务可以继续，但要切换到别的任务时，新建聊天更省钱。

**可以在同一聊天继续的示例：**

```
登录功能修复
↓
同一个登录修复的测试追加
↓
同一个修复的类型错误处理
```

**建议新建聊天的示例：**

```
登录功能修复
↓
编写 README
↓
修改付费页面
```

切换到不同任务时，不拖带上一个上下文，成本更低。

---

## 8. 同一任务过长时使用 `/compact`

即使是同一任务，如果聊天变长了，可以使用 `/compact` 压缩对话。

示例：

```
/compact 只保留当前 bug、修改文件和剩余 TODO，其余压缩
```

完全不同的任务 → 新建聊天；同一任务但聊天长了 → `/compact` 很方便。

---

## 9. 尝试其他方案使用 `/fork`

在同一任务中想尝试不同方案时，`/fork` 很方便。

在同一个聊天中混合多种方案，上下文容易变得混杂。

使用区分如下：

```
完全不同的任务 → 新建聊天
同一任务的不同方案 → /fork
同一任务聊天变长 → /compact
```

---

## 10. 仅在必要时使用 Agent

Agent 模式很方便，但从省钱角度来看不建议经常使用。

适合 Agent 的工作：

- 跨多个文件的实现
- 含测试执行的修改
- 需要调查原因的 bug
- 需要文件探索的工作
- 需要命令执行的工作

相反，以下场景用 Ask 或 Edit 就够了：

- 单个文件的小修改
- 询问错误的含义
- 咨询实现方案
- 代码片段改进
- 注释或 README 修改

Agent 虽然方便，但让它自由探索成本较高，因此要控制使用场景。

---

## 11. 关闭不需要的 tools / MCP

Agent 可用的工具过多时，可能增加无谓的工具调用。

工具调用的结果也会消耗上下文，因此想省钱时可以关闭不需要的 tools 或 MCP 服务器。

示例：

```
本次需要的 tools:
- filesystem 即可

不需要:
- terminal
- browser
- database
- 外部 MCP
```

建议只在必要时启用。

企业用户应注意 MCP 与公司政策和管理员设置的关系，不要随意添加外部 MCP。

---

## 12. 通过 `.gitignore` / `files.exclude` 排除巨大文件

构建产物和巨大文件通常让 AI 读取没有意义。

建议排除的对象：

```
node_modules/
dist/
build/
.next/
coverage/
*.log
*.map
*.min.js
```

不过，lock file 和生成文件有时也有需要的情况。

做法是：平时不让 AI 读，需要时再单独指定。

---

## 13. 在 `memory.md` 中简要总结，供下次聊天读取

`memory.md` 不是 Copilot 的内部记忆，而是放在仓库中的普通 Markdown 文件。

推荐位置：

```
docs/ai-memory.md
```

目的：

```
让 AI 读取长聊天记录
↓
只让 AI 读取简短的 memory.md
↓
节省 AI Credits
```

需要注意，`memory.md` 本身也会被计入上下文。

但比起让 AI 读取整个长聊天记录，读取简短摘要的成本更低。

### `docs/ai-memory.md` 示例

```markdown
# AI Memory

## Current Status
- Main features:
- Current task:
- Recent changes:
- Next steps:

## Important Rules
- Do not scan the whole repository unless needed.
- Do not refactor unrelated files.
- Keep changes small and focused.
- Prefer Ask mode for questions.
- Use Agent mode only when multi-file edits or investigation are needed.

## Last Work Summary
-

## Next Task
-
```

### 工作结束时的 prompt

```
Update docs/ai-memory.md with:
- what changed
- changed files
- remaining TODO
Keep it short.
```

### 下一次新建聊天的开头

```
Cost-saving mode.
Read docs/ai-memory.md first.
Target only: <file or directory>.
First output plan only.
Do not edit yet.
```

---

## 14. 先用 M365 Copilot / ChatGPT 等工具打磨好 prompt

在随意丢给 Copilot 之前，先用其他 AI 或记事本整理好 prompt 也很有效。

**粗糙 prompt：**

```
好像有 bug，修一下
```

**整理后的 prompt：**

```
Cost-saving mode.

Target only: src/auth/login.ts

Problem:
- 登录失败时异常被吞噬

Goal:
- 把错误内容返回给 UI
- 不改变已有的成功处理逻辑

Rules:
- Do not scan the whole repo
- Do not refactor unrelated files
- First output plan only
```

打磨好 prompt 再提交，可以减少 Copilot 侧的探索和追问。

但企业用户需要格外注意：

不要将公司内部代码、客户信息、故障日志、个人信息等粘贴到未经公司批准的 AI 服务中。

安全表述应为：

```
仅在已获公司批准的 AI 环境中，且不包含机密信息的前提下整理 prompt
```

---

## 15. 善用代码补全和 Next Edit Suggestions

Code completions 和 Next Edit Suggestions 不消耗 AI Credits。

因此，比起让 Chat 或 Agent 全部代写，更好的方式是：

```
自己写函数名、类型、注释
↓
通过补全让 AI 补完后续
```

示例：

```
// Calculate monthly usage percentage and return warning level
function calculateUsageWarningLevel(
```

写到这里，让补全接手。

与其「全部让 AI 生成」，不如「自己确定方向，让补全来填充」，成本更低。

---

## 16. 不要频繁使用 Copilot Code Review

Copilot Code Review 很方便，但会消耗 AI Credits。

此外，视情况还会消耗 GitHub Actions minutes。

想省钱的话，注意以下几点：

- 仅对重要的 PR 使用
- 不要用于巨大 PR
- 不要包含生成文件
- 不要包含 lock file 或构建产物
- 先自己过一遍 diff

AI 审查很方便，但不能完全替代安全审查和最终质量保证。

---

## 17. 为 Custom Agent 配置轻量模型和最少必要 tools

如果不想每次都手动选择模型和 tools，可以按用途创建 Custom Agent。

示例：

```
docs-agent      → 轻量模型 / 无 tools
ui-agent        → mini 系列 / 仅 filesystem
review-agent    → 中等模型 / 以 read-only 为主
security-agent  → 高能力模型 / 仅在必要时使用
```

关键是针对每个 Agent 限制以下内容：

```
使用的模型
可用的 tools
目标任务
输出格式
```

把 Agent 做成万能的，最终成本反而容易增加。

Custom Agent 应以「小且专业化」为原则。

---

## 18. 用 `/context` 确认导致沉重的源头

在 Copilot CLI 等环境中，可以用 `/context` 查看当前上下文。

```
/context
```

查看要点：

```
是对话记录太沉重
还是文件读取太沉重
还是 MCP tools 太沉重
还是输出太冗长
```

在不知道哪里沉重的情况下盲目省钱，不如先找到原因再对症下药。

---

## 19. 注意 prompt cache，使用固定模板

与其每次随意更换开头的 prompt 和指令，不如使用固定模板更稳定。

**应避免的：**

```
每次放不同的冗长前言
放入大量当天心情或闲聊
放入与任务无关的说明
```

**推荐的：**

```
使用固定的简短模板
保持 instructions.md 简短稳定
统一请求格式
```

与其说刻意瞄准 prompt cache，不如理解为**每次保持 prompt 简短稳定**更容易理解。

---

## 20. 安装 gh CLI，减少 MCP tools 的浪费

在使用 Copilot CLI 或 GitHub MCP 的场景下，拥有 GitHub CLI 环境会更便利。

```
gh --version
```

CLI 和 MCP 相关操作存在环境差异，不一定适合所有人。

但对基于 CLI 使用 Copilot 的人来说，这有助于梳理 GitHub 相关操作。

企业用户应提前确认公司电脑能否安装 CLI、能否使用 MCP。

---

## 21. CLI review 前切换到轻量模型

在 CLI 中使用 review 相关命令时，先切换到轻量模型或 Auto 可能更省钱。

示例：

```
/model Auto
/review
```

或：

```
/model <轻量模型>
/review
```

但安全、支付相关的 review 过度依赖轻量模型可能在质量上有风险。

重要 review 中，不仅要考虑成本，也要考虑安全性。

---

## 22. 尝试 `/chronicle:cost-tips`

某些环境下，可以运行根据最近使用情况给出省钱建议的命令。

```
/chronicle:cost-tips
```

或根据环境可能为以下形式：

```
/chronicle cost tips
```

版本和环境可能导致不可用，抱着「能用就试试」的态度即可。

---

## 23. 通过 Agent Debug Logs 排查沉重原因

面向进阶用户，查看 Agent Debug Logs 可以确认 token 和工具调用在何处增长。

查看要点：

- tool call 是否过多
- 是否读取了不必要的文件
- 输出是否过长
- cache 是否生效
- 是否反复进行相同调查

在「最近 AI Credits 突然减少得很快」时很有帮助。

---

## 实际使用的模板

### 日常提问

```
Ask mode.
Auto model.
简短回答。
```

### 实现之前

```
Cost-saving mode.

Target only: <file or directory>
Do not scan the whole repository.
Do not use #codebase.
Do not refactor unrelated files.
First output target files and plan only.
Do not edit yet.
```

### 实现时

```
Implement only the approved plan.
No unrelated refactor.
Keep output concise.
Use lightweight model if possible.
```

### 工作结束时

```
Update docs/ai-memory.md with:
- what changed
- changed files
- remaining TODO
Keep it short.
```

### 下一次新建聊天

```
Cost-saving mode.
Read docs/ai-memory.md first.
Target only: <file or directory>.
First output plan only.
Do not edit yet.
```

---

## 企业用户的注意事项

企业用户不能仅考虑节省 AI Credits，还需要考虑信息泄露、审计和公司内部规则的合规性。

需要特别注意以下几点：

```
先用 M365 Copilot / ChatGPT 等打磨 prompt
添加外部 MCP
使用未经公司批准的模型
```

这些行为虽然方便，但涉及公司代码和日志的传输问题。

在文章或公司内部推广时，应遵循以下安全原则：

```
仅限在已获公司批准的 AI 环境中使用
不将机密信息、客户信息、源代码粘贴到未经批准的 AI 中
仅在管理员已许可的环境中使用 CLI / MCP / Custom Endpoint
```

---

## 总结

GitHub Copilot 的 AI Credits 时代，不是「不使用 AI」的时代，而是**管理好传递给 AI 的信息量**的时代。

尤其重要的是：

```
优先使用 Ask 模式
使用轻量模型 / Auto 模式
指定目标文件
不要随意使用 #codebase
每开始一个新任务就新建聊天
仅在必要时使用 Agent
用 memory.md 简短交接
```

个人认为，最应该封杀的 prompt 是这个：

```
全部读一遍，看着办吧
```

今后应该采用以下方式：

```
缩小目标范围
将计划与实现分开
不拖带冗长的历史记录
```

这种做法不仅能节省 AI Credits，也能提升实现质量。

---

> 原文链接：https://zenn.dev/yuzu_krs/articles/cb7ea9d1dbb02d
>
> 作者：yuzu-krs
