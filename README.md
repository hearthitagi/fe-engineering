# 前端工程化实操教程

> 12 周，只学三件事：**pnpm workspace**、**Vite 8**、**GitHub Actions**。
> 每章 = 知识讲解 + 跟做实操 + 只给验收的闭卷练习 + 故障演练 + 自测。

---

## 这套教程是什么

- 一条**可以照做**的路径：每步有命令、有预期输出、有失败处理
- 一个**从零搭建**的实验仓库 `fe-lab`：不提供预埋脏数据的种子仓库；每章按步骤制造问题、观察现象、再修复，故障成为可重复的练习
- 一套**可复现的度量**：构建时长、产物大小、chunk 数，全部由 `tooling/bench` 产出，不写「提升明显」
- 一份**可交付的结果**：第 12 周你会有一个能给别人用的仓库模板 + 一条能上线的流水线

## 这套教程不是什么

- 不是工具百科。Turborepo、Nx、Changesets、Webpack、Rspack、Vercel、K8s 都不教，只在地图里告诉你「什么时候再回来看」
- 不是 DevOps 全集。Docker 只讲「把静态站点做成镜像并部署到 NAS」所需的部分，不讲编排、Swarm、网络模式
- 不是配置速查。配置只在解释机制时出现
- 不覆盖框架原理、业务开发、后端、K8s

---

## 12 周路线

| 周 | 内容 | 周末产出 |
|---|---|---|
| 1 | 学习地图、环境、选型、pnpm workspace 起步 | 4 包仓库跑通，第一份构建数据 |
| 2 | 依赖模型、包边界、跨包引用与类型 | 依赖体检报告，两种引用方案对比 |
| 3 | 统一规范、日常排障 + 闭卷考核 | 新包零配置接入，30 分钟重建仓库 |
| 4 | Vite 心智模型、度量基线 | 链路图，基线报告 |
| 5 | 分包与长效缓存 | 改业务代码只变业务 chunk 的 hash |
| 6 | Tree shaking、资源与传输 | 三个失效点修复，资源对比表 |
| 7 | 构建与启动速度 + 综合报告 + 闭卷考核 | 体积/速度前后对比，陌生项目诊断 |
| 8 | 流水线设计、Actions 基础 | PR 自动检查跑通 |
| 9 | 质量门禁与性能预算 | 坏代码被真实拦住 |
| 10 | 容器化：Vite 产物 → 镜像 | 镜像构建成功，本地 Compose 跑通 |
| 11 | NAS 部署与回滚 + 流水线性能与安全 | 手动拉取部署到 NAS，回滚成功，耗时分解表 |
| 12 | 脚手架、端到端演练、故障手册 | 模板仓库，10 个故障剧本 |

每周 8~12 小时。基础章节（M2/M3/B3/B4/C2/C4）建议留两个晚上，不要赶。

---

## 目录

### Part 0 · 准备
- [00 学习地图与使用方法](00-学习地图与使用方法.md)（初稿）
- [01 环境与实验仓库](01-环境与实验仓库.md)（初稿）

### Part 1 · Monorepo：pnpm workspace
- [M1 该不该上 Monorepo](part1-monorepo/M1-该不该上Monorepo.md)（初稿）
- [M2 pnpm workspace 从零](part1-monorepo/M2-pnpm-workspace从零.md)（初稿）
- [M3 依赖模型与包边界](part1-monorepo/M3-依赖模型与包边界.md)（初稿）
- [M4 跨包引用与类型](part1-monorepo/M4-跨包引用与类型.md)（初稿）
- [M5 统一规范与提交门禁](part1-monorepo/M5-统一规范与提交门禁.md)（初稿）
- [M6 日常操作与排障](part1-monorepo/M6-日常操作与排障.md)（初稿）

