---
title: Skill 编写最佳实践指南（Anthropic Claude）
source: https://github.com/obra/superpowers/blob/main/skills/writing-skills/anthropic-best-practices.md
tags: [skill-编写, 最佳实践, claude, prompt-工程, ai-工具]
created: 2026-05-26
language: chinese
---

# Skill 编写最佳实践指南

学习如何编写有效的 Skill，使 Claude 能够发现并成功使用。

好的 Skill 应该是简洁的、结构良好的，并经过真实使用的测试。本指南提供实用的编写决策，帮助你编写Claude 能够发现和有效使用的 Skill。

## 核心原则

### 简洁至关重要

上下文窗口是公共资源。你的 Skill 与 Claude 需要知道的其他所有内容共享上下文窗口，包括：

- 系统提示
- 对话历史
- 其他 Skill 的元数据
- 你的实际请求

**Skill 中并非每个 token 都会产生直接成本。** 在启动时，只有所有 Skill 的元数据（名称和描述）被预加载。Claude 仅在 Skill 变得相关时读取 SKILL.md，并且只在需要时读取其他文件。

但是，在 SKILL.md 中保持简洁仍然很重要：一旦 Claude 加载它，每个 token 都会与对话历史和其他上下文竞争。

**默认假设：Claude 已经非常聪明**

只添加 Claude 不已经知道的上下文。对每条信息提出质疑：

- "Claude 真的需要这个解释吗？"
- "我可以假设 Claude 知道这个吗？"
- "这个段落值得它的 token 成本吗？"

**好例子：简洁（约 50 个 token）**
```python
# 提取 PDF 文本
使用 pdfplumber 进行文本提取：

import pdfplumber

with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

**坏例子：过于冗长（约 150 个 token）**
```
## 提取 PDF 文本

PDF（便携式文档格式）是一种常见的文件格式，包含文本、图像和其他内容。要从 PDF 中提取文本，你需要使用一个库。有许多可用于 PDF 处理的库，但我们建议使用 pdfplumber，因为它易于使用并处理大多数情况...
```

简洁版本假设 Claude 知道 PDF 的含义以及库如何工作。

### 设置适当的自由度

将特定性级别与任务的易变性和可变性相匹配。

**高自由度（基于文本的指令）：**

适用场景：
- 多种方法都有效
- 决策取决于上下文
- 启发式方法指导方法

示例：
```
## 代码审查流程

1. 分析代码结构和组织
2. 检查潜在的错误或边界情况
3. 建议改进可读性和可维护性的方案
4. 验证对项目约定的遵守
```

**中等自由度（伪代码或带参数的脚本）：**

适用场景：
- 存在首选模式
- 接受一些变化
- 配置影响行为

示例：
```
## 生成报告

使用此模板并根据需要自定义：

def generate_report(data, format="markdown", include_charts=True):
    # 处理数据
    # 生成指定格式的输出
    # 可选地包括可视化
```

**低自由度（特定脚本，很少或没有参数）：**

适用场景：
- 操作易出错且容易出现问题
- 一致性至关重要
- 必须遵循特定顺序

示例：
```
## 数据库迁移

运行此脚本：

python scripts/migrate.py --verify --backup

