---
title: AI 编程插件与 Compound Engineering Plugin 介绍
source: https://github.com/EveryInc/compound-engineering-plugin
tags: [plugin, compound-engineering, cursor, claude-code, skill, ai-编程]
created: 2026-07-07
language: chinese
---

# AI 编程插件与 Compound Engineering Plugin 介绍

## 一、什么是 AI 编程插件（Plugin）

在 Cursor、Claude Code、Codex、GitHub Copilot 等 AI 编程工具中，**插件（Plugin）** 是一种可安装、可复用的能力扩展包。它把一组预定义的指令、工作流和领域知识打包在一起，让 AI Agent 在对话中按固定方法论执行任务，而不必每次从零描述完整流程。

### 插件解决什么问题

传统用法里，开发者需要反复编写长提示词：「先分析需求、再写计划、再实现、再 review……」每次换项目或换任务都要重来。插件把这类**可复用的工程实践**沉淀为标准化模块，安装后即可通过斜杠命令（如 `/ce-plan`）一键触发。

### 插件通常包含什么

| 组成部分 | 作用 |
|---------|------|
| **Skills** | 可触发的技能命令，每个 Skill 对应一套完整工作流（如规划、实现、审查） |
| **Agents / Personas** | 可选的专项子代理或评审角色，用于多视角分析 |
| **References** | 参考文档、模板、脚本，供 Skill 运行时按需加载 |
| **Manifest** | 插件元数据，声明名称、版本、兼容的工具平台 |

### 插件与 Rules、Skills 的关系

- **Rules**：持久约束 AI 行为的规则（代码风格、提交规范等），始终生效
- **Skills**：按需触发的任务型工作流，用户或 AI 在特定场景下主动调用
- **Plugin**：把多个 Skills（及配套资源）打包分发，支持跨工具安装

插件本质是 **Skills 的产品化分发形态**——一次安装，全团队共享同一套工程方法论。

### 主流工具的插件机制

| 工具 | 安装方式示例 |
|------|-------------|
| **Cursor** | Agent 对话中 `/add-plugin <name>`，或从插件市场搜索安装 |
| **Claude Code** | `/plugin marketplace add <repo>` → `/plugin install <plugin>` |
| **Codex** | CLI 或 App 中添加自定义 Marketplace 源后安装 |
| **GitHub Copilot** | VS Code 命令面板「Install Plugin from Source」 |
| **OpenCode / Pi / Kimi 等** | 各工具通过 JSON 配置或 CLI 从 GitHub 仓库直接加载 |

安装后，插件内的 Skills 会注册为斜杠命令，AI 在匹配场景下也会自动识别并加载对应 Skill。

---

## 二、Compound Engineering Plugin 介绍

