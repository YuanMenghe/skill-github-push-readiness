# skill-github-push-readiness

Portable [Agent Skill](https://agentskills.io/) for auditing **GitHub-facing** work before you push: README and docs freshness, CI vs local config parity, GitHub Pages, repository **About** / homepage links, workflows, and release hygiene.

Canonical skill file: [`skills/github-push-readiness/SKILL.md`](skills/github-push-readiness/SKILL.md).

## Install (pick your tool)

Layout is standard: one directory per skill containing `SKILL.md` with YAML frontmatter (`name`, `description`, …) plus Markdown instructions. Copy the folder `github-push-readiness` (the directory that contains `SKILL.md`) into your product’s skills directory, or symlink it.

| Product | Typical skills directory | Official docs |
| --- | --- | --- |
| **Cursor** | `~/.cursor/skills/` (user) or `<repo>/.cursor/skills/` (project) | [Cursor Skills](https://cursor.com/docs/context/skills) |
| **Claude Code** | `~/.claude/skills/` or `<repo>/.claude/skills/` | [Claude Code Skills](https://code.claude.com/docs/en/skills) |
| **OpenAI Codex** | `~/.codex/skills/` (see Codex docs for OS paths) | [Codex Skills](https://developers.openai.com/codex/skills/) |
| **OpenCode** | Follow OpenCode’s skills path (often project-level) | [OpenCode Skills](https://opencode.ai/docs/skills/) |

**Windows quick copy (PowerShell)** — adjust `YOUR_GITHUB_USER` if you clone with HTTPS/SSH under another path:

```powershell
$src = "D:\VSCode\skill-github-push-readiness\skills\github-push-readiness"
# Cursor (user-wide)
New-Item -ItemType Directory -Force "$env:USERPROFILE\.cursor\skills\github-push-readiness" | Out-Null
Copy-Item -Recurse -Force "$src\*" "$env:USERPROFILE\.cursor\skills\github-push-readiness\"
# Claude Code (user-wide)
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills\github-push-readiness" | Out-Null
Copy-Item -Recurse -Force "$src\*" "$env:USERPROFILE\.claude\skills\github-push-readiness\"
# Codex (user-wide)
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex\skills\github-push-readiness" | Out-Null
Copy-Item -Recurse -Force "$src\*" "$env:USERPROFILE\.codex\skills\github-push-readiness\"
```

Other Agent Skills–compatible clients (VS Code Copilot, Gemini CLI, Goose, Roo Code, etc.) are listed on the [Agent Skills client showcase](https://agentskills.io/clients); use each product’s documented folder name and restart if required.

### Compatibility notes

- **Frontmatter**: Uses `name`, `description`, and `disable-model-invocation` (also recognized by Claude Code). If a client rejects unknown keys, remove optional keys and keep `name` + `description` only.
- **Invocation**: In Claude Code, the folder name maps to `/github-push-readiness`. In Cursor, reference the skill from the Skills picker or by name.
- **“OpenClaude”**: If you mean a **Claude Code–compatible** fork, use that product’s documented skills path (often same layout as `.claude/skills/<skill-name>/SKILL.md`).

## Contributing

Issues and PRs welcome. Keep `SKILL.md` tool-agnostic; put editor-specific tips in this README.

## License

MIT — see [LICENSE](LICENSE).
