---
title: Git Worktree 隔离工作区技能
source: https://www.skills.sh/obra/superpowers/using-git-worktrees
tags: [agent-workflows, git, worktree, parallel-development, isolation, skill]
tool: all
created: 2026-06-15
---

## 意图

在需要与当前工作区隔离的功能开发、或执行实施计划之前，确保 AI 在独立的工作区中操作。
优先使用平台原生 worktree 工具；无原生能力时回退到 `git worktree`，并自动完成目录选择、安全校验、依赖安装与测试基线验证。

## 触发方式

当用户出现以下意图时可触发：
- 开始新功能开发，需要与当前分支隔离
- 多 Agent 并行开发，各自需要独立工作目录
- 执行实施计划前，先准备干净的工作环境
- 创建 worktree / 隔离分支 / 并行分支
- Superpowers 工作流进入「并行分支准备」阶段

安装命令：

```bash
npx skills add https://github.com/obra/superpowers --skill using-git-worktrees
```

## 核心原则

1. **先检测是否已隔离**：已在 linked worktree 中时，禁止再嵌套创建。
2. **原生工具优先**：平台提供 `EnterWorktree`、`/worktree`、`--worktree` 等能力时，必须使用原生工具，不要手动 `git worktree add`。
3. **Git 作兜底**：仅在没有原生 worktree 工具时使用 `git worktree`。
4. **不对抗 harness**：已有隔离机制时，服从平台，不重复造轮子。

## 核心行为

### 步骤 0：检测现有隔离状态

执行检测命令：

```bash
GIT_DIR=$(cd "$(git rev-parse --git-dir)" 2>/dev/null && pwd -P)
GIT_COMMON=$(cd "$(git rev-parse --git-common-dir)" 2>/dev/null && pwd -P)
BRANCH=$(git branch --show-current)
```

**子模块守卫**：`GIT_DIR != GIT_COMMON` 在子模块中也为真。需额外检查：

```bash
git rev-parse --show-superproject-working-tree 2>/dev/null
```

若返回路径，说明在子模块内，按普通仓库处理，而非已隔离 worktree。

**判断逻辑：**
- `GIT_DIR != GIT_COMMON` 且非子模块 → 已在 linked worktree，跳过创建，直接进入项目 setup。
- `GIT_DIR == GIT_COMMON` 或处于子模块 → 普通 checkout，需询问用户是否创建隔离 worktree（除非用户已有明确偏好）。

### 步骤 1：创建隔离工作区

#### 1a. 原生 Worktree 工具（首选）

若平台提供 worktree 创建能力，直接使用并跳到步骤 3。原生工具会自动处理目录、分支与清理；手动 `git worktree add` 会产生 harness 无法管理的 phantom 状态。

#### 1b. Git Worktree 兜底

**目录选择优先级**（用户显式偏好始终最高）：

1. 用户指令中已声明的 worktree 目录
2. 项目内已有目录：`.worktrees/`（优先）或 `worktrees/`
3. 全局遗留路径：`~/.config/superpowers/worktrees/<project>/`
4. 默认：项目根目录 `.worktrees/`

**安全校验**（仅项目本地目录）：

```bash
git check-ignore -q .worktrees 2>/dev/null || git check-ignore -q worktrees 2>/dev/null
```

若未被 ignore，必须先写入 `.gitignore` 并提交，再创建 worktree。否则 worktree 内容可能被误提交进仓库。

**创建命令：**

```bash
project=$(basename "$(git rev-parse --show-toplevel)")
git worktree add "$path" -b "$BRANCH_NAME"
cd "$path"
```

若因沙箱权限失败，告知用户并在当前目录继续工作。

### 步骤 3：项目 Setup

按项目类型自动检测并安装依赖：

| 检测文件 | 命令 |
|---|---|
| `package.json` | `npm install` |
| `Cargo.toml` | `cargo build` |
| `requirements.txt` | `pip install -r requirements.txt` |
| `pyproject.toml` | `poetry install` |
| `go.mod` | `go mod download` |

### 步骤 4：验证干净基线

运行项目对应测试命令（`npm test` / `cargo test` / `pytest` / `go test ./...`）。

- 测试失败：报告失败项，询问是否继续或先排查。
- 测试通过：报告就绪状态。

**就绪报告格式：**

```text
Worktree ready at <full-path>
Tests passing (<N> tests, 0 failures)
Ready to implement <feature-name>
```

## 行为约束

AI 在使用该技能时应遵守以下约束：

- 禁止在 Step 0 已检测到隔离时再创建嵌套 worktree。
- 禁止在有原生 worktree 工具时仍使用 `git worktree add`。
- 禁止跳过项目本地目录的 `.gitignore` 校验。
- 禁止在基线测试失败时未经确认继续开发。
- 禁止假设 worktree 目录位置，必须按优先级选择。

## 常见错误

| 错误 | 后果 | 修复 |
|---|---|---|
| 对抗 harness | phantom 状态，平台无法管理 | Step 0 检测 + Step 1a 优先原生 |
| 跳过隔离检测 | 嵌套 worktree | 始终先跑 Step 0 |
| 跳过 ignore 校验 | worktree 内容污染 git status | `git check-ignore` 前置 |
| 基线测试失败仍继续 | 无法区分新旧 bug | 报告并获用户确认 |

## 与多 Agent 并行开发的关系

该技能为 Superpowers 工作流第 2 阶段「并行分支准备」，常与以下技能配合：

- `brainstorming` → 设计确认后再隔离
- `writing-plans` / `executing-plans` → 在隔离环境中执行计划
- `dispatching-parallel-agents` → 每个独立任务域可对应独立 worktree + 子 Agent
- `finishing-a-development-branch` → 开发完成后合并/PR 并清理 worktree

## 输出格式

建议输出采用以下结构：

1. 隔离状态检测结果（已在 worktree / 需创建 / 用户拒绝）
2. 使用的机制（原生工具 / git fallback）与目录路径
3. Setup 执行情况（安装了哪些依赖）
4. 基线测试结果（通过 / 失败详情）
5. 就绪确认（分支名、路径、待实现功能）

## 适配版本

- [ ] copilot → `skills/adapters/copilot/`
- [ ] cursor → `skills/adapters/cursor/`
- [ ] claude-code → `skills/adapters/claude-code/`
- [ ] codex → `skills/adapters/codex/`

## 备注

来源为 [obra/superpowers using-git-worktrees](https://www.skills.sh/obra/superpowers/using-git-worktrees)，skills.sh 安装量 100K+。该技能是多 Agent 并行开发中 Git 隔离操作的核心基础设施，与 `dispatching-parallel-agents` 形成「空间隔离 + 任务分发」的组合。
