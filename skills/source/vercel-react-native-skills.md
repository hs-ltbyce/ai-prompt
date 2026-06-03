---
title: React Native 全栈最佳实践技能
source: https://www.skills.sh/vercel-labs/agent-skills/vercel-react-native-skills
tags: [mobile, react-native, expo, performance, architecture, skill]
tool: all
created: 2026-06-03
---

## 意图

帮助 AI 在 React Native/Expo 项目中遵循高优先级工程实践，覆盖性能、动画、导航、组件模式与配置治理。
该技能适合做“移动端工程基线”，避免常见卡顿、重渲染和平台不一致问题。

## 触发方式

当用户出现以下意图时可触发：
- 优化 React Native 性能
- Expo 项目怎么做工程规范
- 列表很卡、动画掉帧怎么处理
- React Native 导航和 UI 组件选型
- /vercel-react-native-skills

## 核心行为

AI 应按影响优先级先处理性能与渲染问题，再处理动画、导航和工程结构问题。

### 步骤 1

建立性能基线：
- 优先处理列表虚拟化、图片加载与重渲染问题。
- 避免在 render 中创建不稳定对象和回调。
- 对高频组件使用 memo 化与稳定依赖。

### 步骤 2

采用原生友好的动画与导航：
- 动画优先使用 GPU 友好属性（transform、opacity）。
- 在可用场景优先 Reanimated 模式。
- 导航优先 Native Stack 和原生 Tabs 能力。

### 步骤 3

统一现代 UI 与平台约束：
- 图片优先现代组件方案，减少旧 API 依赖。
- 交互组件优先可组合且可访问的模式。
- 明确 Safe Area、滚动容器和模态呈现规范。

### 步骤 4

收敛工程结构与配置：
- 对 monorepo/native 依赖边界做清晰拆分。
- 避免隐式配置和历史兼容包长期残留。
- 输出可落地的检查清单而非泛化建议。

## 输出格式

建议使用以下结构：
1. 问题诊断（性能/动画/导航/配置）
2. 优先级修复清单（先高影响再低影响）
3. 代码或配置调整建议
4. 验证指标（流畅度、渲染次数、交互响应）

## 适配版本

- [ ] copilot → skills/adapters/copilot/
- [ ] cursor → skills/adapters/cursor/
- [ ] claude-code → skills/adapters/claude-code/
- [ ] codex → skills/adapters/codex/

## 备注

来源为 vercel-labs/agent-skills 的 vercel-react-native-skills 页面，侧重 React Native 与 Expo 的生产实践。
