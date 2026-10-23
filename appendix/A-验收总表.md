# 附录 A · 验收总表

> 用法：每完成一章，只勾选你能**当场复现**的项。命令以各章正文为准；这里给最短证据。
> 原则：一条验收 = 一个命令或一个可观察结果，不接受“我理解了”。

## Part 0 · 准备

| 章 | 可勾选结果 | 命令 / 证据 |
|---|---|---|
| 00 学习地图 | 能画出 Monorepo、Vite、CI 三块的关系 | `reports/learning.md` 里有一张 Mermaid/手绘图 |
| 00 学习地图 | 知道 L1/L2/L3 的差别，选了自己的目标 | 文档里写下本周目标与验收 |
| 01 环境 | Node/pnpm 版本符合基线 | `node -v`、`pnpm -v`；`packageManager` 已锁定 |
| 01 环境 | 仓库能干净安装 | `pnpm install --frozen-lockfile` |
| 01 环境 | 有第一份本机数据 | `reports/` 下有基线记录（机器、命令、次数、中位数） |

## Part 1 · Monorepo：pnpm workspace

| 章 | 可勾选结果 | 命令 / 证据 |
|---|---|---|
| M1 该不该上 Monorepo | 能说出“不上”的条件 | `docs/m1-decision.md` 有决策卡与反例 |
| M1 该不该上 Monorepo | 选型不是“因为大厂都用” | 文档列出团队规模、发布节奏、权限边界 |
| M2 从零 | 4 个 workspace 包能被识别 | `pnpm -r list --depth -1` |
| M2 从零 | 根脚本可构建/启动 | `pnpm build`；`pnpm dev:admin` 打开页面 |
| M3 依赖模型 | 应用依赖声明在自己的 `package.json` | `pnpm --filter "@lab/admin" list --depth 0` |
| M3 依赖模型 | 能解释幽灵依赖为何本地能跑 | `pnpm why <pkg> -r` 的输出能对应到声明位置 |
| M4 跨包引用 | 源码直连包能被应用构建 | `pnpm --filter "@lab/admin" build` |
| M4 跨包引用 | 类型检查覆盖跨包引用 | `pnpm typecheck`（根脚本） |
| M5 统一规范 | 根目录一份 ESLint 能扫全仓 | `pnpm lint` |
| M5 统一规范 | 提交门禁真实生效 | 故意提交一个 lint 错误，commit 被拦；修复后通过 |
| M6 日常排障 | 能按“安装/构建/运行”定界 | `docs/m6-runbook.md` 有命令与判断表 |
| M6 日常排障 | 能安全清理缓存后恢复 | 清 `node_modules/.vite` 后 dev 仍可启动 |

## Part 2 · 构建：Vite 8

| 章 | 可勾选结果 | 命令 / 证据 |
|---|---|---|
| B1 心智模型 | 能解释 dev 与 build 的链路差异 | 一张链路图 + 能指出某插件在哪个阶段运行 |
| B1 心智模型 | 配置加载顺序清楚 | `pnpm --filter "@lab/admin" exec vite build --debug` 能看到实际配置 |
| B2 度量基线 | 有同机、同命令、三次中位数 | `reports/b2-baseline.md`；脚本输出 |
| B2 度量基线 | 能读 raw/gzip/brotli 与 chunk | `node tooling/bench/size-report.mjs apps/admin/dist` |
| B3 分包与缓存 | 业务改动只影响相关 chunk | 两次构建对比 `dist/assets` 文件名 |
| B3 分包与缓存 | 没有构建时间/随机值导致 hash 抖动 | 同内容构建两次，hash 一致 |
| B4 Tree shaking | 大依赖没有整包进入入口 | `size-report` 中入口体积与依赖图能对应 |
| B4 Tree shaking | 能说出一个失效点及修复 | `reports/` 有前后对比（同机同命令） |
| B5 资源与传输 | 图片有三档尺寸/格式对比 | `reports/` 有原始体积、输出体积、命令 |
| B5 资源与传输 | 能验证缓存/压缩头 | `curl.exe -H "Accept-Encoding: gzip" -I` 看到 `Cache-Control`/`Content-Encoding` |
| B6 启动与构建速度 | 有 dev 启动与 build 的三次中位数 | `node tooling/bench/dev-start.mjs 3 apps/admin` |
| B6 启动与构建速度 | 能用 profile 定位插件/阶段开销 | `vite build --profile` 生成 cpuprofile 并能指出热点 |

