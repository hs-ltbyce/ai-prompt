# AI 整理指南

本文件用于告诉 AI 助手如何帮助整理本仓库的内容。每次需要 AI 帮助整理时，请先让 AI 阅读本文件。

---

## 仓库背景

这是一个个人 AI 编程辅助资产仓库，收录：
- 可复用的提问模板（`templates/`）
- AI 工具规则（`rules/`）
- AI 工具 Skill（`skills/`）

详细目录说明见 [README.md](./README.md)。

---

## 你会遇到的两类整理任务

### 情况一：用户给你一个第三方链接

用户会直接贴出一个 URL，例如：
```
https://github.com/xxx/yyy/blob/main/some-skill.md
```

**你需要做：**
1. 访问该链接，获取内容
2. 判断内容类型（template / rule / skill）
3. 理解内容意图，用一句话概括，写入 `skills/source/` 或对应 source 目录作为母本
4. 默认只产出 source 母本；**仅当用户明确要求**时，再生成 `adapters/` 版本
5. 若需要适配，再识别该内容适用的工具格式（Copilot / Cursor / Claude Code / Codex 等）并写入 `adapters/` 子目录
6. 在文件头部填写 frontmatter，`source` 字段填写原始 URL
7. 文件命名：使用英文小写 + 连字符，例如 `code-review.md`

---

### 情况二：用户直接粘贴一段内容

用户会粘贴一段格式不规范、来源不明的内容，**不会说明是哪里来的**。

**你需要做：**
1. 理解内容意图，判断类型（template / rule / skill）
2. 整理并规范化内容格式（修正 Markdown、补全结构）
3. 提炼核心意图，写入对应 `source/` 目录作为母本
4. 默认只生成 `source/` 母本；**仅当用户明确要求**时，再生成对应 `adapters/` 版本
5. 在文件头部 frontmatter 中，`source` 填写 `unknown`
6. 如果能从内容中推断来源（作者署名、项目名等），在文件中注明

---

## 文件放置规则

| 内容类型 | source 文件位置 | adapters 位置 |
|---|---|---|
| 提问模板 | `templates/` | 无需适配 |
| 规则 | `rules/source/` | `rules/adapters/<tool>/` |
| Skill | `skills/source/` | `skills/adapters/<tool>/` |

**工具目录名对照：**

| 工具 | 目录名 | 原生文件格式 |
|---|---|---|
| GitHub Copilot | `copilot` | `.prompt.md`（带 frontmatter） |
| Cursor | `cursor` | `.mdc`（带 frontmatter） |
| Claude Code | `claude-code` | `CLAUDE.md` 风格的 Markdown |
| Codex | `codex` | `AGENTS.md` 风格的 Markdown |

---

## 文件格式规范

### 所有文件头部必须包含 frontmatter

```markdown
---
title: 文件标题（简洁描述功能）
source: self | https://原始链接 | unknown
tags: [标签1, 标签2]
tool: copilot | cursor | claude-code | codex | all
created: YYYY-MM-DD
---
```

- `source`：来源，三选一
- `tool`：适用工具，`all` 表示工具无关（用于 source/ 目录）
- `tags`：尽量补充，便于后续检索

### source/ 目录文件结构

```markdown
---
title: xxx
source: ...
tags: [...]
tool: all
created: ...
---

## 意图

用 1-3 句话描述这个 skill/rule 要解决什么问题。

## 核心内容

（规则的主要内容，自然语言描述，不依赖任何特定工具格式）

## 适配版本

- [ ] copilot
- [ ] cursor
- [ ] claude-code
- [ ] codex
```

### adapters/ 目录文件结构

以工具原生格式为准，同时保留 frontmatter 标注来源。

---

## 语言统一规范

1. **默认使用中文**：本仓库新增或整理内容时，说明文字、章节标题、步骤描述优先使用中文。
2. **术语可中英并存**：如存在行业通用术语（如 Hook、Composable、Container、Presentational），可保留英文术语并配合中文说明。
3. **来源引用保留原文**：外部链接标题、专有名词、命令行指令可保留原文，不强制翻译。
4. **同一文件保持单一主语言**：除术语与引用外，避免中英文段落混写。
5. **适配文件遵循同一规范**：`skills/adapters/` 与 `rules/adapters/` 内文同样默认中文。

---

## 命名规范

- 全部英文小写
- 单词之间用连字符 `-` 分隔
- 名称体现功能，不体现工具名（工具名已由目录区分）
- 示例：`code-review.md`、`git-commit-style.md`、`test-generation.md`

---

## 整理时的注意事项

1. **不要改变内容的核心意图**，只做格式规范化
2. **统一语言优先中文**：除引用、命令、术语外，内容主体统一用中文表达
3. **内容有明显错误或过时信息**，在文件末尾用 `> ⚠️ 注意：` 块标注，不要直接删改
4. **同一 skill 有多个工具版本时**，source/ 里的母本必须先于 adapters/ 存在
5. **默认先写 source，不主动写 adapters**：除非用户明确要求适配到某个工具
6. **如果无法判断应放哪个目录**，默认放 `skills/source/`，并在文件末尾注明"待分类"
