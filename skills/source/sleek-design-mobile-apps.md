---
title: Sleek 移动设计生成技能
source: https://www.skills.sh/sleekdotdesign/agent-skills/sleek-design-mobile-apps
tags: [mobile, design, api, workflow, screenshot, skill]
tool: all
created: 2026-06-03
---

## 意图

帮助 AI 通过 Sleek API 以自然语言驱动移动端界面设计生成、编辑与截图交付。
该技能更偏“设计生产流水线”，用于快速产出和迭代视觉稿与屏幕方案。

## 触发方式

当用户出现以下意图时可触发：
- 快速生成一个移动应用设计稿
- 给现有屏幕加模块或重排布局
- 想用 API 流程批量产出移动端界面
- 需要把设计结果截图回传

## 核心行为

AI 在执行该技能时，应先确认 API 与权限可用，再进行设计生成与交付。

### 步骤 1

确认调用条件：
- 准备 API Key 并设置环境变量。
- 校验权限范围与配额。
- 明确同步或异步执行模式。

### 步骤 2

组织设计请求：
- 接收高层需求或局部修改指令。
- 使用自然语言描述目标屏幕与改动点。
- 控制一次只推进一个可验证任务。

### 步骤 3

执行与轮询：
- 异步模式下返回 run id 并轮询状态。
- 同步模式下在超时限制内等待结果。
- 遵守单项目单活跃运行约束。

### 步骤 4

截图与交付：
- 每次创建或更新后生成截图。
- 首次创建多屏时提供合并截图视图。
- 对失败请求进行安全重试并记录原因。

## 行为约束

AI 在使用该技能时应避免：
- 缺少密钥或权限时继续执行写操作。
- 在同一项目并发触发多个运行。
- 只返回文本，不返回可视化结果。

## 输出格式

建议使用以下结构：
1. 请求目标与输入说明
2. 执行方式（同步/异步）与状态
3. 产出屏幕与截图清单
4. 下一步设计迭代建议

## 适配版本

- [ ] copilot → skills/adapters/copilot/
- [ ] cursor → skills/adapters/cursor/
- [ ] claude-code → skills/adapters/claude-code/
- [ ] codex → skills/adapters/codex/

## 备注

来源为 sleekdotdesign/agent-skills 的 sleek-design-mobile-apps 页面。该技能更偏设计自动化，而非 RN 代码规范本身。