不要修改命令或添加额外的标志。
```

**类比：将 Claude 视为沿着路径探索的机器人：**
- 两边悬崖的窄桥：只有一条安全的前进方式。提供特定的防护栏和精确的指令（低自由度）。示例：必须按确切顺序运行的数据库迁移。
- ​开放没有危险的田野：许多路径通向成功。给出一般方向并信任 Claude 找到最佳路线（高自由度）。示例：上下文决定最佳方法的代码审查。

### 使用计划的所有模型进行测试

Skill 充当模型的附加功能，因此效率取决于底层模型。使用你计划使用 Skill 的所有模型对其进行测试。

**按模型的测试注意事项：**
- **Claude Haiku**（快速、经济）：Skill 提供了足够的指导吗？
- **Claude Sonnet**（平衡）：Skill 是否清晰高效？
- **Claude Opus**（强大的推理）：Skill 是否避免了过度解释？

对 Opus 完全有效的方法可能对 Haiku 需要更多细节。如果你计划在多个模型中使用 Skill，目标是使指令与所有模型都能很好地配合。

## Skill 结构

### YAML Frontmatter

SKILL.md frontmatter 需要两个字段：
- `name` - Skill 的人性化名称（最多 64 个字符）
- `description` - Skill 的作用和使用时机的一行描述（最多 1024 个字符）

### 命名惯例

使用一致的命名模式使 Skill 更容易引用和讨论。我们建议为 Skill 名称使用动名词形式（动词 + -ing），因为这清楚地描述了 Skill 提供的活动或功能。

**好的命名示例（动名词形式）：**
- "处理 PDF 文件"
- "分析电子表格"
- "管理数据库"
- "测试代码"
- "编写文档"

**可接受的替代方案：**
- 名词短语："PDF 处理"、"电子表格分析"
- 面向行动："处理 PDF"、"分析电子表格"

**避免：**
- 模糊的名称："助手"、"工具"、"实用程序"
- 过于通用："文档"、"数据"、"文件"
- Skill 集合中的不一致模式

### 编写有效的描述

`description` 字段支持 Skill 发现，应该包括 Skill 的作用和使用时机。

**始终用第三人称写作。** 描述被注入到系统提示中，不一致的视角可能会导致发现问题。

- 良好："处理 Excel 文件并生成报告"
- 避免："我可以帮你处理 Excel 文件"
- 避免："你可以用这个来处理 Excel 文件"

**要具体并包含关键术语。** 包括 Skill 的作用和使用时机的具体触发器/上下文。每个 Skill 只有一个描述字段。描述对于技能选择至关重要：Claude 使用它从可能的 100+ 个可用 Skill 中选择正确的 Skill。你的描述必须提供足够的细节供 Claude 知道何时选择此 Skill，而 SKILL.md 的其余部分提供实现细节。

**有效的示例：**

PDF 处理 Skill：
```
description: 从 PDF 文件中提取文本和表格，填充表单，合并文档。在处理 PDF 文件或当用户提到 PDF、表单或文档提取时使用。
```

Excel 分析 Skill：
```
description: 分析 Excel 电子表格、创建数据透视表、生成图表。在分析 Excel 文件、电子表格、表格数据或 .xlsx 文件时使用。
```

Git 提交助手 Skill：
```
description: 通过分析 git diff 生成描述性提交消息。当用户要求帮助编写提交消息或审查暂存更改时使用。
```

**避免含糊的描述，例如：**
```
description: 帮助处理文档
description: 处理数据
description: 对文件进行处理
```

### 渐进式披露模式

SKILL.md 充当概述，根据需要指导 Claude 。参考详细材料，就像入职指南中的目录一样。

**实用指南：**
- 将 SKILL.md 正文保持在 500 行以下以获得最佳性能
- 接近此限制时，将内容分成单独的文件
- 使用以下模式有效地组织说明、代码和资源

#### 模式 1：带引用的高级指南

示例结构：
```markdown
---
name: PDF 处理
description: 从 PDF 文件中提取文本和表格，填充表单，合并文档。在处理 PDF 文件或当用户提到 PDF、表单或文档提取时使用。
---

# PDF 处理

## 快速开始

使用 pdfplumber 提取文本：

```python
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

## 高级功能

**表单填充**：请参阅 [FORMS.md](FORMS.md) 获取完整指南
**API 参考**：请参阅 [REFERENCE.md](REFERENCE.md) 了解所有方法
**示例**：请参阅 [EXAMPLES.md](EXAMPLES.md) 获取常见模式

Claude 仅在需要时加载 FORMS.md、REFERENCE.md 或 EXAMPLES.md。
```

#### 模式 2：特定领域组织

对于具有多个领域的 Skill，按领域组织内容以避免加载无关上下文。当用户询问销售指标时，Claude 只需要读取与销售相关的模式，而不是财务或营销数据。这保持了低 token 使用率和上下文焦点。

```
bigquery-skill/
├── SKILL.md（概览和导航）
└── reference/
    ├── finance.md（收入、计费指标）
    ├── sales.md（机会、管线）
    ├── product.md（API 使用情况、功能）
    └── marketing.md（活动、归因）
```

#### 模式 3：条件细节

显示基本内容，链接到高级内容：

```markdown
# DOCX 处理

## 创建文档

对于新文档，使用 docx-js。请参阅 [DOCX-JS.md](DOCX-JS.md)。

## 编辑文档

对于简单的编辑，直接修改 XML。

**对于跟踪的更改**：请参阅 [REDLINING.md](REDLINING.md)
**对于 OOXML 详细信息**：请参阅 [OOXML.md](OOXML.md)

Claude 仅在用户需要这些功能时读取 REDLINING.md 或 OOXML.md。
```

