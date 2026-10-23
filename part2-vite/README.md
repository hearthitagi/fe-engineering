# Part 2 · 构建：只学 Vite 8

**周期**：第 4~7 周 ｜ **目标层级**：L2

这一部分不教 Webpack、Rspack、Rollup、SWC。只把 Vite 8（Rolldown + Oxc）从原理到体积、速度、资源讲透，并且用同一个「脏」项目做前后对比。

| 章节 | 核心问题 | 产出 | 状态 |
|---|---|---|---|
| B1 Vite 心智模型 | dev 与 build 是两条链路；ESM、预构建、Rolldown/Oxc 各管什么 | 链路图 | 初稿 |
| B2 度量与基线 | 分析器怎么读、计时怎么测才可信、指标怎么定义 | 第一份基线报告 | 初稿 |
| B3 分包与长效缓存 | 懒加载、`codeSplitting` 分组、vendor 粒度、hash 稳定性、preload | 业务改动只变业务 hash | 初稿 |
| B4 Tree shaking 与依赖治理 | ESM/CJS 边界、`sideEffects`、barrel、按需、重复依赖 | 修掉 3 个失效点 | 初稿 |
| B5 资源与传输 | 图片、字体、CSS、gzip/brotli、sourcemap | 资源对比表 | 初稿 |
| B6 启动与构建速度 | 冷启动/HMR/构建分别慢在哪、插件开销、profile 定位 | 瓶颈报告 | 初稿 |

## 本部分预埋的坑

1. 全量 antd + moment + dayjs + lodash + echarts，单一巨大 vendor chunk
2. 一张 2.4MB 未压缩图片，首屏同步加载
3. `@lab/ui` 没有 `sideEffects`，barrel 全量 re-export
4. 配置里用 `define` 注入构建时间，导致 hash 抖动

## 闭卷考核（第 7 周末）

拿到一个陌生的 Vite 项目，30 分钟内给出「体积前三 + 速度瓶颈」的诊断，以及按优先级排序的三条建议。要求每条建议都能说出预期收益和验证方法。