### Part 2 · 构建：Vite 8
- [B1 Vite 心智模型](part2-vite/B1-Vite心智模型.md)（初稿）
- [B2 度量与基线](part2-vite/B2-度量与基线.md)（初稿）
- [B3 分包与长效缓存](part2-vite/B3-分包与长效缓存.md)（初稿）
- [B4 Tree shaking 与依赖治理](part2-vite/B4-TreeShaking与依赖治理.md)（初稿）
- [B5 资源与传输](part2-vite/B5-资源与传输.md)（初稿）
- [B6 启动与构建速度](part2-vite/B6-启动与构建速度.md)（初稿）

### Part 3 · CI/CD：GitHub Actions + Docker + NAS
- [C1 流水线设计](part3-cicd/C1-流水线设计.md)（初稿）
- [C2 GitHub Actions 基础](part3-cicd/C2-GitHubActions基础.md)（初稿）
- [C3 质量门禁与性能预算](part3-cicd/C3-质量门禁与性能预算.md)（初稿）
- [C4 容器化](part3-cicd/C4-容器化.md)（初稿）
- [C5 部署与回滚](part3-cicd/C5-部署与回滚.md)（初稿）
- [C6 流水线性能与安全](part3-cicd/C6-流水线性能与安全.md)（初稿）

### Part 4 · 整合
- [P1 脚手架与配置预设](part4-capstone/P1-脚手架与配置预设.md)（初稿）
- [P2 端到端演练](part4-capstone/P2-端到端演练.md)（初稿）
- [P3 故障演练手册](part4-capstone/P3-故障演练手册.md)（初稿）

### 附录
- [A 验收总表](appendix/A-验收总表.md)（初稿）
- [B 排障速查](appendix/B-排障速查.md)（初稿）
- [C 工具地图](appendix/C-工具地图.md)（初稿）
- [D 资源清单](appendix/D-资源清单.md)（初稿）
- [E 公司落地路线](appendix/E-公司落地路线.md)（初稿）
- [F GitLab CI 对照与迁移](appendix/F-GitLabCI对照与迁移.md)（初稿）

---

## 每章怎么用

1. **先看终点**：跳过正文，先看第 0 节的命令与输出，知道自己要做出什么
2. **读知识讲解**：重点看「失败模式」和「常见误解」，这两节决定你能不能排障
3. **跟做 Guided**：命令自己敲，不要复制粘贴；每步对照预期输出
4. **做闭卷练习 Transfer**：合上教程，只按验收标准做。卡住再看三级提示
5. **做故障演练**：故意弄坏，观察现象，修回来，写下一条规范
6. **闭卷自测**：5 道题 + 用 3 句话解释给非前端听
7. **记录数据**：填进 `reports/`，前后对比要有同一台机器、同一命令

---

## 环境基线

| 项 | 版本 | 备注 |
|---|---|---|
| Node | 24 LTS | 本机 24.14 |
| pnpm | 12.15 | 由 `packageManager` 字段 + corepack 锁定；项目设置写在 `pnpm-workspace.yaml` |
| 构建 | Vite 8.x | Rolldown + Oxc，注意 `rollupOptions` 已弃用 |
| UI | React 19 | |
| 语言 | TypeScript 6.x（strict） | 与 create-vite 9 模板一致 |
| 规范 | ESLint 9 flat config | |
| CI | GitHub Actions | 公司 GitLab 用附录 F 迁移 |
| 部署 | Docker + Compose，运行在自有 NAS | 镜像走 GHCR |
| NAS | QNAP QTS 5.2.10 / Container Station 3.1.2 / x86_64 | SSH 端口 33456，仅局域网；镜像 `linux/amd64` |

Windows 用户注意：`mkdir -p`、`NODE_ENV=x cmd`、`&&` 链式写法在 PowerShell 里行为不同。教程主线给 PowerShell，bash 写法放在折叠块里；符号链接、CRLF、长路径问题在 01 章统一处理。

---

## 命令与代码约定

- 每章的命令都在基线环境实测过，代码块标注文件名
- 不在关键配置里用 `...` 省略
- 每个命令都给「你应该看到什么」
- 版本敏感处给 `<details>` 折叠块，写清旧版本的差异
- 所有性能数字附：机器、命令、次数、中位数