### 避免深度嵌套的引用

当从其他引用文件引用文件时，Claude 可能会部分读取文件。遇到嵌套引用时，Claude 可能会使用 `head -100` 等命令来预览内容，而不是读取整个文件，导致信息不完整。

保持从 SKILL.md 一级深的引用。所有参考文件应直接链接到 SKILL.md，以确保在需要时 Claude 读取完整文件。

**坏例子：太深：**
```
# SKILL.md
请参阅 [advanced.md](advanced.md)...

# advanced.md
请参阅 [details.md](details.md)...

# details.md
这是实际信息...
```

**好例子：一级深：**
```
# SKILL.md

**基本用法**：[SKILL.md 中的说明]
**高级功能**：请参阅 [advanced.md](advanced.md)
**API 参考**：请参阅 [reference.md](reference.md)
**示例**：请参阅 [examples.md](examples.md)
```

### 用目录结构化更长的参考文件

对于长度超过 100 行的参考文件，在顶部包含目录。这确保 Claude 即使在部分读取时也能看到可用信息的全部范围。

示例：
```markdown
# API 参考

## 目录
- 身份验证和设置
- 核心方法（创建、读取、更新、删除）
- 高级功能（批量操作、webhooks）
- 错误处理模式
- 代码示例

## 身份验证和设置
...

## 核心方法
...
```

Claude 然后可以读取完整文件或根据需要跳到特定部分。

## 工作流和反馈循环

### 将工作流用于复杂任务

将复杂操作分解为清晰的顺序步骤。对于特别复杂的工作流，提供一个检查清单，Claude 可以将其复制到其响应中并在进行时检查。

**示例 1：研究合成工作流（对于没有代码的 Skill）：**

```markdown
## 研究合成工作流

复制此检查清单并跟踪你的进度：

```
研究进度：
- [ ] 步骤 1：读取所有源文档
- [ ] 步骤 2：识别关键主题
- [ ] 步骤 3：交叉引用索赔
- [ ] 步骤 4：创建结构化摘要
- [ ] 步骤 5：验证引文
```

**步骤 1：读取所有源文档**

查看 `sources/` 目录中的每个文档。记下主要论点和支持证据。

**步骤 2：识别关键主题**

寻找跨源的模式。什么主题反复出现？源在哪里一致或不一致？

**步骤 3：交叉引用索赔**

对于每个主要声明，验证它在源材料中出现。记下每个观点支持的来源。

**步骤 4：创建结构化摘要**

按主题组织发现。包括：
- 主要声明
- 来自来源的支持证据
- 冲突的观点（如果有）

**步骤 5：验证引文**

检查每个声明是否引用了正确的源文档。如果引文不完整，请返回步骤 3。
```

此示例展示工作流如何适用于不需要代码的分析任务。检查清单模式适用于任何复杂的多步骤过程。

**示例 2：PDF 表单填充工作流（对于有代码的 Skill）：**

```markdown
## PDF 表单填充工作流

复制此检查清单并在完成项目时检查：

```
任务进度：
- [ ] 步骤 1：分析表单（运行 analyze_form.py）
- [ ] 步骤 2：创建字段映射（编辑 fields.json）
- [ ] 步骤 3：验证映射（运行 validate_fields.py）
- [ ] 步骤 4：填充表单（运行 fill_form.py）
- [ ] 步骤 5：验证输出（运行 verify_output.py）
```

**步骤 1：分析表单**

运行：`python scripts/analyze_form.py input.pdf`

这提取表单字段及其位置，保存到 `fields.json`。

**步骤 2：创建字段映射**

编辑 `fields.json` 为每个字段添加值。

**步骤 3：验证映射**

运行：`python scripts/validate_fields.py fields.json`

继续之前修复任何验证错误。

**步骤 4：填充表单**

运行：`python scripts/fill_form.py input.pdf fields.json output.pdf`

**步骤 5：验证输出**

运行：`python scripts/verify_output.py output.pdf`

如果验证失败，请返回步骤 2。
```

清晰的步骤可防止 Claude 跳过关键验证。检查清单可帮助 Claude 和你跟踪通过多步骤工作流的进度。

### 实施反馈循环

常见模式：运行验证器 → 修复错误 → 重复

此模式大大提高了输出质量。

**示例 1：样式指南合规性（对于没有代码的 Skill）：**

