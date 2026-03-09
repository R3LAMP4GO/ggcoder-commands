# GG Coder Commands

This repo contains reusable prompt command templates for GG Coder.

## Structure
- `commands/` — Each `.md` file is a standalone command prompt
- Commands use GG Coder's native tools: bash, find, grep, read, write, edit, tasks, web_search, mcp__grep__searchGitHub

## Conventions
- Commands are self-contained — each file has everything needed to execute
- `[CWD]` placeholders get replaced with the actual working directory at runtime
- `[STACK]` placeholders get replaced with detected project stack
- Tasks created via the `tasks` tool should have concise, actionable prompts
- No fluff, no hedging — decisive instructions only
