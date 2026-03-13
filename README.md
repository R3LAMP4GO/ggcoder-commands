# GG Coder Commands

Reusable prompt commands for GG Coder by Ken Kai — ported and adapted from [minimal-claude](https://github.com/KenKaiii/minimal-claude) plugin patterns for GG Coder's native toolset.

## Commands

| Command | Description |
|---------|-------------|
| [`commit`](commands/commit.md) | Lint → typecheck → AI commit message → push |
| [`candy`](commands/candy.md) | 5-agent parallel codebase health scan (dead code, bugs, security) |
| [`carrot`](commands/carrot.md) | 8-agent parallel verification against real-world patterns & docs |
| [`setup-tests`](commands/setup-tests.md) | Auto-detect stack → scaffold comprehensive tests |
| [`research`](commands/research.md) | 6-agent research → output actionable RESEARCH.md |
| [`setup-quality`](commands/setup-quality.md) | Auto-detect project → configure linting/typechecking |
| [`setup-speedrun`](commands/setup-speedrun.md) | One-time project setup for automated feature pipeline |
| [`speedrun`](commands/speedrun.md) | Full auto feature pipeline — plan → build → validate → ship |
| [`speedrunp`](commands/speedrunp.md) | Parallel speedrun — multiple features via git worktrees |

## Speedrun Pipeline

The speedrun system automates the full feature lifecycle. Give it a GitHub issue, walk away, come back to merged code.

### How it works

```
GitHub Issue → Plan (research + decompose) → Build (chunk by chunk) → Validate (fix errors) → Ship (merge + close)
```

### Quick start

```bash
# 1. One-time setup (detects stack, creates scripts)
/setup-speedrun

# 2. Run a single feature
speedrun 42

# 3. Run all open issues labeled 'auto'
speedrun

# 4. Parallel mode (3 concurrent via worktrees)
speedrunp 42,43,44
```

### Architecture

| Script | Role |
|--------|------|
| `run-feature.sh` | Orchestrator — 4 phases (plan → build → validate → ship) |
| `run-plan.sh` | Build executor — runs chunks from a GitHub issue plan |
| `run-speedrun-parallel.sh` | Parallel coordinator — worktrees + sliding window |

### Key features

- **Auto-retry** on rate limits, API errors, connection drops (5 retries, exponential backoff)
- **Branch drift recovery** — if ggcoder switches branches, scripts auto-recover
- **Quality gates** after every chunk — lint + typecheck must pass
- **Auto-merge conflict resolution** for lockfiles, CLAUDE.md, config files
- **Timing** on every phase — know where time goes
- **Plan issue cleanup** — ship closes both feature and plan issues
- **`--all` summary stats** — succeeded/failed/total at the end

### Templates

The `templates/` directory contains the script templates used by `setup-speedrun`. They use `__PROJECT_DIR__` and `__CHECK_CMD__` placeholders that get replaced per-project.

## Usage

Copy-paste a command's content as your prompt to GG Coder, or reference the file directly.

These commands leverage GG Coder's built-in tools:
- `bash` — run shell commands (lint, test, git)
- `find` / `grep` / `read` — explore codebase
- `tasks` — spawn parallel sub-tasks
- `web_search` / `web_fetch` — research docs
- `mcp__grep__searchGitHub` — verify against real-world code

## Differences from Claude Code Plugin

| Feature | Claude Code | GG Coder |
|---------|-------------|----------|
| Parallel agents | Task tool (subagent) | `tasks` tool (task pane) |
| Code search | Grep MCP (if configured) | `mcp__grep__searchGitHub` (built-in) |
| Web research | WebSearch (if configured) | `web_search` + `web_fetch` (built-in) |
| Slash commands | `/command` syntax | Copy-paste or file reference |
| Hooks | hooks.json events | Not applicable |

## License

MIT