```markdown
## 内容审查流程

1. 按照 STYLE_GUIDE.md 中的指南草拟内容
2. 对照检查清单评审：
   - 检查术语一致性
   - 验证示例遵循标准格式
   - 确认所有需要的部分都存在
3. 如果发现问题：
   - 记下每个问题及其具体部分参考
   - 修改内容
   - 再次审查检查清单
4. 仅当所有要求都得到满足时才继续
5. 完成并保存文档
```

这显示了使用参考文档而不是脚本的验证循环模式。"验证器"是 STYLE_GUIDE.md，Claude 通过读取和比较进行检查。

**示例 2：文档编辑流程（对于有代码的 Skill）：**

```markdown
## 文档编辑流程

1. 对 `word/document.xml` 进行编辑
2. **立即验证**：`python ooxml/scripts/validate.py unpacked_dir/`
3. 如果验证失败：
   - 仔细查看错误消息
   - 修复 XML 中的问题
   - 再次运行验证
4. **仅在验证通过时继续**
5. 重建：`python ooxml/scripts/pack.py unpacked_dir/ output.docx`
6. 测试输出文档
```

验证循环提前发现错误。

## 内容指南

### 避免对时间敏感的信息

不要包含会过时的信息：

**坏例子：对时间敏感（会变错）：**
```
如果你在 2025 年 8 月之前执行此操作，请使用旧 API。
2025 年 8 月之后，使用新 API。
```

**好例子（使用"旧模式"部分）：**

```markdown
## 当前方法

使用 v2 API 端点：`api.example.com/v2/messages`

## 旧模式

<details>
<summary>旧版 v1 API（已于 2025 年 8 月弃用）</summary>

v1 API 使用了：`api.example.com/v1/messages`

此端点不再受支持。
</details>
```

旧模式部分提供历史背景，而不会杂乱主要内容。

### 使用一致的术语

在整个 Skill 中选择一个术语并使用它：

**好的 - 一致：**
- 始终"API 端点"
- 始终"字段"
- 始终"提取"

**不好的 - 不一致：**
- 混合"API 端点"、"URL"、"API 路由"、"路径"
- 混合"字段"、"框"、"元素"、"控件"
- 混合"提取"、"拉出"、"获取"、"检索"

一致性可帮助 Claude 理解和遵循说明。

## 常见模式

### 模板模式

为输出格式提供模板。将严格程度与你的需求相匹配。

**对于严格要求（如 API 响应或数据格式）：**

```markdown
## 报告结构

始终使用此确切的模板结构：

\`\`\`markdown
# [分析标题]

## 执行摘要
[关键发现的一段概述]

## 主要发现
- 支持数据的发现 1
- 支持数据的发现 2
- 支持数据的发现 3

## 建议
1. 具体可操作的建议
2. 具体可操作的建议
\`\`\`
```

**对于灵活的指导（当适应有用时）：**

```markdown
## 报告结构

这是一个明智的默认格式，但根据分析使用你的最佳判断：

\`\`\`markdown
# [分析标题]

## 执行摘要
[概述]

## 主要发现
[根据你发现的内容调整部分]

## 建议
[根据具体背景定制]
\`\`\`

根据具体的分析类型调整部分。
```

### 示例模式

对于输出质量取决于查看示例的 Skill，提供输入/输出对，就像常规提示中一样：

```markdown
## 提交消息格式

按照这些示例生成提交消息：

**示例 1：**
输入：使用 JWT 令牌添加了用户身份验证
输出：
\`\`\`
feat(auth): 实现基于 JWT 的身份验证

添加登录端点和令牌验证中间件
\`\`\`

**示例 2：**
输入：修复了报告中日期显示不正确的错误
输出：
\`\`\`
fix(reports): 更正时区转换中的日期格式

在报告生成中始终使用 UTC 时间戳
\`\`\`

**示例 3：**
输入：更新了依赖项并重构了错误处理
输出：
\`\`\`
chore: 更新依赖项并重构错误处理

- 将 lodash 升级到 4.17.21
- 标准化端点上的错误响应格式
\`\`\`

遵循此样式：类型（范围）：简短描述，然后是详细说明。
```

示例帮助 Claude 更清楚地理解所需的样式和详细级别，而不仅仅是描述。

### 条件工作流模式

指导 Claude 通过决策点：

