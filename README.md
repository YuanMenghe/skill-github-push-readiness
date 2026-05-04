# skill-github-push-readiness

[English summary](#english-summary)

面向 **推代码到 GitHub 前后** 的清单型 [Agent Skill](https://agentskills.io/)：检查 README/文档是否跟上变更、本地配置与 CI/GitHub Pages 是否一致、仓库 About 与主页链接、工作流与安全发布等。技能正文为中文，见 [`skills/github-push-readiness/SKILL.md`](skills/github-push-readiness/SKILL.md)。

---

## 简体中文说明

### 这是什么

把「推上 GitHub 才想起来」的事项收成一条 Agent 可执行流程：对照 **A→G** 分组（文档、CI 对齐、`.github`、Pages、对外入口、发布与安全、杂项），输出 **已通过 / 需改仓库 / 需去 GitHub 网页设置 / 建议** 四类结论。适合在发 PR、合并前、发版、或刚改完 Pages/文档时让 AI 跑一遍。

### 仓库结构

```text
skill-github-push-readiness/
├── README.md                          # 本说明（中英）
├── LICENSE                            # MIT
└── skills/
    └── github-push-readiness/
        └── SKILL.md                   # 技能本体（清单 + 输出模板）
```

安装时只需使用 **`github-push-readiness` 整个文件夹**（内含 `SKILL.md`），复制或符号链接到各产品规定的 skills 目录即可。

### 如何安装（按你用的工具选一行）

标准约定：每个 skill 一个目录，目录里必须有 **`SKILL.md`**，顶部为 YAML（至少含 `name`、`description`），下面为 Markdown 指令。

| 工具 | 常见安装位置 | 官方文档 |
| --- | --- | --- |
| **Cursor** | 用户级 `~/.cursor/skills/`，或仓库内 `<项目>/.cursor/skills/` | [Cursor Skills](https://cursor.com/docs/context/skills) |
| **Claude Code** | `~/.claude/skills/` 或 `<项目>/.claude/skills/` | [Claude Code Skills](https://code.claude.com/docs/en/skills) |
| **OpenAI Codex** | `~/.codex/skills/`（Windows 多为 `%USERPROFILE%\.codex\skills\`） | [Codex Skills](https://developers.openai.com/codex/skills/) |
| **OpenCode** | 以 [OpenCode 文档](https://opencode.ai/docs/skills/) 为准，多为项目级路径 | 同上 |

其它声明兼容 [Agent Skills](https://agentskills.io/) 的客户端（VS Code Copilot、Gemini CLI、Goose、Roo Code 等）见 [客户端列表](https://agentskills.io/clients)；以各产品文档中的 **skills 目录名** 为准，部分工具修改后需重启会话。

### Windows 一键复制示例（PowerShell）

先克隆本仓库，再把 `$src` 指到克隆目录里的 `skills\github-push-readiness`：

```powershell
# git clone https://github.com/YuanMenghe/skill-github-push-readiness.git
$src = ".\skill-github-push-readiness\skills\github-push-readiness"
# Cursor（用户级）
New-Item -ItemType Directory -Force "$env:USERPROFILE\.cursor\skills\github-push-readiness" | Out-Null
Copy-Item -Recurse -Force "$src\*" "$env:USERPROFILE\.cursor\skills\github-push-readiness\"
# Claude Code（用户级）
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills\github-push-readiness" | Out-Null
Copy-Item -Recurse -Force "$src\*" "$env:USERPROFILE\.claude\skills\github-push-readiness\"
# Codex（用户级）
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex\skills\github-push-readiness" | Out-Null
Copy-Item -Recurse -Force "$src\*" "$env:USERPROFILE\.codex\skills\github-push-readiness\"
```

### 如何使用

- **Cursor**：在对话里通过 Skills 选择 `github-push-readiness`，或说明「按 GitHub 推送检查 skill 过一遍」。
- **Claude Code**：目录名即命令，可使用 `/github-push-readiness`；若 `description` 与当前任务匹配，也可能被自动加载。
- **Codex**：按 Codex 文档将 skill 放入技能目录后，用该产品支持的方式引用技能名称或描述。

### 兼容性说明

- **Frontmatter**：当前使用 `name`、`description`、`disable-model-invocation`（Claude Code / Cursor 等常见）。若某客户端报错，可删掉可选字段，**只保留 `name` 与 `description`**。
- **「OpenClaude」等分叉**：若与 Claude Code 技能布局一致，一般仍为 `.claude/skills/<技能名>/SKILL.md`；请以该产品的官方说明为准。

### 参与贡献

欢迎 Issue / PR。请保持 **`SKILL.md` 与具体编辑器无关**；各编辑器的路径与技巧写在 **本 README** 即可。

### 许可证

MIT，见 [LICENSE](LICENSE)。

---

## English summary

Portable **Agent Skill** for auditing GitHub-facing work before push (README/docs, CI parity, Pages, About links, workflows, release hygiene). Canonical file: [`skills/github-push-readiness/SKILL.md`](skills/github-push-readiness/SKILL.md) (instructions mainly in **Chinese**).

**Install:** Copy the folder `skills/github-push-readiness/` into your tool’s skills directory (see table and PowerShell snippet in the [简体中文说明](#简体中文说明) section above).

**Contributing:** Keep `SKILL.md` tool-agnostic; editor-specific notes belong in this README.

**License:** MIT — [LICENSE](LICENSE).
