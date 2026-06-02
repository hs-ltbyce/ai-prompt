---
title: Git 提交规范化技能
source: https://www.skills.sh/github/awesome-copilot/git-commit
tags: [git, commit, conventional-commits, workflow, skill]
tool: all
created: 2026-06-02
---

## 意图

帮助 AI 在执行 Git 提交时，基于实际变更内容生成符合 Conventional Commits 规范的提交信息。
该技能不仅用于“写提交信息”，也用于在提交前判断变更类型、识别影响范围、必要时重新组织暂存内容，并避免危险 Git 操作。

## 触发方式

当用户出现以下意图时可触发：
- 帮我提交当前改动
- 生成 commit message
- 按 Conventional Commits 提交
- /commit
- 帮我判断这次改动应该用 feat 还是 fix

## 核心内容

AI 在执行该技能时，应遵循以下流程。

### 步骤 1：分析当前变更

先检查仓库状态，再根据是否已暂存选择 diff 来源：
- 若已有 staged 内容，优先分析 staged diff。
- 若没有 staged 内容，则分析 working tree diff。
- 同时查看文件状态，确认本次提交涉及哪些文件。

分析目标包括：
- 这是新增能力、修复缺陷、文档更新还是重构。
- 影响范围属于哪个模块、目录或功能域。
- 这批改动是否属于“一个逻辑变更”。

### 步骤 2：必要时整理暂存内容

如果当前变更混杂了多个逻辑目的，AI 应优先建议按逻辑拆分提交，而不是把所有改动一次性提交。
可采用以下原则：
- 只把同一目标的文件放进同一提交。
- 对混合改动使用选择性暂存或交互式暂存。
- 明确提醒不要把敏感信息、私钥、凭据或环境文件提交进仓库。

### 步骤 3：生成 Conventional Commit 信息

提交信息格式应为：

```text
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

其中：
- type：根据改动性质选择，如 feat、fix、docs、refactor、test、chore 等。
- scope：可选，表示影响模块，如 api、auth、ui、docs。
- description：一句话概括改动，使用现在时和祈使语气，尽量控制在 72 个字符以内。

常见 type 选择建议：
- feat：新增功能。
- fix：修复缺陷。
- docs：仅文档变更。
- style：仅格式或样式调整，不影响逻辑。
- refactor：重构但不新增功能也不修复缺陷。
- perf：性能优化。
- test：新增或调整测试。
- build：构建系统、依赖或打包流程变更。
- ci：CI/CD 配置变更。
- chore：杂项维护。
- revert：撤销历史提交。

若存在破坏性变更，应显式标注 breaking change：
- 可在 type 或 scope 后添加 `!`。
- 或在 footer 中加入 `BREAKING CHANGE:` 说明。

### 步骤 4：执行提交

在提交前，AI 应先向用户展示准备使用的 commit message，确保信息与实际改动一致。
若改动较简单，可使用单行提交；若需要解释背景、兼容性影响或关联 issue，则补充 body 和 footer。

footer 可包含：
- `Closes #123`
- `Refs #456`
- `BREAKING CHANGE: ...`

## 行为约束

AI 在使用该技能时应遵守以下约束：
- 不要修改全局或本地 Git 配置，除非用户明确要求。
- 不要使用强制提交、强制推送、硬重置等破坏性命令。
- 不要绕过 hooks，例如 `--no-verify`，除非用户明确要求。
- 不要把多个无关改动压进同一个提交。
- 若提交因 hooks 或校验失败而中断，应先修复问题，再创建新的提交，而不是默认 amend。

## 最佳实践

- 每次提交只表达一个清晰的逻辑目的。
- 提交描述使用“做什么”，不要写成“已经做了什么”。
- scope 应尽量稳定、简洁，并贴近真实模块边界。
- 若改动影响外部行为、接口或迁移路径，应在 body 或 footer 中补充说明。
- 若无法准确判断 type，优先先解释判断依据，再生成候选提交信息供用户确认。

## 输出格式

建议输出采用以下结构：
1. 当前改动判断：本次变更属于什么类型，影响哪些范围。
2. 暂存建议：是否需要拆分提交，是否已有不应提交的文件。
3. 推荐提交信息：给出 1 个主推荐和可选备选。
4. 执行动作：在用户确认后执行提交命令。

## 适配版本

- [ ] copilot
- [ ] cursor
- [ ] claude-code
- [ ] codex

## 备注

来源为 GitHub awesome-copilot 的 git-commit skill，经中文整理后作为工具无关的 source 母本保存。