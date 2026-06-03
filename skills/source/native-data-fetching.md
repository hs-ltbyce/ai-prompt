---
title: Expo 原生数据获取技能
source: https://www.skills.sh/expo/skills/native-data-fetching
tags: [mobile, expo, networking, cache, offline, auth, skill]
tool: all
created: 2026-06-03
---

## 意图

帮助 AI 在 Expo 应用中实现稳定的数据请求体系，覆盖缓存、离线、错误处理与鉴权流。
该技能适用于“移动端网络层治理”，重点是可恢复性和弱网可用性。

## 触发方式

当用户出现以下意图时可触发：
- Expo 里怎么做数据请求和缓存
- 离线优先和后台刷新怎么实现
- token 刷新与安全存储怎么做
- 网络报错重试策略如何设计

## 核心行为

AI 应先设计请求与状态策略，再落地离线和鉴权细节。

### 步骤 1

定义请求基线：
- 统一请求封装和错误模型。
- 规范超时、重试与退避策略。
- 区分可重试与不可重试错误。

### 步骤 2

建立缓存和离线机制：
- 为关键数据设置缓存与失效策略。
- 结合网络状态处理离线读写行为。
- 设计恢复联网后的同步流程。

### 步骤 3

落实鉴权与安全：
- token 使用安全存储方案。
- 处理 access token 刷新与失效跳转。
- 避免在客户端暴露敏感配置。

### 步骤 4

结合路由数据加载：
- 在合适场景使用路由级加载模式。
- 明确首屏、预取和回填策略。
- 保证失败回退和重试入口可见。

## 输出格式

建议使用以下结构：
1. 网络层架构（请求、缓存、鉴权）
2. 核心流程（在线/离线/恢复）
3. 关键代码骨架
4. 观测与告警建议

## 适配版本

- [ ] copilot → skills/adapters/copilot/
- [ ] cursor → skills/adapters/cursor/
- [ ] claude-code → skills/adapters/claude-code/
- [ ] codex → skills/adapters/codex/

## 备注

来源为 expo/skills 的 native-data-fetching 页面，强调移动端请求稳定性与离线能力。
