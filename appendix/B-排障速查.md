# 附录 B · 排障速查

> 按“你看到的错误/现象”索引。先定界，再修复；不要一上来删 `node_modules`、清缓存、重装。
> 命令默认 PowerShell；NAS 上的命令用 bash。

## 0. 30 秒定界

| 现象 | 先查 | 第一命令 |
|---|---|---|
| 本地也失败 | 依赖/配置/代码 | `pnpm install --frozen-lockfile` |
| 本地好、CI 坏 | 环境/锁文件/权限/缓存 | 看失败 step 的第一条错误 |
| CI 好、NAS 坏 | 镜像/认证/架构/端口 | `docker compose ps`、`docker compose logs` |
| NAS 好、浏览器坏 | 缓存/HTML/资源路径 | `curl.exe -I`、DevTools Network |
| 只有部分用户坏 | 缓存/CDN/旧 Service Worker | 无痕窗口 + `version.json` |

## 1. pnpm 与依赖

| 现象/错误 | 最可能原因 | 先跑 | 修复 |
|---|---|---|---|
| `ERR_PNPM_OUTDATED_LOCKFILE` | `package.json` 改了，lockfile 没提交 | `pnpm install`；`git diff pnpm-lock.yaml` | 提交同步后的 lockfile；CI 保持 `--frozen-lockfile` |
| `ERR_PNPM_IGNORED_BUILDS` | pnpm 12 默认拦截依赖的安装脚本 | `pnpm approve-builds --all` 或看 `pnpm-workspace.yaml` | 只批准需要的（如 `esbuild`）；写入 `allowBuilds` |
| `ERR_PNPM_NO_MATCHING_VERSION` | 版本/catalog 不存在或拼错 | `pnpm view <pkg> versions --json` | 改版本或补 catalog；不要用不存在的 tag |
| `ERR_PNPM_PEER_DEP_ISSUES` | peer 版本不匹配 | `pnpm why <pkg>`；`pnpm list -r --depth 1` | 对齐版本；必要时在根 `pnpm.overrides` 统一 |
| `ERR_PNPM_WORKSPACE_PKG_NOT_FOUND` | workspace 包名/路径写错 | `pnpm -r list --depth -1` | 对齐 `package.json` 的 `name` 与依赖声明 |
| `Cannot find module 'x'` / `Failed to resolve import` | 幽灵依赖、漏声明、路径错 | `pnpm --filter <app> why x` | 在真正使用它的包 `pnpm --filter <app> add x` |
| `EACCES` / `EPERM`（Windows 符号链接） | 无符号链接权限 | `pnpm install` 的完整错误 | 开启开发者模式/管理员终端；或按 01 章调整 `nodeLinker` |
| `ELIFECYCLE Command failed` | 某个 package script 失败 | 往上看第一条真实错误 | 修脚本；不要只看 `ELIFECYCLE` |
| 安装很慢/反复下载 | store 不命中、registry 慢 | `pnpm store path`；`pnpm config get registry` | 固定 registry；CI 缓存 pnpm store；不要提交 store |

## 2. TypeScript 与 Vite

