---
title: Expo 原生 UI 构建技能
source: https://www.skills.sh/expo/skills/building-native-ui
tags: [mobile, expo, ui, navigation, native, skill]
tool: all
created: 2026-06-03
---

## 意图

帮助 AI 在 Expo 项目中按照原生平台习惯构建 UI，而不是把 Web 布局直接搬到移动端。
该技能强调路由、导航、组件与视觉细节的一致性，确保 iOS/Android 体验自然。

## 触发方式

当用户出现以下意图时可触发：
- Expo 页面怎么做原生感
- Router、Tabs、Modal 结构如何设计
- 移动端布局和安全区怎么处理
- Expo 组件选型和反模式规避

## 核心行为

AI 应先确定导航和页面结构，再处理样式与交互细节。

### 步骤 1

搭建路由与导航骨架：
- 明确 stack、tabs、modal 的路由层级。
- 使用清晰的目录与命名约定。
- 避免混乱跳转链路。

### 步骤 2

采用原生优先的布局与组件：
- 正确处理 safe area、滚动和弹层。
- 保持触达区域、间距和层级的移动端可用性。
- 避免 Web 专属元素和旧式替代方案。

### 步骤 3

统一样式与视觉规范：
- 按平台规范处理阴影、动画和视觉反馈。
- 使用现代 Expo 组件生态。
- 兼顾可读性和性能。

### 步骤 4

验证真实设备行为：
- 优先在 Expo Go 中完成首轮验证。
- 识别何时必须进入自定义原生构建。
- 输出可复现的验证步骤。

## 输出格式

建议使用以下结构：
1. 页面与导航结构图
2. 组件与样式实现建议
3. 反模式提示
4. 设备验证清单

## 适配版本

- [ ] copilot → skills/adapters/copilot/
- [ ] cursor → skills/adapters/cursor/
- [ ] claude-code → skills/adapters/claude-code/
- [ ] codex → skills/adapters/codex/

## 备注

来源为 expo/skills 的 building-native-ui 页面，适合 Expo UI 架构和界面规范落地。
