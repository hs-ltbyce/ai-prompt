---
title: Superpowers 完整开发方法论框架
source: https://github.com/obra/superpowers
tags: [methodology, skills-framework, agentic-development, test-driven, code-generation]
created: 2026-05-26
---

## 意图

Superpowers 是一个完整的 AI 编程助手框架和方法论，提供标准化的 Skills 库和开发流程，支持多个大型语言模型工具（Claude Code、GitHub Copilot、Cursor 等）。通过统一的工作流（头脑风暴 → 使用 Git Worktree → 编写计划 → 执行计划 → 测试驱动开发 → 代码审查 → 完成分支）来规范 AI 辅助编程的全生命周期。

## 概述

Superpowers 是基于 Agent-Driven Development 的完整软件开发方法论，包括：

- **Skills Library**：预定义的可复用工作流 Skill，覆盖测试、调试、协作、规划等领域
- **多工具支持**：兼容 Claude Code、GitHub Copilot CLI、Cursor Agent、Codex CLI/App、Factory Droid、Gemini CLI、OpenCode 等
- **统一工作流**：强制执行标准化的开发流程，确保代码质量和一致性

## 核心工作流（7 个阶段）

### 1. 头脑风暴阶段（Brainstorming）
- 在开始编码前激活，通过 Socratic 对话方法细化需求
- 输出清晰的设计文档，分段检查验证

### 2. 并行分支准备（Using Git Worktrees）
- 设计批准后激活，创建隔离的工作区
- 运行项目安装，验证测试基线清洁

### 3. 制定计划（Writing Plans）
- 将设计分解为 2-5 分钟的微任务
- 每个任务包含：确切的文件路径、完整代码、验证步骤

### 4. 执行计划（Executing Plans / Subagent-Driven Development）
- 自动化执行或分派子 Agent 逐项处理
- 两阶段审查（规范合规性 → 代码质量）

### 5. 测试驱动开发（Test-Driven Development）
- 强制 RED-GREEN-REFACTOR 循环
- 先写失败的测试，再实现最小代码，最后重构

### 6. 代码审查（Requesting Code Review）
- 针对实现计划进行审查
- 按严重程度报告问题，关键问题阻止进度

### 7. 完成分支（Finishing a Development Branch）
- 验证所有测试通过
- 提供选项：合并/PR/保留/丢弃
- 清理 Worktree

## 关键 Skills 库

### 测试
- `test-driven-development` - RED-GREEN-REFACTOR 循环 + 反模式参考

### 调试
- `systematic-debugging` - 4 阶段根因分析
- `verification-before-completion` - 完成前验证

### 协作
- `brainstorming` - Socratic 设计细化
- `writing-plans` - 详细实施计划
- `executing-plans` - 批量执行 + 检查点
- `dispatching-parallel-agents` - 并发子 Agent 工作流
- `requesting-code-review` - 前置审查检查清单
- `receiving-code-review` - 反馈应对流程
- `using-git-worktrees` - 并行开发分支
- `finishing-a-development-branch` - 合并/PR 决策流程
- `subagent-driven-development` - 快速迭代 + 两阶段审查

### 元
- `writing-skills` - 创建新 Skill 的最佳实践指南
- `using-superpowers` - 框架系统介绍

## 核心理念

- **测试优先**：始终先写测试
- **系统化胜于临时方案**：流程优于猜测
- **复杂度减少**：简洁性是首要目标
- **证据优于声称**：验证后再宣布成功
- **YAGNI**：不要实现你觉得将来可能需要的功能
- **DRY**：不要重复代码

## 收藏价值

- 完整的方法论模板，可参考学习 AI 编程流程设计
- Skills 编写规范，对自建 Skill 库有借鉴意义
- 多工具兼容的实现方式，展示不同平台的适配策略

## 参考资源

- **官方仓库**：https://github.com/obra/superpowers
- **许可**：MIT
- **社区**：[Discord 频道](https://discord.gg/35wsABTejz)
- **原始发布**：https://blog.fsck.com/2025/10/09/superpowers/
