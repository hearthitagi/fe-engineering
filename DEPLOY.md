# 部署到 GitHub Pages

本站是纯静态站点：`index.html` 加载 Docsify，浏览器直接读取同目录下的 Markdown。**不需要构建，也不需要 GitHub Actions。** 推送 Markdown 后，GitHub Pages 会自动重新发布。

## 先选仓库名

GitHub Pages 的地址由仓库名决定：

| 目标地址 | 仓库名 | 说明 |
|---|---|---|
| `https://<用户名>.github.io/` | `<用户名>.github.io` | 用户主页，站点在根路径。想用 `xxx.github.io` 直接访问，选这个 |
| `https://<用户名>.github.io/fe-engineering/` | `fe-engineering` | 项目主页，地址会多一层路径 |

下面的命令以「用户主页」为例。把 `<用户名>` 换成你的 GitHub 用户名。

## 第一次发布

### 1. 在 GitHub 建空仓库

打开 <https://github.com/new>，仓库名填 `<用户名>.github.io`，例如 `alice.github.io`。

- 选择 Public
- **不要**勾选 Add a README file、Add .gitignore、Choose a license
- 点击 Create repository

### 2. 在本机提交并推送

在 PowerShell 里执行：

```powershell
cd E:\1code\study\fe-engineering

git init -b main
git add .
git commit -m "docs: publish tutorial site"

# HTTPS 方式；如果已配置 SSH，也可以换成 git@github.com:<用户名>/<用户名>.github.io.git
git remote add origin https://github.com/<用户名>/<用户名>.github.io.git
git push -u origin main
```

第一次推送时，Git 会要求登录 GitHub。推荐用 Git Credential Manager 或 Personal Access Token，不要把 token 写进仓库。

### 3. 打开 Pages

进入仓库的 **Settings → Pages**：

1. 在 **Build and deployment** 下，把 **Source** 选为 **Deploy from a branch**
2. **Branch** 选 `main`，目录选 `/ (root)`
3. 点击 **Save**

等待 1～2 分钟，访问：

```text
https://<用户名>.github.io/
```

第一次发布有时要等几分钟；可以在 **Actions** 标签页看到 `pages build and deployment` 的进度。

## 日常更新

改完 Markdown 后：

```powershell
cd E:\1code\study\fe-engineering
git add .
git commit -m "docs: update chapter"
git push
```

Pages 会自动重新发布，不需要手动点部署。

## 本地预览

Docsify 通过 HTTP 请求读取 Markdown，直接双击 `index.html`（`file://`）会因浏览器安全策略加载失败。请起一个本地静态服务器：

```powershell
cd E:\1code\study\fe-engineering
npx --yes serve . -l 4173
```

然后打开 <http://localhost:4173/>。

如果不想用 `npx`，也可以用 Python：

```powershell
python -m http.server 4173
```

## 这个站点的结构

```text
fe-engineering/
├─ index.html          # Docsify 入口、主题和插件配置
├─ _sidebar.md         # 全站侧边栏
├─ .nojekyll           # 让 GitHub Pages 原样提供 _sidebar.md 等以下划线开头的文件
├─ README.md           # 首页
├─ assets/docsify/     # 本地化的 Docsify、搜索、Mermaid，避免依赖 CDN
├─ part1-monorepo/
├─ part2-vite/
├─ part3-cicd/
├─ part4-capstone/
└─ appendix/
```

> 本站只发布 `fe-engineering/` 目录。上级的《工程化教程-设计方案-v3.md》是内部规划，不随站点发布。

## 常见问题

### 打开首页只看到 `正在加载教程……`

通常是 Markdown 请求失败。按 F12 看 Console/Network：

- `README.md` 404：确认 Pages 的目录是 `/ (root)`，且文件在仓库根目录
- `_sidebar.md` 404：确认根目录有 `.nojekyll`，并确认 Pages 已重新发布
- 本地打开：确认用的是 `http://localhost:4173/`，不是 `file://`

### 访问 `https://<用户名>.github.io/` 是 404

- 仓库名必须**完全等于** `<用户名>.github.io`
- 用户名和仓库名都要用你的 GitHub 账号名，大小写不敏感但拼写要一致
- Pages 第一次发布尚未完成，等几分钟再看

### 访问 `https://<用户名>.github.io/fe-engineering/` 才对

说明仓库名是 `fe-engineering`，这是项目主页，不是用户主页。想用根路径访问，需要把内容放到名为 `<用户名>.github.io` 的仓库。

### 中文文件名或链接 404

GitHub Pages 支持 UTF-8 路径。确认：

- 文件名与 `_sidebar.md`、`README.md` 中的链接大小写、空格、标点完全一致
- URL 中的中文被浏览器编码后仍能对应到实际文件
- 不是用 `file://` 打开的本地页面

### 改了 Markdown，页面还是旧的

GitHub Pages 的 CDN 有缓存。先强制刷新（Ctrl+F5），再确认 Actions 里的发布任务已经成功。`index.html` 和 Markdown 不是带 hash 的静态资源，不要给它们设置长期缓存。

## 可选：项目主页

如果不想新建 `<用户名>.github.io`，也可以把 `fe-engineering` 直接作为项目仓库发布：

1. GitHub 新建仓库 `fe-engineering`
2. 按上面的命令推送
3. Settings → Pages → Deploy from a branch → `main` / `(root)`
4. 访问 `https://<用户名>.github.io/fe-engineering/`

功能完全一样，只是地址多一层路径。教程里的相对链接不受影响。
