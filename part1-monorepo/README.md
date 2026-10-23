# Part 1 · Monorepo：只学 pnpm workspace

**周期**：第 1~3 周 ｜ **目标层级**：L2，M1/M5 到 L3

这一部分不教 Turborepo、Nx、Lerna、Changesets。只把 pnpm workspace 学到能维护一个真实多包仓库的程度：依赖从哪来、包之间怎么引用、出了问题怎么查。

| 章节 | 核心问题 | 产出 | 状态 |
|---|---|---|---|
| M1 该不该上 Monorepo | 什么信号适合、成本落在哪、工具各自定位 | 选型决策卡 | 初稿 |
| M2 pnpm workspace 从零 | workspace 协议、`catalog:`、`packageManager`、`allowBuilds` | 4 包仓库跑通 | 初稿 |
| M3 依赖模型与包边界 | store、符号链接、幽灵依赖、peer、重复版本、filter、拓扑序 | 依赖体检报告 | 初稿 |
| M4 跨包引用与类型 | 源码直连 vs 构建产物、`exports`、TS 配置、循环依赖 | 两种方案对比 | 初稿 |
| M5 统一规范与提交门禁 | ESLint 9 flat 共享配置、TS base、commitlint、husky + lint-staged | 新包零配置接入 | 初稿 |
| M6 日常操作与排障 | 增删包、升级、锁文件冲突、filter 写错、CI 与本地不一致 | 操作手册 + 10 条报错 | 初稿 |

## 本部分预埋的坑

1. admin 直接 `import lodash` 但没声明依赖（本地能跑，CI 崩）
2. 内部包写 `^0.1.0`，workspace 实际是 0.2.0（装出重复副本）
3. utils ↔ ui 循环依赖（类型推断变 any）
4. `@lab/ui` 只出 ESM，被 CJS 工具消费

## 闭卷考核（第 3 周末）

30 分钟内从空目录重建 4 包仓库：应用能启动、包间引用正确、`pnpm --filter` 能精确构建单个包及其依赖。允许查官方文档，不允许看教程。