```markdown
## 文档修改工作流

1. 确定修改类型：

   **创建新内容？** → 遵循下面的"创建工作流"
   **编辑现有内容？** → 遵循下面的"编辑工作流"

2. 创建工作流：
   - 使用 docx-js 库
   - 从头开始构建文档
   - 导出为 .docx 格式

3. 编辑工作流：
   - 解包现有文档
   - 直接修改 XML
   - 每次更改后验证
   - 完成时重新打包
```

如果工作流变得大或复杂，有许多步骤，请考虑将它们推送到单独的文件中，并告诉 Claude 根据任务读取相应的文件。

## 评估和迭代

### 首先构建评估

在编写广泛的文档之前创建评估。这确保你的 Skill 解决真实问题，而不是 document想象的。

**评估驱动的开发：**

1. **识别差距**：在没有 Skill 的情况下在代表性任务上运行 Claude。记录具体的失败或缺失的背景
2. **创建评估**：构建三个测试这些差距的方案
3. **建立基准**：测量不使用 Skill 时 Claude 的性能
4. **编写最小说明**：创建足够的内容来弥补差距并通过评估
5. **迭代**：执行评估、与基准比较并改进

这种方法确保你解决实际问题，而不是预期可能永远不会出现的要求。

**评估结构：**

```json
{
  "skills": ["pdf-processing"],
  "query": "从这个 PDF 文件中提取所有文本并将其保存到 output.txt",
  "files": ["test-files/document.pdf"],
  "expected_behavior": [
    "使用适当的 PDF 处理库或命令行工具成功读取 PDF 文件",
    "从文档中的所有页面提取文本内容，不漏掉任何页面",
    "以清晰可读的格式将提取的文本保存到名为 output.txt 的文件中"
  ]
}
```

评估是衡量 Skill 有效性的真理来源。

### 与 Claude 一起迭代开发 Skill

最有效的 Skill 开发过程涉及 Claude 本身。与一个 Claude 实例（"Claude A"）合作创建将由其他实例（"Claude B"）使用的 Skill。Claude A 帮助你设计和完善说明，而 Claude B 在真实任务中测试它们。这是有效的，因为 Claude 模型既可以理解如何编写有效的 agent 说明，也能理解 agent 需要的信息。

**创建新 Skill：**

1. **在没有 Skill 的情况下完成任务**：与 Claude A 一起使用正常提示来处理问题。在你工作时，你自然会提供背景、解释偏好并共享程序知识。注意你反复提供的信息。

2. **识别可复用的模式**：完成任务后，识别你提供的会对类似的未来任务有用的上下文。示例：如果你进行了 BigQuery 分析，你可能提供了表名、字段定义、过滤规则（如"始终排除测试帐户"）和常见查询模式。

3. **要求 Claude A 创建 Skill**："创建一个 Skill，以捕捉我们刚刚使用的这个 BigQuery 分析模式。包括表模式、命名约定和关于过滤测试帐户的规则。"

Claude 模型本身理解 Skill 格式和结构。你不需要特殊的系统提示或"编写技能"skill 来让 Claude 帮助创建 Skill。只需要求 Claude 创建 Skill，它就会生成具有适当 frontmatter 和主体内容的正确结构化 SKILL.md 内容。

4. **审查简洁性**：检查 Claude A 是否没有添加不必要的解释。问："删除关于赢率含义的解释 - Claude 已经知道这个。"

5. **改进信息架构**：要求 Claude A 更有效地组织内容。例如："组织此内容，使表架构位于单独的参考文件中。我们以后可能会添加更多表。"

6. **在类似任务上测试**：在相关用例上将该 Skill 与 Claude B（加载了 Skill 的新实例）一起使用。观察 Claude B 是否找到正确的信息、正确应用规则以及成功处理任务。

7. **基于观察进行迭代**：如果 Claude B 遇到困难或遗漏了什么，请返回 Claude A 并具体说明："当 Claude 使用此 Skill 时，它忘记了为 Q4 按日期过滤。我们应该添加关于日期过滤模式的部分吗？"

**迭代现有 Skill：**

当改进 Skill 时，同一层级模式继续。你在以下之间交替：
- 与 Claude A 合作（帮助完善 Skill 的专家）
- 与 Claude B 测试（使用 Skill 执行真实工作的 agent）
- 观察 Claude B 的行为并将见解带回 Claude A

1. **在真实工作流中使用 Skill**：为 Claude B（加载了 Skill）提供实际任务，而不是测试方案