[Compound Engineering Plugin](https://github.com/EveryInc/compound-engineering-plugin) 是由 [Every](https://every.to) 团队维护的官方插件，面向 Claude Code、Codex、Cursor、GitHub Copilot 等多平台，目前 GitHub 星标超过 2 万。其核心口号是：

> **AI skills that make each unit of engineering work easier than the last.**  
> 让每一次工程工作，都比上一次更容易。

### 核心理念：复利工程（Compound Engineering）

传统开发会积累技术债：每加一个功能，复杂度上升；每修一个 bug，局部知识留在某人脑子里，下次还得重新摸索。代码库越大，上下文越难把握，改动越慢。

**复利工程**反转这一趋势，主张 **80% 投入规划与审查，20% 投入执行**：

- 用 `/ce-brainstorm`、`/ce-plan` 充分规划，再写代码
- 用 `/ce-code-review`、`/ce-doc-review` 审查，校准判断
- 用 `/ce-compound` 把经验固化为文档，供下次复用
- 保持代码质量，让后续改动始终容易

目标不是增加仪式，而是获得**杠杆**：好的 brainstorm 让 plan 更准，好的 plan 让实现更小，好的 review 抓住模式而不只是 bug，好的 compound 笔记让下一个 Agent 不必从零学习。

### 核心工作流（六步闭环）

```
brainstorm → plan → work → simplify → review → compound → 重复（上下文更聪明）
```

| Skill | 用途 |
|-------|------|
| `/ce-brainstorm` | 交互式探索需求，产出需求文档，再进入规划 |
| `/ce-plan` | 将需求转化为可执行的实现计划 |
| `/ce-work` | 按计划系统实现，支持 worktree 隔离与任务追踪 |
| `/ce-simplify-code` | 实现后精简代码，提升可读性与复用性 |
| `/ce-code-review` | 多 Agent 视角对照计划做代码审查 |
| `/ce-compound` | 将本次学习写入 `docs/solutions/`，供下次循环读取 |

每一轮 `/ce-compound` 产出的知识，会被下一轮 `/ce-brainstorm` 和 `/ce-plan` 作为上下文读取——**复利循环的回流箭头正是这个插件的设计精髓**。

### 典型使用场景

**标准功能开发：**

```text
/ce-brainstorm 让后台任务重试更安全
/ce-plan
/ce-work
/ce-simplify-code
/ce-code-review
/ce-compound
```

**尚无明确方向时，先发散再收敛：**

```text
/ce-ideate 新的绘图工具
/ce-brainstorm   # 承接 ideate 的最优候选
/ce-plan
...
```

**调试 Bug（跳过 brainstorm/plan，直达根因）：**

```text
/ce-debug checkout webhook 有时会创建重复发票
/ce-code-review
/ce-compound
```

**全自动流水线（规划后放手）：**

```text
/ce-brainstorm 描述功能需求
/lfg
```

`/lfg` 会自动执行：规划 → 实现 → 简化 → 审查并修复 → 浏览器测试 → 提交 → 推送 → 开 PR → 监控 CI 直至通过。适合规划清楚后暂时离开、回来时 PR 已绿。

### 完整 Skill 清单（29 个）

插件当前内置 **29 个 Skills**，覆盖从战略到交付的全链路：

| 分类 | Skills |
|------|--------|
| **战略与方向** | `/ce-strategy`、`/ce-ideate`、`/ce-pov` |
| **规划与执行** | `/ce-brainstorm`、`/ce-plan`、`/ce-work`、`/ce-worktree` |
| **质量保障** | `/ce-simplify-code`、`/ce-code-review`、`/ce-doc-review`、`/ce-debug` |
| **知识沉淀** | `/ce-compound`、`/ce-compound-refresh`、`/ce-explain` |
| **Git 与 PR** | `/ce-commit`、`/ce-commit-push-pr`、`/ce-resolve-pr-feedback` |
| **测试与验证** | `/ce-test-browser`、`/ce-test-xcode`、`/ce-dogfood`、`/ce-polish` |
| **运营与反馈** | `/ce-product-pulse`、`/ce-promote`、`/ce-riffrec-feedback-analysis`、`/ce-sweep` |
| **工具与自动化** | `/ce-setup`、`/ce-optimize`、`/ce-proof`、`/lfg` |

专项评审、调研等行为内嵌在各 Skill 的本地 prompt 资源中，不依赖独立 Agent 包。

### 安装方式

**Cursor：**

```text
/add-plugin compound-engineering
```

或在插件市场搜索 "compound engineering"。

**Claude Code：**

```text
/plugin marketplace add EveryInc/compound-engineering-plugin
/plugin install compound-engineering
```

**Codex CLI：**

```bash
codex plugin marketplace add EveryInc/compound-engineering-plugin
codex plugin add compound-engineering@compound-engineering-plugin
```

**GitHub Copilot（VS Code）：**

1. 命令面板运行 `Chat: Install Plugin from Source`
2. 仓库填 `EveryInc/compound-engineering-plugin`
3. 选择 `compound-engineering` 插件

安装后，在任意项目中运行 `/ce-setup` 检查仓库配置与可选工具能力。

### 技术架构特点

- **Root-native、Skills-only 布局**：仓库根目录即插件包，`skills/` 目录存放所有 Skill 定义
- **跨平台兼容**：同一份仓库支持 Claude Code、Cursor、Codex、Copilot、OpenCode、Pi、Kimi 等，各工具通过 manifest 或转换层加载
- **无需 Bun 安装**：普通用户直接从 GitHub / 插件市场安装；Bun 仅用于仓库开发与测试
- **知识文件约定**：计划输出到 `docs/plans/`，学习沉淀到 `docs/solutions/`，战略文档为 `STRATEGY.md`，形成项目级复利知识库

### 与其他方法论的关系

Compound Engineering 与 [Superpowers](https://github.com/obra/superpowers) 等 Agent 方法论有相似之处——都强调规划先行、TDD、系统化审查。差异在于：

- Compound Engineering 以 **插件 + 斜杠命令** 形式深度集成多工具生态
- 强调 **`/ce-compound` 知识回流**，让每次迭代都读取历史 solutions
- 提供 `/lfg` 等端到端自动化流水线，覆盖从 brainstorm 到绿 PR 的全流程

### 延伸阅读

- [Skill 文档目录](https://github.com/EveryInc/compound-engineering-plugin/blob/main/docs/skills/README.md)
- [Compound engineering: how Every codes with agents](https://every.to/chain-of-thought/compound-engineering-how-every-codes-with-agents)
- [The story behind compounding engineering](https://every.to/source-code/my-ai-had-already-fixed-the-code-before-i-saw-it)

---

## 小结

AI 编程插件把可复用的工程方法论打包为可安装的能力扩展；**Compound Engineering Plugin** 是其中体系最完整、跨平台支持最广的开源方案之一。它用「规划 → 执行 → 审查 → 沉淀」的复利循环，让 Agent 不只写代码，而是持续积累项目知识，使后续每一次开发都比上一次更轻松。
