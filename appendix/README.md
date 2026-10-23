# 附录

| 附录 | 用途 | 状态 |
|---|---|---|
| [A 验收总表](A-验收总表.md) | 每章可勾选项，每条带命令或可观察结果 | 初稿 |
| [B 排障速查](B-排障速查.md) | 按错误信息索引：现象 → 原因 → 修复 | 初稿 |
| [C 工具地图](C-工具地图.md) | Turborepo/Nx/Changesets/Rollup/Webpack/Rspack/Vercel/K8s：解决什么、何时再学 | 初稿 |
| [D 资源清单](D-资源清单.md) | 官方文档 + 少量长文，标注读哪一节 | 初稿 |
| [E 公司落地路线](E-公司落地路线.md) | 不改公司项目怎么练；渐进式落地顺序与说服材料 | 初稿 |
| [F GitLab CI 对照与迁移](F-GitLabCI对照与迁移.md) | 可运行的 `.gitlab-ci.yml`、runner、registry、差异坑、迁移清单 | 初稿 |

## 工具地图的写法（约定）

每个工具只回答四个问题，控制在 5 行内：

1. 它解决什么问题
2. 出现什么信号才值得学（例如：仓库 > 15 个包、CI 构建 > 10 分钟）
3. 和本教程已学内容的替换关系
4. 大概要花多久

不写安装步骤，不写配置示例，避免变成第二套教程。Docker 已进入正文，不在本附录里。

## F 的定位

不是「GitLab 简介」，而是**把 C2~C5 的流水线翻译成 GitLab CI 并能跑**：

- 概念映射表（workflow/job/matrix/cache/artifact/environment/secrets）
- 完整的 `.gitlab-ci.yml`：verify → build → test → docker → deploy
- self-managed runner 的注册、tag 选择、docker executor
- GitLab Container Registry 登录与 NAS 拉取
- 四个高频差异：`cache:policy`、artifacts 自动传递、`rules:changes`、MR 与 branch 双跑
- 一页迁移检查表，预计半天完成