2. **观察 Claude B 的行为**：记下它在哪里苦恼、成功或做出意外选择。示例观察："当我要求 Claude B 提供地区销售报告时，它编写了查询但忘记了过滤掉测试帐户，即使 Skill 提到了这条规则。"

3. **返回 Claude A 进行改进**：分享当前的 SKILL.md 并描述你观察到的。问："我注意到 Claude B 在要求地区报告时忘记了过滤测试帐户。Skill 提到了过滤，但也许它不够突出？"

4. **审查 Claude A 的建议**：Claude A 可能建议重新组织以使规则更突出、使用更强的语言，如"必须过滤"而不是"始终过滤"，或重组工作流部分。

5. **应用并测试更改**：用 Claude A 的改进更新 Skill，然后在类似请求上再次与 Claude B 测试

6. **根据使用情况重复**：当你遇到新场景时，继续这个观察-完善-测试循环。每次迭代都根据真实 agent 行为而不是假设来改进 Skill。

**收集团队反馈：**

1. 与队友分享 Skill 并观察他们的使用情况
2. 问：Skill 在预期时激活吗？说明清楚吗？缺少什么？
3. 纳入反馈以解决你自己的使用模式中的盲点

**为什么这种方法有效**：Claude A 了解 agent 需求，你提供域名专业知识，Claude B 通过真实使用揭示差距，迭代完善基于观察到的行为而不是假设来改进 Skill。

### 观察 Claude 如何使用 Skill

当你迭代 Skill 时，注意 Claude 实际上在实践中如何使用它们。注意：

- **意外的探索路径**：Claude 是否以你没有预期的顺序读取文件？这可能表明你的结构不如你想象的那样直观
- **错过的连接**：Claude 是否未能遵循对重要文件的引用？你的链接可能需要更明确或更突出
- **对某些部分的过度依赖**：如果 Claude 反复读取同一文件，请考虑该内容是否应该在主 SKILL.md 中
- **被忽略的内容**：如果 Claude 从不访问捆绑的文件，它可能是不必要的或在主说明中信号不佳

基于这些观察而不是假设进行迭代。Skill 的元数据中的"名称"和"描述"特别关键。Claude 在决定是否响应当前任务触发 Skill 时使用这些。确保它们清楚地描述 Skill 的作用和应何时使用。

## 要避免的反模式

### 避免 Windows 风格路径

始终使用正斜杠在文件路径中，即使在 Windows 上：

- ✓ 好的：`scripts/helper.py`、`reference/guide.md`
- ✗ 避免：`scripts\helper.py`、`reference\guide.md`

Unix 风格路径在所有平台上有效，而 Windows 风格路径在 Unix 系统上会导致错误。

### 避免提供太多选项

除非必要，否则不要提出多种方法：

**坏例子：太多选择（令人困惑）：**
```
你可以使用 pypdf、或 pdfplumber、或 PyMuPDF、或 pdf2image、或...
```

**好例子：提供默认值（带转义舱口）：**
```
使用 pdfplumber 进行文本提取：

\`\`\`python
import pdfplumber
\`\`\`

对于需要 OCR 的扫描 PDF，请改为使用 pdf2image 和 pytesseract。
```

## 高级：带有可执行代码的 Skill

以下部分重点关注包含可执行脚本的 Skill。如果你的 Skill 仅使用 markdown 说明，请跳至"有效 Skill 的检查清单"。

### 解决，不要推诿

在为 Skill 编写脚本时，处理错误条件，而不是推给 Claude。

**好例子：明确处理错误：**

```python
def process_file(path):
    """处理文件，如果不存在则创建它。"""
    try:
        with open(path) as f:
            return f.read()
    except FileNotFoundError:
        # 创建具有默认内容的文件而不是失败
        print(f"未找到文件 {path}，创建默认值")
        with open(path, 'w') as f:
            f.write('')
        return ''
    except PermissionError:
        # 提供替代方案而不是失败
        print(f"无法访问 {path}，使用默认值")
        return ''
```

**坏例子：推给 Claude：**

```python
def process_file(path):
    # 只是失败，让 Claude 找出来
    return open(path).read()
```

配置参数也应该得到证实和记录，以避免"voodoo constants"（Ousterhout 定律）。如果你不知道正确的值，Claude 怎么会确定它？

**好例子：自记录：**

```python
# HTTP 请求通常在 30 秒内完成
# 更长的超时时间考虑到了慢速连接
REQUEST_TIMEOUT = 30

# 三次重试平衡了可靠性与速度
# 大多数间歇性故障在第二次重试时解决
MAX_RETRIES = 3
```