| 现象/错误 | 最可能原因 | 先跑 | 修复 |
|---|---|---|---|
| `TS2307: Cannot find module` | 路径/类型/依赖/`exports` 不对 | `pnpm --filter <pkg> exec tsc --noEmit --traceResolution` | 修 `paths`/依赖/`exports`；不要用 `any` 盖住 |
| `TS6059: File is not under rootDir` | `rootDir` 与 include 冲突 | 看报错文件与 `tsconfig` | 调整 `rootDir`/`include`；库构建单独 `tsconfig.build.json` |
| `TS6377: Cannot write file ... because it would overwrite input` | `outDir` 指到源码/构建信息脏 | `tsc -b --clean`；看 `outDir` | 分离 `outDir`；清 `.tsbuildinfo` 后重跑 |
| `TS2742: inferred type cannot be named` | 声明生成时类型不可命名 | `pnpm --filter <pkg> build` | 显式导出类型/用 `import type`；不要只加 `skipLibCheck` |
| `Failed to resolve import` | 别名、扩展名、包未安装 | `vite build --debug` | 修 alias/依赖；Vite 8 用 `resolve.alias` |
| 配置改了不生效 | `vite.config.js` 优先于 `.ts` | `vite build --debug` 看 loaded config | 删除重复配置，只留一份 |
| `build.rollupOptions` 弃用警告 | Vite 8 用 Rolldown | 看 warning | 改 `build.rolldownOptions` |
| `manualChunks` 不生效 | Vite 8 分包 API 变了 | 看 build 输出/文档 | 用 `output.codeSplitting.groups`，路径分隔符写 `[\\/]` |
| 页面报重复 React/无效 hook | 多份 React | `pnpm why react`；`pnpm list -r react` | 对齐版本；根 `pnpm.overrides` 统一 |
| `JavaScript heap out of memory` | 构建/分析器内存不足 | 看是否开了 analyzer | 临时 `NODE_OPTIONS=--max-old-space-size=4096`；关掉 analyzer 再测 |
| `[vite] pre-transform error` / 改了没反应 | `node_modules/.vite` 旧缓存 | 停 dev；删 `node_modules/.vite` | 重启 dev；仍失败看真实堆栈 |
| `does not provide an export named` | ESM/CJS 混用或入口不对 | 看包 `exports`/`type` | 改入口或换导入方式；不要只看 Vite 报错 |
| `Cannot use import statement outside a module` | Node/CJS 配置不匹配 | 看 `package.json` 的 `type` | 改 `.mjs`/`type: module` 或构建配置 |

## 3. GitHub Actions

| 现象/错误 | 最可能原因 | 先跑 | 修复 |
|---|---|---|---|
| `pnpm: command not found` | 没装 pnpm / 顺序错 | 看 step 顺序 | 先 `pnpm/action-setup`，再 `setup-node`，再 install |
| `ERR_PNPM_OUTDATED_LOCKFILE`（CI） | lockfile 漂移 | 本地 `pnpm install` | 提交 lockfile；不要在 CI 去掉 frozen |
| `Artifact not found` / 下载为空 | `path` 写错或构建没产出 | 看 upload step 日志 | 对齐 `apps/admin/dist`；先 `Test-Path` |
| `Resource not accessible by integration` | `GITHUB_TOKEN` 权限不足 | 看 workflow `permissions` | 加 `contents: read` / `packages: write`，按需最小化 |
| `denied: permission_denied`（GHCR） | 包权限/可见性/组织策略 | 看 publish 日志 | 开 `packages: write`；检查包设置与 SSO |
| `exec format error` | 镜像架构与运行机不一致 | `docker image inspect ... Architecture` | 构建 `linux/amd64`；不要只靠本机默认 |
| `No space left on device` | runner 磁盘满 | 看 df/缓存 | 清无用缓存/产物；缩小 artifact；换 runner |
| `git` checkout 失败 | 权限/子模块/LFS | 看 checkout 日志 | 配 token/子模块；大文件不要进 Git |
| 缓存一直不命中 | key 变了/缓存范围不对 | 看 cache step 的 key | key 绑 lockfile；不要缓存 `node_modules`（pnpm 用 store） |
| 并发部署互相覆盖 | 没有 concurrency | 看 run 列表 | 加 `concurrency`；部署串行 |

## 4. Docker、GHCR 与 QNAP

