---
name: github-push-readiness
description: >-
  Audits repository and GitHub-facing artifacts before or after push: README/docs
  freshness, config parity (local vs CI/Pages), GitHub Pages and About links,
  workflows, security hygiene, and release metadata. Use when pushing to GitHub,
  opening a PR, preparing a release, setting up GitHub Pages, or when the user
  asks to verify repo consistency, 推送前检查, GitHub 配置对齐, or 仓库说明是否更新.
disable-model-invocation: false
---

# GitHub 推送与仓库对外一致性检查

在「准备推送 / 合并 PR / 发布版本 / 启用 Pages」等节点，按下面清单逐项核对。能读文件则读仓库内文件；仅存在于 GitHub 网页的设置（分支保护、Secrets、About 里的 Website）在报告中单独列为「需在 GitHub 网页确认」。

## 使用方式

1. 用 `git status`、`git diff`、`git log -1` 了解本次变更范围。
2. 按 **A→G** 顺序扫描；跳过明显不适用的项（如无 Pages 则跳过 D）。
3. 输出一份简短报告：**已满足** / **需改代码** / **需去 GitHub 设置** / **建议**。

---

## A. 文档与「项目说明」

| 检查项 | 做法 |
|--------|------|
| README | 相对上次发布或主分支：功能、安装、环境要求、常用命令是否仍正确；顶部徽章（CI、版本、许可证）链接是否仍指向本仓库/正确 workflow。 |
| 变更是否值得写进 README | 用户可见行为、破坏性变更、新环境变量、新脚本入口 → 应出现在 README 或 CHANGELOG。 |
| 项目级说明 | 是否存在 `CONTRIBUTING.md`、`CODE_OF_CONDUCT.md`、`SECURITY.md`（漏洞披露）、`LICENSE` 与 `package.json`/`pyproject.toml` 等声明的许可证一致。 |
| 深度文档 | `docs/`、`ARCHITECTURE.md`、`docs/README.md` 是否与代码结构/主要模块一致；若有 Docusaurus/VitePress/MkDocs，构建命令是否在 README 写明。 |
| API / 配置样例 | `.env.example`、`config.sample.*`、`application.example.yml` 是否包含本次新增的配置键且无真实密钥。 |

---

## B. 本地配置与 CI / 自动化对齐

| 检查项 | 做法 |
|--------|------|
| 语言/运行时版本 | Node：`.nvmrc` / `engines` vs workflow 的 `node-version`；Python：`.python-version` / `runtime.txt` vs CI；Rust：`rust-toolchain.toml` vs CI。 |
| 锁文件与安装命令 | `package-lock`/`pnpm-lock`/`yarn.lock`/`Poetry.lock`/`uv.lock` 是否随依赖变更提交；CI 使用的包管理器与仓库一致。 |
| 构建/测试命令 | `package.json` scripts、`Makefile`、CI 中 `npm test` / `pytest` 是否与本地默认一致。 |
| Lint / 格式 | `.eslintrc`、`ruff.toml`、`biome.json` 等若忽略路径变更，CI 是否仍覆盖新目录。 |
| Docker | `Dockerfile` 基础镜像版本、暴露端口、`docker-compose` 服务名与环境变量是否与 README 一致。 |

---

## C. `.github` 与工作流

| 检查项 | 做法 |
|--------|------|
| Workflows | `.github/workflows/*.yml`：触发分支（`main`/`master`）、路径过滤是否与当前分支名/目录结构一致；若用 `workflow_dispatch`，文档是否说明用法。 |
| Action 版本 | 是否 pinned（SHA 或明确版本），避免误用 `@main` 于生产关键路径（按团队规范）。 |
| Issue/PR 模板 | `.github/ISSUE_TEMPLATE/`、`.github/pull_request_template.md` 是否存在且字段仍适用。 |
| CODEOWNERS | 路径与当前目录结构是否匹配；新大目录是否应加 owner。 |
| Dependabot | `dependabot.yml` 的 ecosystem、目录、schedule 是否覆盖实际 manifest 位置。 |

---

## D. GitHub Pages

| 检查项 | 做法 |
|--------|------|
| 产物与源 | 静态站点：构建输出目录与 `actions/configure-pages` / `peaceiris/actions-gh-pages` 等配置一致；文档站：分支 `gh-pages` 或 `docs/` on default branch 与仓库 Settings → Pages 策略一致。 |
| Base URL / 子路径 | 若仓库为 `username.github.io/repo-name/`，检查 `base`、`homepage`、VitePress/Docusaurus `baseUrl`、Router `basename` 是否含正确前缀。 |
| 自定义域名 | `CNAME` 或 `static/CNAME` 是否存在；README 是否注明 DNS/HTTPS 由用户在 Settings 中完成。 |
| Pages 权限 | 使用 GITHUB_TOKEN 部署时，确认文档中说明：Settings → Actions → General → Workflow permissions（read and write、允许 GitHub Pages）。 |
| 404 / SPA | 单页应用是否在 Pages 上有 `404.html` 回退或等价方案。 |

---

## E. 「主页」与对外入口是否指向本站

在 GitHub **网页** 核对（仓库内无法完全验证）：

| 检查项 | 说明 |
|--------|------|
| Repository **About** | Website / Topics 是否指向正确 URL（生产站、Pages、文档站）。 |
| Profile README | 若用 `username/username` 展示作品集，是否包含本项目链接与一句描述。 |
| 组织/多仓库导航 | 若有「portal」或总索引仓库，是否加入新子项目链接。 |
| README「在线演示」 | Demo、Storybook、Swagger UI 链接是否仍可达。 |

---

## F. 发布与安全

| 检查项 | 做法 |
|--------|------|
| 版本与 changelog | `CHANGELOG.md` / GitHub Releases 是否与 `package.json` version、`Cargo.toml` version 等一致；破坏性变更是否标 semver major。 |
| 密钥与令牌 | `git diff` 中无 `.env`、无私钥、无长效 token；`grep` 常见误提交模式（如 `BEGIN RSA PRIVATE KEY`、`api_key=`）对暂存区可选扫。 |
| 子模块 / 子树 | `.gitmodules` URL 是否应为 HTTPS 公开可读；相对路径子模块在 fork 场景是否可接受。 |
| 大文件 / LFS | 是否误提交二进制；是否应使用 Git LFS 或 release 附件。 |

---

## G. 其他常见「推上去才想起来」项

- **默认分支名**：文档与 CI 仍写 `master` 但仓库已改为 `main`。
- **Required status checks**：新 workflow 名称是否在分支保护规则里勾选。
- **环境 (Environments)**：`environment: production` 是否在仓库 Settings → Environments 中配置 protection / secrets。
- **Pages 与 Actions 并发**：同一分支既有旧 `gh-pages` 提交又有 Actions 部署时，避免互相覆盖（文档说明单一来源）。
- **开源合规**：第三方许可证 `NOTICE`、前端资源版权、模型权重是否应 `.gitignore` 或大文件策略。
- **归档与弃用**：若仓库已弃用，README 顶部说明并链接替代项目。

---

## 输出模板（复制使用）

```markdown
## GitHub 推送就绪检查（摘要）

**变更范围**：[简述]

### 已通过
- …

### 需在仓库内修改
- …

### 需在 GitHub 网页完成
- …

### 建议（非阻塞）
- …
```

---

## 额外资源

若清单过长，可将某仓库的固定忽略项记入项目内 `.cursor/rules` 或本 skill 同目录下的 `reference.md`（可选）；保持 **SKILL.md** 为通用清单即可。跨工具安装路径见本仓库根目录 [README.md](../../README.md)。