**坏例子：魔法数字：**

```python
TIMEOUT = 47  # 为什么是 47？
RETRIES = 5   # 为什么是 5？
```

### 提供实用脚本

即使 Claude 可以编写脚本，预制脚本也有优势：

**实用脚本的好处：**
- 比生成的代码更可靠
- 节省 token（无需在上下文中包含代码）
- 节省时间（不需要代码生成）
- 确保跨用途的一致性

指令文件（例如，forms.md）引用该脚本，Claude 可以执行它而无需将其完整内容加载到上下文中。

**重要区分**：在说明中清楚说明 Claude 是否应该：
- **执行脚本**（最常见）："运行 `analyze_form.py` 来提取字段"
- **作为参考读取**（对于复杂逻辑）："查看 `analyze_form.py` 了解提取算法"

对于大多数实用脚本，执行是首选，因为它更可靠和高效。

**示例：**

```markdown
## 实用脚本

**analyze_form.py**：从 PDF 中提取所有表单字段

\`\`\`bash
python scripts/analyze_form.py input.pdf > fields.json
\`\`\`

输出格式：
\`\`\`json
{
  "field_name": {"type": "text", "x": 100, "y": 200},
  "signature": {"type": "sig", "x": 150, "y": 500}
}
\`\`\`

**validate_boxes.py**：检查重叠的边界框

\`\`\`bash
python scripts/validate_boxes.py fields.json
# 返回："OK" 或列出冲突
\`\`\`

**fill_form.py**：将字段值应用于 PDF

\`\`\`bash
python scripts/fill_form.py input.pdf fields.json output.pdf
\`\`\`
```

### 使用可视化分析

当输入可以呈现为图像时，让 Claude 分析它们：

```markdown
## 表单布局分析

1. 将 PDF 转换为图像：
   \`\`\`bash
   python scripts/pdf_to_images.py form.pdf
   \`\`\`

2. 分析每个页面图像以识别表单字段
3. Claude 可以直观地看到字段位置和类型
```

在此示例中，你需要编写 `pdf_to_images.py` 脚本。Claude 的视觉功能有助于理解布局和结构。

### 创建可验证的中间输出

当 Claude 执行复杂的开放式任务时，它可能会犯错误。"计划-验证-执行"模式通过让 Claude 首先以结构化格式创建计划，然后在执行之前使用脚本验证该计划来提前发现错误。

示例：想象要求 Claude 根据电子表格更新 PDF 中的 50 个表单字段。如果没有验证，Claude 可能会引用不存在的字段、创建冲突的值、错过必填字段或错误地应用更新。

**解决方案**：使用上面显示的工作流模式（PDF 表单填充），但添加一个中间 `changes.json` 文件，在应用更改之前对其进行验证。工作流变成：分析 → 创建计划文件 → 验证计划 → 执行 → 验证。

**为什么这种模式有效：**
- 提前发现错误：验证在应用更改前发现问题
- 可机器验证：脚本提供客观验证
- 可逆规划：Claude 可以在不接触原件的情况下迭代计划
- 清晰调试：错误消息指向特定问题

**何时使用**：批量操作、破坏性更改、复杂验证规则、高风险操作。

**实施提示**：使用特定的错误消息使验证脚本详细，例如"字段 'signature_date' 未找到。可用字段：customer_name、order_total、signature_date_signed"以帮助 Claude 修复问题。

### 包装依赖项

Skill 在具有特定于平台的限制的代码执行环境中运行：
- **claude.ai**：可以从 npm 和 PyPI 安装包并从 GitHub 存储库拉取
- **Anthropic API**：没有网络访问权限且没有运行时包安装

在 SKILL.md 中列出所需的包，并验证它们在代码执行工具文档中可用。

### 运行时环境

Skill 在具有文件系统访问权限、bash 命令和代码执行功能的代码执行环境中运行。

**Claude 访问 Skill 的方式：**

1. **元数据预加载**：在启动时，所有 Skill 的 YAML frontmatter 中的名称和描述被加载到系统提示中
2. **按需读取文件**：Claude 在需要时使用 bash Read 工具从文件系统访问 SKILL.md 和其他文件
3. **脚本高效执行**：实用脚本可以通过 bash 执行，而无需将其完整内容加载到上下文中。只有脚本的输出消耗 token
4. **大型文件没有上下文处罚**：参考文件、数据或文档在实际读取之前不会消耗上下文 token