| 现象/错误 | 最可能原因 | 先跑 | 修复 |
|---|---|---|---|
| `pull access denied` | 未登录/无权限/包私有 | `docker login ghcr.io` | NAS 用 `read:packages` 凭据；检查包可见性 |
| `manifest unknown` | tag 不存在/被清理/拼错 | `docker manifest inspect <img>:<tag>` | 用存在的 sha；恢复保留策略 |
| `port is already allocated` | 端口被旧容器/其他服务占用 | `docker ps`；`netstat -ano \| findstr :8088` | 停旧容器/换端口；不要 `down -v` 误删数据 |
| `unhealthy` | 健康检查路径/端口/启动时间不对 | `docker inspect` 的 Health.Log | 修 `/healthz` 精确响应；加 `start_period` |
| `permission denied ... docker.sock` | 用户不在 docker 组/无权限 | `docker info` | 用有权限用户；不要长期 root |
| `docker compose: command not found` | QNAP 只有 `docker compose` V2 | `docker compose version` | 用 V2 语法；不要装旧 `docker-compose` |
| `Permission denied (publickey)` | SSH key 没配/用户不对 | `ssh -vvv qnap` | 配公钥、端口、用户；不要开密码登录 |
| `401 Unauthorized` | token 过期/缺 scope | `docker login` 输出 | 换 token；`read:packages`；检查 SSO |
| `x509: certificate signed by unknown authority` | 代理/自签证书/时钟 | `curl -I` 同地址 | 修代理/证书/时间；不要全局 `--insecure` |
| `container name already in use` | 旧容器未删/Compose 项目名不同 | `docker ps -a` | 用同一 Compose 项目；确认后删旧容器 |
| 镜像拉取很慢/失败 | 网络/镜像大/架构 | `docker pull` 单独跑 | 缩小镜像；配代理；确认 `linux/amd64` |

## 5. Nginx 与浏览器

| 现象/错误 | 最可能原因 | 先跑 | 修复 |
|---|---|---|---|
| 发布后仍旧页面 | HTML 被长缓存/Service Worker | `curl.exe -I /`；无痕窗口 | `index.html` 用 `no-cache`；`version.json` 用 `no-cache`（或 `no-store`） |
| 刷新后资源 404 | `base`/路径/旧 HTML 指向旧 hash | Network 面板看请求路径 | 对齐 Vite `base` 与 Nginx `root`；不要手改 hash |
| 502 Bad Gateway | 上游没起/端口错 | `docker compose ps`；`logs` | 修端口/启动；健康检查先通过 |
| `/healthz` 200 但页面坏 | SPA fallback 把未知路径返回 200 | `curl -i /healthz` 看 body | 精确 `location = /healthz` 返回 `ok` |
| 静态资源不压缩 | Nginx 没开 gzip/brotli | `curl -H "Accept-Encoding: gzip" -I` | 开 gzip；预压缩按 B5 |
| 缓存头不生效 | `add_header` 被子 location 覆盖 | `curl -I` 实际响应 | 在最终 location 显式写全部需要的头 |
| 只有部分用户旧版 | CDN/代理/多副本 | 对比不同网络/节点 | 清 CDN；统一缓存策略；版本校验 |

## 6. 万能但危险的动作

| 动作 | 什么时候可以用 | 风险 |
|---|---|---|
| 删 `node_modules` | 确认是本地安装损坏 | 丢失现场；不能证明根因 |
| 删 `pnpm-lock.yaml` | 几乎不要 | 依赖漂移、无法复现 |
| `docker system prune -a` | 确认没有要保留的镜像/容器 | 删掉回滚镜像 |
| `latest` 部署 | 不要用于生产 | 无法对应 sha，无法回滚 |
| 关闭 required check | 紧急且有人批准 | 坏代码进 main |
| `--no-verify` | 几乎不要 | 绕过 lint/commit 门禁 |
| 生产上直接改容器 | 不要 | 与镜像/Compose 不一致，下次部署丢失 |

## 7. 一条排障记录模板

```md
- 时间：
- 现象：
- 影响范围：
- 版本/sha：
- 定界（本地/CI/NAS/浏览器）：
- 诊断命令与输出：
- 根因：
- 修复：
- 验证：
- 防复发：
- 耗时：
```
