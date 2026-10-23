# 附录 D · 资源清单

> 只列官方文档和少量长文。链接会随版本移动；以当前大版本为准。
> 读法：先读“读哪节”，别从首页一路点到天亮。

## 1. 运行时与包管理

| 资源 | 读哪节 | 为什么读 |
|---|---|---|
| [Node.js 24 文档](https://nodejs.org/docs/latest-v24.x/api/) | `packages`、`esm`、`fs` | 理解 `exports`、ESM、脚本里的文件操作 |
| [Node.js 版本与发布](https://nodejs.org/en/about/previous-releases) | LTS/Current 表 | 解释为什么基线锁 Node 24 |
| [pnpm Workspaces](https://pnpm.io/workspaces) | Workspace 协议、`pnpm-workspace.yaml` | 本教程 Part 1 的底座 |
| [pnpm Settings](https://pnpm.io/settings) | `nodeLinker`、`allowBuilds`、`engineStrict`、store | pnpm 11+ 项目设置写在哪里 |
| [pnpm Filtering](https://pnpm.io/filtering) | `--filter`、`...`、`--fail-if-no-match` | 根脚本与 CI 只跑目标包 |
| [pnpm CI](https://pnpm.io/continuous-integration) | GitHub Actions、`--frozen-lockfile` | C2 的安装步骤 |
| [pnpm CLI](https://pnpm.io/pnpm-cli) | `why`、`list`、`approve-builds` | 排障与依赖审计 |

## 2. 构建：Vite 8 与 Rolldown

| 资源 | 读哪节 | 为什么读 |
|---|---|---|
| [Vite Guide](https://vite.dev/guide/) | 为什么选 Vite、开发/构建 | B1 的心智模型 |
| [Vite Config](https://vite.dev/config/) | `build`、`resolve`、`server`、`define` | B3/B4/B6 的配置入口 |
| [Vite Build](https://vite.dev/guide/build) | 构建、分包、base、library mode | 产物与部署路径 |
| [Vite Features](https://vite.dev/guide/features) | TS、CSS、资源、glob、Web Workers | B5 的资源处理 |
| [Vite Troubleshooting](https://vite.dev/guide/troubleshooting) | 配置加载、HMR、依赖预构建 | 排障速查的上游依据 |
| [Rolldown](https://rolldown.rs/) | 概览、配置、迁移 | Vite 8 的打包器；理解 `rolldownOptions` |
| [TypeScript tsconfig](https://www.typescriptlang.org/tsconfig/) | `moduleResolution`、`paths`、`rootDir`、`declaration` | M4/B2 的类型与产物 |
| [TypeScript Project References](https://www.typescriptlang.org/docs/handbook/project-references.html) | 什么时候用、`composite` | 多包类型检查 |
| [TypeScript Modules Reference](https://www.typescriptlang.org/docs/handbook/modules/reference.html) | `exports`、ESM/CJS 解析 | 跨包引用与库入口 |
| [ESLint Flat Config](https://eslint.org/docs/latest/use/configure/configuration-files) | 配置文件、ignores、语言选项 | M5 的根配置 |

## 3. CI/CD：GitHub Actions 与 GitLab

| 资源 | 读哪节 | 为什么读 |
|---|---|---|
| [GitHub Actions Workflow Syntax](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax) | `on`、`jobs`、`steps`、`permissions`、`concurrency` | C2/C6 的完整依据 |
| [Automatic token authentication](https://docs.github.com/en/actions/security-for-github-actions/security-guides/automatic-token-authentication) | `GITHUB_TOKEN` 权限 | 为什么默认只读、何时加 `packages: write` |
| [GitHub Container Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry) | 登录、推送、拉取、可见性 | C4/C5 的 GHCR |
| [Actions Dependency Caching](https://docs.github.com/en/actions/reference/workflows-and-actions/dependency-caching) | cache key、restore/save | C6 的缓存命中 |
| [Security hardening for Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions) | pin action、最小权限、脚本注入 | C6 的安全自查 |
| [GitLab CI/CD YAML](https://docs.gitlab.com/ci/yaml/) | `stages`、`rules`、`cache`、`artifacts`、`needs` | 附录 F 的对照实现 |
| [GitLab Runner Register](https://docs.gitlab.com/runner/register/) | docker executor、tags | 附录 F 的 runner |
| [GitLab Container Registry](https://docs.gitlab.com/user/packages/container_registry/) | 登录、deploy token、可见性 | 附录 F 的 registry |

## 4. 容器、Nginx 与 NAS

| 资源 | 读哪节 | 为什么读 |
|---|---|---|
| [Docker Build Best Practices](https://docs.docker.com/build/building/best-practices/) | 多阶段、`.dockerignore`、缓存 | C4 的镜像 |
| [Docker Multi-stage Builds](https://docs.docker.com/build/building/multi-stage/) | 命名阶段、`COPY --from` | 为什么最终镜像不含 Node |
| [Dockerfile Reference](https://docs.docker.com/reference/dockerfile/) | `ARG`、`COPY`、`HEALTHCHECK`、`EXPOSE` | 读 C4 的 Dockerfile |
| [Nginx `ngx_http_headers_module`](https://nginx.org/en/docs/http/ngx_http_headers_module.html) | `add_header`、继承规则 | 缓存头为什么“没生效” |
| [Nginx `ngx_http_core_module`](https://nginx.org/en/docs/http/ngx_http_core_module.html) | `location`、`try_files`、`root` | SPA fallback 与 `/healthz` 误判 |
| [QNAP Container Station 3](https://www.qnap.com/en/how-to/faq/article/how-to-use-container-station-3) | 创建容器、Compose、共享文件夹 | NAS 侧操作 |
| [QNAP QTS 5.2 文档](https://docs.qnap.com/operating-system/qts/5.2.x/en-us/) | 共享文件夹、用户、SSH | 部署目录与权限 |

## 5. Web 性能与缓存

| 资源 | 读哪节 | 为什么读 |
|---|---|---|
| [MDN Cache-Control](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) | 指令、`immutable`、`no-cache` | B5/C4 的缓存策略 |
| [web.dev HTTP cache](https://web.dev/articles/http-cache) | 缓存决策、指纹、失效 | 为什么 HTML 与 assets 策略不同 |
| [web.dev Optimize LCP](https://web.dev/articles/optimize-lcp) | 图片、预加载、传输 | B5 的资源取舍 |
| [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview) | 性能/可访问性报告 | 补充观察，不替代同机基线 |

## 6. 怎么读

1. 先读每章的“读哪节”，不要通读整站；
2. 打开文档后先看目录，确认当前大版本；
3. 遇到版本差异，以本机 `pnpm why` / `vite --version` / `docker version` 为准；
4. 把关键结论写进 `reports/`，附链接和日期；
5. 文档与教程冲突时，先复现，再判断是版本变化还是教程错误。

## 7. 不建议作为主资料的来源

- 二手“10 分钟学会”文章：版本容易过期；
- 复制来的 `vite.config`：不知道每个选项为什么存在；
- 没有命令和版本的性能结论；
- 只讲“最佳实践”不讲失败模式的清单。