**这如何影响你的编写：**
- **文件路径很重要**：Claude 像浏览文件系统一样浏览你的 Skill 目录。使用正斜杠（`reference/guide.md`），而不是反斜杠
- **名称文件有描述性**：使用指示内容的名称：`form_validation_rules.md`，而不是 `doc2.md`
- **为发现而组织**：按域或功能组织目录
  - 好的：`reference/finance.md`、`reference/sales.md`
  - 坏的：`docs/file1.md`、`docs/file2.md`
- **绑定全面的资源**：包括完整的 API 文档、广泛的示例、大型数据集；在访问之前没有上下文处罚
- **对确定性操作选择脚本**：编写 `validate_form.py` 而不是要求 Claude 生成验证代码
- **使执行意图清晰**：
  - "运行 `analyze_form.py` 来提取字段"（执行）
  - "查看 `analyze_form.py` 了解提取算法"（作为参考读取）
- **测试文件访问模式**：通过与真实请求测试来验证 Claude 可以浏览你的目录结构

**示例：**

```
bigquery-skill/
├── SKILL.md（概览，指向参考文件）
└── reference/
    ├── finance.md（收入指标）
    ├── sales.md（管线数据）
    └── product.md（使用情况分析）
```

当用户询问有关收入的问题时，Claude 读取 SKILL.md，看到对 `reference/finance.md` 的引用，并调用 bash 来仅读取该文件。sales.md 和 product.md 文件保留在文件系统中，消耗零上下文 token，直到需要。这个基于文件系统的模型是支持渐进式披露的原因。Claude 可以浏览并选择性地加载每项任务所需的内容。

### MCP 工具引用

如果你的 Skill 使用 MCP（模型上下文协议）工具，总是使用完全限定的工具名称以避免"工具未找到"错误。

**格式**：`ServerName:tool_name`

**示例：**

```
使用 BigQuery:bigquery_schema 工具来检索表架构。
使用 GitHub:create_issue 工具来创建问题。
```

其中：
- `BigQuery` 和 `GitHub` 是 MCP 服务器名称
- `bigquery_schema` 和 `create_issue` 是这些服务器中的工具名称

没有服务器前缀，Claude 可能会找不到工具，特别是当有多个 MCP 服务器可用时。

### 避免假设工具已安装

不要假设包可用：

**坏例子：假设安装**：
```
使用 pdf 库来处理文件。
```

**好例子：明确说明依赖项**：
```
安装所需的包：\`pip install pypdf\`

然后使用它：
\`\`\`python
from pypdf import PdfReader
reader = PdfReader("file.pdf")
\`\`\`
```

## 技术说明

### YAML Frontmatter 要求

SKILL.md frontmatter 需要 `name`（最多 64 个字符）和 `description`（最多 1024 个字符）字段。

### Token 预算

将 SKILL.md 正文保持在 500 行以下以获得最佳性能。如果你的内容超过此限制，请使用上述"渐进式披露"模式将其分成单独的文件。

## 有效 Skill 的检查清单

在分享 Skill 之前，验证：

### 核心质量
- [ ] 描述是具体的且包含关键术语
- [ ] 描述包括 Skill 的作用和使用时机
- [ ] SKILL.md 正文少于 500 行
- [ ] 其他详细信息位于单独的文件中（如果需要）
- [ ] 没有对时间敏感的信息（或在"旧模式"部分）
- [ ] 始终使用一致的术语
- [ ] 示例是具体的，而不是抽象的
- [ ] 文件引用是一级深
- [ ] 适当使用渐进式披露
- [ ] 工作流有明确的步骤

### 代码和脚本
- [ ] 脚本解决问题而不是推给 Claude
- [ ] 错误处理是明确的和有帮助的
- [ ] 没有"magic numbers"（所有值都已证实）
- [ ] 说明中列出了所需的包并验证为可用
- [ ] 脚本有清晰的文档
- [ ] 没有 Windows 风格路径（全部前斜杠）
- [ ] 关键操作的验证/验证步骤
- [ ] 包含质量关键任务的反馈循环

### 测试
- [ ] 创建了至少三个评估
- [ ] 使用 Haiku、Sonnet 和 Opus 进行了测试
- [ ] 在真实用例下进行了测试
- [ ] 纳入了团队反馈（如果适用）

---

**来源**：[GitHub - obra/superpowers](https://github.com/obra/superpowers)
