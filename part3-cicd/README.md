# Part 3 · CI/CD：GitHub Actions + Docker + NAS

**周期**：第 8~11 周 ｜ **目标层级**：L2，C1/C3/C6 到 L3

只用一个 CI 平台（GitHub Actions），把「提交 → 出镜像 → 手动上线到 QNAP」这条链路走通。GitLab 用附录 F 复刻，不重复教学。Docker 只讲把静态站点做成镜像并部署所需的部分。

| 章节 | 核心问题 | 产出 | 状态 |
|---|---|---|---|
| C1 流水线设计 | 阶段怎么分、工件与幂等、失败拦在哪层、GH/GL 概念映射 | 流水线设计图 | 初稿 |
| C2 Actions 基础 | 触发、job、matrix、concurrency、cache、artifact、secrets、permissions | PR 流水线跑通 | 初稿 |
| C3 质量门禁与性能预算 | lint/type/test/build 排序、size-limit、required checks | 门禁清单 + 真实拦截 | 初稿 |
| C4 容器化 | 多阶段构建、层缓存、构建期 vs 运行期变量、nginx 缓存头、健康检查、GHCR 推送、架构 | 30MB 级镜像 + 推送到 GHCR | 初稿 |
| C5 部署与回滚 | GHCR 拉取、QNAP Container Station、Compose 参数化、健康检查、回滚脚本、版本记录 | NAS 手动部署 + 回滚一次 | 初稿 |
| C6 流水线性能与安全 | 5 分钟目标、命中率、并行、pin SHA、最小权限、镜像与密钥安全 | 耗时分解表 + 安全自查 | 初稿 |

## 上线主线（持续交付，人工触发）

```
Actions: pnpm build → docker build → push GHCR（tag = commit sha，另打 latest 别名）
   ↓
你在 Actions 摘要 / Release 里确认要发的 sha
   ↓
SSH 到 QNAP（仅局域网）: cd /share/Container/fe-lab-admin && ./deploy.sh <sha>
   ↓
deploy.sh: 写 .env 的 IMAGE_TAG → docker compose pull → up -d
   ↓
校验 /healthz 200 + /version.json 的 sha 与目标一致
   ↓ 失败
用上一个 tag 重新 up -d，脚本退出码非 0，日志留在 NAS 上
```

不需要内网穿透；除局域网 SSH 管理外，不开放其他入站端口。NAS 只主动向 GHCR 发起出站 HTTPS。

## 本部分预埋的坑

1. 单阶段 Dockerfile，镜像 1.2GB（node_modules 和源码全带进去）
2. `index.html` 被设成一年缓存，发版后用户永远看到旧页面
3. 把 `VITE_API_BASE` 当成运行期环境变量，改容器配置不生效
4. 部署脚本先停旧容器再起新容器，新镜像起不来时线上直接空白
5. `latest` 作为唯一 tag，回滚时不知道上一版是哪个
6. 健康检查探的是 80 端口，但 nginx 监听 8080，部署永远「成功」
7. CI 构建的是 `linux/arm64`，NAS 是 x86_64，启动即 `exec format error`
8. 用 `docker-compose`（带连字符）在 Container Station 3 上找不到命令
9. 私有 GHCR 镜像在 Container Station UI 里拉取超时，命令行才行
10. 本地 pnpm 12 / CI pnpm 版本不一致，`--frozen-lockfile` 失败

## 闭卷考核（第 11 周末）

从空仓库写一条流水线：PR 跑完验证检查且不发布；合并后构建镜像并推到 GHCR；在 NAS 上一条命令完成部署与健康校验；部署失败能用上一个 tag 回滚；能说清「为什么回滚不能依赖 `latest`」。
