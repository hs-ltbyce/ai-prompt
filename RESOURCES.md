# 外部资源导航

收录 AI 编程辅助相关的技能市场、优质仓库和专业工具，供参考与选材。

---

## 技能市场

| 平台 | 链接 | 说明 |
|---|---|---|
| **skills.sh** | [skills.sh](https://skills.sh) | 目前全球最大的开放 Agent Skills 平台，总安装量超 9 万，支持一键安装命令，可查看排行榜与热门技能 |
| **Codelibrium** | [codelibrium.com](https://codelibrium.com) | 新兴 AI 行为文件生成器与市场，支持 Cursor、Claude Code、Windsurf 等多工具，可发布作品并赚取积分 |
| **LobeHub Skills Marketplace** | [lobehub.com](https://lobehub.com) | 社区驱动的技能市场，含 `rules-distill` 等实用技能，可从已有技能中提炼通用原则生成新 Rules |
| **Qoder 社区** | [qoder.dev](https://qoder.dev) | 按前端、后端、DevOps 等岗位分类的技能社区，便于按职能快速筛选相关资源 |
| Cursor Directory | [cursor.directory](https://cursor.directory) | 社区共享的 Cursor Rules 集合，可直接套用 |
| PromptHub | [prompthub.us](https://www.prompthub.us) | 专注 AI 提示词的市场与分享平台 |

---

## 官方与社区优质仓库

### 官方资源

| 仓库 | Stars | 链接 | 说明 |
|---|---|---|---|
| **anthropics/skills** | 138k | [github.com/anthropics/skills](https://github.com/anthropics/skills) | Anthropic 官方维护的 Agent Skills 仓库，提供开放标准规范（[agentskills.io](https://agentskills.io)）和大量高质量示例，是 skills.sh 生态的权威参考 |
| GitHub Copilot 文档 | — | [docs.github.com/copilot](https://docs.github.com/en/copilot) | Copilot 全量官方文档，含自定义指令规范 |
| Claude Code 文档 | — | [docs.anthropic.com/claude-code](https://docs.anthropic.com/en/docs/claude-code/overview) | Claude Code CLI 官方使用指南 |
| Anthropic Cookbook | — | [github.com/anthropics/anthropic-cookbook](https://github.com/anthropics/anthropic-cookbook) | Claude 官方最佳实践与 Prompt 示例 |
| OpenAI Prompt Engineering | — | [platform.openai.com/docs/guides/prompt-engineering](https://platform.openai.com/docs/guides/prompt-engineering) | OpenAI 官方 Prompt 工程指南 |

### 社区精选

| 仓库 | Stars | 链接 | 说明 |
|---|---|---|---|
| **obra/superpowers** | 199k | [github.com/obra/superpowers](https://github.com/obra/superpowers) | 目前 star 数最高的 agentic skills 框架，覆盖 Claude Code、Codex、Cursor、Copilot CLI 等，内含完整的开发方法论（TDD、子 Agent 分发、Git Worktree 等） |
| **Claude-Code-Everything-You-Need-to-Know** | 1.8k | [github.com/wesammustafa/Claude-Code-Everything-You-Need-to-Know](https://github.com/wesammustafa/Claude-Code-Everything-You-Need-to-Know) | 全面的 Claude Code 实战指南，涵盖 Skills、Commands、Subagents、Hooks、Rules、MCP 等各个维度 |
| **TRAE-Skills** | 220 | [github.com/HighMark-31/TRAE-Skills](https://github.com/HighMark-31/TRAE-Skills) | 150+ 个按岗位（前端/后端/DevOps/安全/测试/AI 工程等）分类的技能集合，兼容 TRAE、Claude Code、Cursor |
| awesome-chatgpt-prompts | 高 | [github.com/f/awesome-chatgpt-prompts](https://github.com/f/awesome-chatgpt-prompts) | 高质量 ChatGPT Prompt 合集，star 极高 |
| awesome-cursorrules | 高 | [github.com/PatrickJS/awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) | 社区共享的 Cursor Rules 大全 |
| awesome-copilot | — | [github.com/github/awesome-copilot](https://github.com/github/awesome-copilot) | GitHub 官方整理的 Copilot 指令与资源 |

---

## 专业工具与 CLI

| 工具 | 链接 | 说明 |
|---|---|---|
| **npx skills CLI** | [skills.sh/docs/cli](https://www.skills.sh/docs/cli) · [github.com/vercel-labs/skills](https://github.com/vercel-labs/skills) | Vercel 出品，skills.sh 的官方 CLI，支持搜索、安装、更新、卸载技能。示例：`npx skills add vercel-labs/skills --skill find-skills` |
| **skill-rules (sr)** | *(待核实)* | 跨 IDE 同步 Skills/Rules 的工具，支持 Claude Code、Cursor、Windsurf，并可按 dev/qa/production 环境阶段激活不同配置 |
| **GitHub Copilot in the CLI** | [docs.github.com/…/about-github-copilot-in-the-cli](https://docs.github.com/en/copilot/github-copilot-in-the-cli/about-github-copilot-in-the-cli) | `gh copilot suggest` / `explain`，终端内 AI 辅助 |
| **Claude Code** | [docs.anthropic.com/claude-code](https://docs.anthropic.com/en/docs/claude-code/overview) | Anthropic 官方 AI 编码 CLI，支持 CLAUDE.md 注入 |
| **Aider** | [aider.chat](https://aider.chat) | 终端内与 Git 深度集成的 AI 结对编程工具 |
| **Continue** | [continue.dev](https://continue.dev) | 开源 IDE 插件，支持本地/云端多模型，可自定义 context |
| **Cursor** | [cursor.com](https://www.cursor.com) | 内置 AI 的代码编辑器，支持 `.cursor/rules/` 注入 |
| **Codex CLI** | [github.com/openai/codex](https://github.com/openai/codex) | OpenAI 官方开源终端编码 Agent，支持 AGENTS.md |
