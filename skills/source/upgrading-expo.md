---
title: Expo SDK 升级迁移技能
source: https://www.skills.sh/expo/skills/upgrading-expo
tags: [mobile, expo, upgrade, migration, compatibility, skill]
tool: all
created: 2026-06-03
---

## 意图

帮助 AI 以结构化流程完成 Expo SDK 升级，识别并处理破坏性变更、依赖冲突与迁移事项。
该技能用于降低升级风险，确保从诊断到回归验证形成闭环。

## 触发方式

当用户出现以下意图时可触发：
- Expo 要从旧版本升级到新版本
- 升级后报错，依赖冲突怎么排查
- React 19 或新架构迁移怎么做
- 音视频、路由、导航等模块升级策略

## 核心行为

AI 在执行该技能时，应先做版本与依赖诊断，再推进分步升级与回归验证。

### 步骤 1

升级前诊断：
- 识别当前 SDK、React 与关键依赖版本。
- 检查历史补丁、配置残留和隐式依赖。
- 明确目标版本与升级窗口。

### 步骤 2

执行升级主流程：
- 按官方路径升级 SDK 与核心包。
- 处理 lockfile、缓存和预构建状态。
- 逐步解决 peer dependency 冲突。

### 步骤 3

处理关键迁移项：
- 新架构、React 19、编译器相关变更。
- 导航与路由入口迁移。
- 音视频和废弃包替换。

### 步骤 4

完成回归与清理：
- 执行构建、运行与关键路径回归。
- 清理无效配置和历史兼容项。
- 输出升级记录与后续待办。

## 行为约束

AI 在使用该技能时应避免：
- 跳过升级前诊断直接改依赖。
- 一次性大规模改动而不分阶段验证。
- 忽略官方 breaking changes 列表。

## 输出格式

建议使用以下结构：
1. 当前状态诊断
2. 升级计划（分阶段）
3. 迁移变更清单
4. 验证结果与遗留问题

## 适配版本

- [ ] copilot → skills/adapters/copilot/
- [ ] cursor → skills/adapters/cursor/
- [ ] claude-code → skills/adapters/claude-code/
- [ ] codex → skills/adapters/codex/

## 备注

来源为 expo/skills 的 upgrading-expo 页面，覆盖 SDK 53-55 相关升级注意事项与迁移参考。
