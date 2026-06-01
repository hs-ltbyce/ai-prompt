---
title: Container/Presentational 模式技能
source: https://www.skills.sh/patternsdev/skills/container-presentational
tags: [react, vue, architecture, separation-of-concerns, skill]
tool: all
created: 2026-06-01
---

## 意图

帮助 AI 在前端组件设计中正确应用 Container/Presentational 模式：将数据获取与业务逻辑和 UI 渲染解耦。
在现代实践中，优先使用 React Hooks 或 Vue Composables 达成同样目标，避免不必要的容器层。

## 触发方式

当用户提到以下关键词时触发：
- 容器展示模式
- 聪明组件与木偶组件
- 视图与逻辑分离
- 如何拆分 React 组件职责
- 用 Hook 替代容器组件

## 核心行为

AI 应先判断是否真的需要拆分容器层，再给出最小可行实现方案。
输出应包含：适用性判断、组件拆分建议、现代替代方案（Hook/Composable）、以及简洁代码骨架。

### 步骤 1

识别场景是否适合该模式：
- 适合：需要复用展示组件、需要更强可测试性、数据逻辑复杂。
- 不适合：页面很小且一次性、拆分后样板代码明显增多、已有 Hook/Composable 完成解耦。

### 步骤 2

给出职责边界：
- Presentational 组件：只接收 props 并渲染。
- Container 组件或 Hook/Composable：负责数据请求、状态管理、副作用和错误处理。

### 步骤 3

优先输出现代写法：
- React：优先自定义 Hook + 展示组件；必要时增加容器外壳。
- Vue 3：优先 Composable + 展示组件；必要时增加容器外壳。

### 步骤 4

补充工程化建议：
- 为展示组件提供纯渲染测试。
- 为 Hook 或 Composable 提供状态流转测试。
- 避免为简单场景过度抽象。

## 输出格式

建议使用以下结构：
1. 适用性判断（适合/不适合 + 原因）
2. 组件拆分方案（职责清单，容器层与展示层）
3. 最小代码模板（展示组件 + Hook/Composable + 可选容器组件）
4. 测试建议（展示层/逻辑层）

## 适配版本

- [ ] copilot → skills/adapters/copilot/
- [ ] cursor → skills/adapters/cursor/
- [ ] claude-code → skills/adapters/claude-code/
- [ ] codex → skills/adapters/codex/

## 备注

来源为 patterns.dev 在 skills.sh 的公开技能页面。该模式依然有效，但在 React 与 Vue 现代栈中应优先通过 Hook 或 Composable 进行逻辑复用。