## Part 3 · CI/CD：GitHub Actions + Docker + NAS

| 章 | 可勾选结果 | 命令 / 证据 |
|---|---|---|
| C1 流水线设计 | 能画出 PR/main/部署三条链路 | `docs/pipeline.md` 有 Mermaid |
| C1 流水线设计 | 能解释工件、缓存、幂等、权限 | 文档中每条都有本仓库例子 |
| C2 Actions 基础 | PR 自动跑 verify | `gh pr checks` 看到 `CI / verify` |
| C2 Actions 基础 | 构建产物可下载 | Actions run 的 `web-dist` artifact |
| C3 质量门禁 | smoke 在坏产物上会失败 | `pnpm test` 通过；改坏 `dist` 后失败 |
| C3 质量门禁 | 体积预算真实拦截 | 超预算时脚本退出非 0；PR 红 |
| C3 质量门禁 | 分支保护要求该检查 | GitHub Rules/Branch protection 截图或设置记录 |
| C4 容器化 | 多阶段镜像可构建并运行 | `docker build`；`docker run -p 8080:8080`；`/healthz` 200 |
| C4 容器化 | 镜像架构与 NAS 一致 | `docker image inspect <img> --format '{{.Architecture}}'` = `amd64` |
| C5 部署与回滚 | NAS 能按 sha 部署 | `./deploy.sh <sha>`；`/version.json` 匹配 |
| C5 部署与回滚 | 回滚真实可用 | `./deploy.sh --rollback`；版本回到上一版 |
| C6 性能与安全 | 有 5 分钟目标的耗时分解 | `reports/c6-pipeline.md` |
| C6 性能与安全 | 权限最小化、Action 已 pin | workflow 中 `permissions` 与 `@<sha>` 可核对 |

## Part 4 · 整合

| 章 | 可勾选结果 | 命令 / 证据 |
|---|---|---|
| P1 脚手架 | 一条命令建包 | `pnpm gen:pkg @lab/logger`；目录已存在时拒绝 |
| P1 脚手架 | 一条命令建应用 | `pnpm gen:app reports`；`build` 通过 |
| P1 脚手架 | 新包零 ESLint 配置 | 新包无 `eslint.config.*`；根 `pnpm lint` 通过 |
| P1 脚手架 | 新人 30 分钟路径可走 | `docs/newcomer.md` + 一次真实计时 |
| P2 端到端 | issue → PR → 门禁 → merge 有链接 | `reports/p2-e2e.md` |
| P2 端到端 | 镜像 → NAS → 版本 → 回滚有证据 | `/version.json`、部署输出、回滚后版本 |
| P3 故障手册 | 10 个剧本能说出定位路径 | `reports/p3-drills.md` |
| P3 故障手册 | 至少 5 个真实复现并修回 | 每个有现象/诊断/根因/修复/验收 |

## 全局完成标准

- [ ] 从空仓库能按教程走到 NAS 部署（不依赖“种子脏仓库”）
- [ ] 所有性能结论都有机器、命令、次数、中位数
- [ ] PR 门禁真实拦截过坏代码
- [ ] 镜像 tag 与 main commit sha 一一对应
- [ ] 回滚在真实/测试 NAS 上执行过
- [ ] 模板仓库不含 secret、`node_modules`、`dist`
- [ ] 另一个人能按 `docs/newcomer.md` 独立跑起来

## 验收记录建议

```md
# 验收记录

- 日期：
- 机器：
- 仓库 sha：
- Node/pnpm：
- 完成章节：
- 未通过项及原因：
- 下一步：
```
