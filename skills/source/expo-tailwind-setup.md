---
title: Expo Tailwind 样式体系搭建技能
source: https://www.skills.sh/expo/skills/expo-tailwind-setup
tags: [mobile, expo, tailwind, nativewind, styling, skill]
tool: all
created: 2026-06-03
---

## 意图

帮助 AI 在 Expo 项目中建立 Tailwind CSS v4 的跨平台样式体系，统一 iOS/Android/Web 的样式表达。
该技能用于降低样式维护成本，并保留原生平台差异化控制能力。

## 触发方式

当用户出现以下意图时可触发：
- Expo 项目接入 Tailwind
- NativeWind 怎么和 Expo 配合
- 想统一移动端和 Web 的 className 样式
- 主题变量和平台颜色如何管理

## 核心行为

AI 应先完成运行时与构建链配置，再定义组件层和主题层约定。

### 步骤 1

完成依赖与构建接入：
- 接入 Tailwind v4 与 NativeWind 对应能力。
- 配置 Metro 和 PostCSS 所需项。
- 确认无需历史 Babel 兜底方案。

### 步骤 2

建立可复用样式组件层：
- 提供包装后的基础组件集合。
- 统一 className 到原生样式映射。
- 减少重复样式拼接逻辑。

### 步骤 3

定义主题与平台差异：
- 使用 CSS 变量维护主题令牌。
- 为平台差异提供条件样式能力。
- 保证深浅色和语义色策略一致。

### 步骤 4

打通 JS 侧主题读取与验证：
- 允许在逻辑层访问关键主题值。
- 校验移动端和 Web 渲染一致性。
- 输出最小化迁移清单。

## 输出格式

建议使用以下结构：
1. 安装与配置步骤
2. 基础组件层设计
3. 主题变量与平台样式策略
4. 迁移与验证清单

## 适配版本

- [ ] copilot → skills/adapters/copilot/
- [ ] cursor → skills/adapters/cursor/
- [ ] claude-code → skills/adapters/claude-code/
- [ ] codex → skills/adapters/codex/

## 备注

来源为 expo/skills 的 expo-tailwind-setup 页面，重点是 Expo 的 Tailwind v4 现代接入路径。
