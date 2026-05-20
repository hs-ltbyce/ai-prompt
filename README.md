# AI Prompt 知识库

> 个人 AI 编程辅助资产沉淀仓库，收录自己在工作中积累的 Prompt 模板、Rules、Skills，以及从外部收集的优质资源。

## 目的

通过系统化整理和沉淀，持续提升借助 AI 进行编码的能力。

---

## 目录结构

```
ai-prompt/
├── README.md              # 本文件
├── AI_GUIDE.md            # AI 整理指南（告诉 AI 如何帮你整理内容）
│
├── templates/             # 可复用的提问模板（对话框架，反复使用）
│
├── rules/
│   ├── source/            # 工具无关的规则母本（代码风格、提交规范等）
│   └── adapters/          # 各工具适配说明（如何将母本部署到 Copilot/Cursor 等）
│
├── skills/
│   ├── source/            # 工具无关的 skill 意图描述
│   └── adapters/          # 各工具的具体实现（Copilot .prompt.md / Cursor 格式等）
│
└── _templates/            # 新建内容时的格式模板（规范参考）
```

---

## 内容说明

### `templates/` — 提问模板

存放**可复用的提问框架**，而非一次性对话。例如：
- "帮我 review 这段代码，重点关注性能和安全"
- "帮我将以下需求拆解为开发任务"

### `rules/source/` — 规则母本

与工具无关的规则内容，按场景拆分文件：
- `code-style.md`：命名、格式、注释规范
- `git-commit.md`：提交信息规范
- `testing.md`：测试要求

### `rules/adapters/` — 工具适配

说明如何将母本规则部署到各 AI 工具：
- Copilot → `.github/copilot-instructions.md`
- Cursor → `.cursor/rules/*.mdc`
- Claude Code → `CLAUDE.md`
- Codex → `AGENTS.md`

### `skills/source/` — Skill 意图描述

用自然语言描述一个 skill 要做什么，不依赖具体工具格式，作为各适配版本的"母本"。

### `skills/adapters/` — Skill 工具实现

按工具分子目录，存放对应格式的 skill 文件：
- `copilot/`：`.prompt.md` 格式
- `cursor/`：Cursor 规范格式
- `claude-code/`：Claude Code 格式
- `codex/`：Codex 格式

### `_templates/` — 格式模板

新建文件时参考的规范模板，保证内容格式统一。

---

## 如何新增内容

1. 参考 `_templates/` 中的对应模板
2. 放入正确目录
3. 如需 AI 帮助整理，参见 [AI_GUIDE.md](./AI_GUIDE.md)

---

## 来源标注规范

每个文件头部应注明来源，格式如下：

```markdown
---
source: self | https://... | unknown
tags: [coding, review, python]
---
```

- `self`：自己原创
- URL：外部来源链接
- `unknown`：来源不明（直接粘贴内容）

---

## 外部资源

技能市场、官方与社区优质仓库、专业工具与 CLI 的整理汇总，详见 [RESOURCES.md](./RESOURCES.md)。
