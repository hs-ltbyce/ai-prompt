# Mobile 技能选型矩阵

## 覆盖技能

- vercel-react-native-skills
- sleek-design-mobile-apps
- building-native-ui
- native-data-fetching
- expo-tailwind-setup
- upgrading-expo

## 能力定位矩阵

| 技能 | 核心定位 | 最适合场景 | 典型风险/限制 |
| --- | --- | --- | --- |
| vercel-react-native-skills | RN/Expo 工程与性能基线 | 新项目规范建设、性能治理 | 规则覆盖广，需按优先级落地 |
| sleek-design-mobile-apps | 设计生成与截图交付流水线 | 快速产出设计方案、设计迭代 | 依赖外部 API 与权限，非纯代码规范 |
| building-native-ui | Expo 原生 UI 与导航实践 | 构建原生感页面和路由结构 | 需遵循平台规范，不能照搬 Web 布局 |
| native-data-fetching | 移动端网络、缓存、离线与鉴权 | 离线优先、弱网稳定性、鉴权流程 | 要求明确缓存和恢复策略 |
| expo-tailwind-setup | Expo 的 Tailwind v4 样式体系 | 统一跨端样式表达与主题变量 | 需完成 Metro/PostCSS 配置与组件封装 |
| upgrading-expo | Expo SDK 升级与迁移流程 | 跨版本升级、破坏性变更处理 | 必须分阶段验证，避免一次性大改 |

## 推荐组合

### 1) 通用生产组合（优先推荐）

- vercel-react-native-skills
- building-native-ui
- native-data-fetching

适用：大多数 RN/Expo 业务项目，从规范、UI 到网络层形成完整基础能力。

### 2) 设计驱动组合

- sleek-design-mobile-apps
- building-native-ui
- expo-tailwind-setup

适用：强调视觉产出效率与界面风格一致性，同时保留原生交互体验。

### 3) 升级维护组合

- upgrading-expo
- vercel-react-native-skills
- native-data-fetching

适用：存量项目升级与稳定性回归，优先处理兼容性与风险收敛。

## 安装命令参考

```bash
npx skills add https://github.com/vercel-labs/agent-skills --skill vercel-react-native-skills
npx skills add https://github.com/sleekdotdesign/agent-skills --skill sleek-design-mobile-apps
npx skills add https://github.com/expo/skills --skill building-native-ui
npx skills add https://github.com/expo/skills --skill native-data-fetching
npx skills add https://github.com/expo/skills --skill expo-tailwind-setup
npx skills add https://github.com/expo/skills --skill upgrading-expo
```

## 对应 source 母本

- `skills/source/vercel-react-native-skills.md`
- `skills/source/sleek-design-mobile-apps.md`
- `skills/source/building-native-ui.md`
- `skills/source/native-data-fetching.md`
- `skills/source/expo-tailwind-setup.md`
- `skills/source/upgrading-expo.md`
