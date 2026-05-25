> **Note (2026-05-23) — Consolidated into LibreSessionFlow**
>
> This standalone has been folded into the umbrella repo
> [LibreSessionFlow-Claude-Code](https://github.com/HermeticOrmus/LibreSessionFlow-Claude-Code),
> alongside 9 sibling session-lifecycle plugins. The current version of this
> functionality lives at `plugins/explore/` in the umbrella.
>
> This repo remains live as a landing page. New work happens in the umbrella.

---

<p align="center">
  <img src="https://ormus.solutions/mascot/golden_swan.gif" alt="ormus-explore" width="128" style="image-rendering: pixelated;" />
</p>

<h1 align="center">ormus-explore</h1>

<p align="center">
  <em>Token-optimized AST-based code search for Claude Code. 4-8x cheaper than reading full files.</em>
</p>

<p align="center">
  <a href="https://github.com/HermeticOrmus/ormus-explore/stargazers"><img src="https://img.shields.io/github/stars/HermeticOrmus/ormus-explore?style=flat-square&color=aa8142" alt="Stars" /></a>
  <a href="https://github.com/HermeticOrmus/ormus-explore/blob/main/LICENSE"><img src="https://img.shields.io/github/license/HermeticOrmus/ormus-explore?style=flat-square&color=aa8142" alt="License" /></a>
  <a href="https://github.com/HermeticOrmus/ormus-explore/commits"><img src="https://img.shields.io/github/last-commit/HermeticOrmus/ormus-explore?style=flat-square&color=aa8142" alt="Last Commit" /></a>
  <img src="https://img.shields.io/badge/Claude_Code-aa8142?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code" />
</p>

---

> **Token-optimized structural code search for Claude Code. Stop reading whole files — get the map first.**

A Claude Code skill that swaps the default `Read → Grep → Glob` exploration cycle for AST-based structural search powered by tree-sitter MCP. Save 4-8x tokens on file understanding and 11-18x on codebase-wide exploration.

## Why

The default Claude Code workflow for understanding code is read-and-narrow: glob to find files, grep to find matches, read full files to see context. For a 1000-line file you might need to know about one function, that's 12k+ tokens spent to surface 50 lines of relevance. Across a session, those costs compound.

`smart-explore` flips it: index the codebase as an AST first, then fetch only the symbols you need. A function takes 400-2000 tokens to unfold instead of 12000 to Read.

## Install

This skill depends on the **tree-sitter MCP server** for AST parsing. Install both:

### 1. Install the tree-sitter MCP server

```bash
# One-time setup — installs Claude Code's tree-sitter MCP
claude mcp add tree-sitter
```

(Or the equivalent invocation for your tree-sitter MCP package — check the [tree-sitter MCP docs](https://github.com/modelcontextprotocol) for the canonical install command at the time you read this.)

The MCP must expose three tools: `smart_search`, `smart_outline`, `smart_unfold`. Without those tools available, this skill has nothing to call.

### 2. Install the skill

```bash
git clone https://github.com/HermeticOrmus/ormus-explore ~/.claude/skills/smart-explore
```

Or as a Claude Code plugin:

```bash
claude plugin marketplace add HermeticOrmus/ormus-explore
```

Restart Claude Code so the skill registry picks it up. Verify with `claude mcp list` — `tree-sitter` should be present.

## Usage

The skill activates automatically when relevant. To invoke explicitly:

```
/smart-explore
```

Then your next tool call should be one of:

```
smart_search(query="shutdown", path="./src")        — find symbols across a directory
smart_outline(file_path="path/to/file.ts")          — structural skeleton of one file
smart_unfold(file_path="path/to/file.ts", symbol_name="myFunction")  — full source of one symbol
```

## Workflow

```
1. smart_search(query="auth", path="./src")
   → ranked symbols + folded file views, ~2-6k tokens
2. Pick the relevant ones from the results
3. smart_unfold on the symbols you actually need
   → ~400-2k tokens per symbol, AST-precise boundaries
```

That's it. No globbing, no full-file reads, no scanning hundreds of lines to find one function.

## Token economics

| Approach | Tokens | Use case |
|---|---|---|
| `smart_outline` | 1,000-2,000 | "What's in this file?" |
| `smart_unfold` | 400-2,100 | "Show me this function" |
| `smart_search` | 2,000-6,000 | "Find all X across the codebase" |
| `search + unfold` | 3,000-8,000 | End-to-end: find and read (primary workflow) |
| `Read` (full file) | 12,000+ | Only when you truly need everything |
| Explore agent | 39,000-59,000 | Cross-file narrative synthesis |

**11-18x savings** on codebase exploration vs the Explore agent. **4-8x savings** on file understanding vs Read. The narrower your query, the wider the gap.

## When to fall back to standard tools

`smart-explore` is a scalpel. Use the regular tools for:

- **Grep** — exact string/regex search ("all TODO comments", "where is `EXACT_NAME` written")
- **Read** — files under ~100 lines, JSON/markdown/configs
- **Glob** — file path patterns ("all test files")
- **Explore agent** — cross-file synthesis with narrative ("how does the entire auth flow work end-to-end?")

## Pairs with

- **[ormus-handoff](https://github.com/HermeticOrmus/ormus-handoff)** — capture session state before context limits.
- **[ormus-pickup](https://github.com/HermeticOrmus/ormus-pickup)** — restore context from a HANDOFF file.
- **[ormus-absorb](https://github.com/HermeticOrmus/ormus-absorb)** — distill conversation knowledge into persistent memory.
- **[ormus-vibe-proof](https://github.com/HermeticOrmus/ormus-vibe-proof)** — security hardening for vibe-coded full-stack apps.
- **[ormus-meta-prompting](https://github.com/HermeticOrmus/ormus-meta-prompting)** — categorical foundations for AI prompt engineering.

Together they form the **ormus session lifecycle** — composable Claude Code skills for doing serious work across days, machines, and context resets.

## License

MIT. See [LICENSE](LICENSE).

## Origin

Distilled from real Claude Code sessions on large polyrepo codebases. Built around the tree-sitter MCP because AST-precise symbol boundaries beat regex-precise text matches every time.

---

## Part of the Libre Open-Source Stack for Claude Code

This repository is part of a growing family of open-source toolkits for Claude Code.

### Libre suite — comprehensive plugin bundles

- [LibreUIUX-Claude-Code](https://github.com/HermeticOrmus/LibreUIUX-Claude-Code) — UI/UX development (152 agents, 70 plugins, 76 commands, 74 skills)
- [LibreArch-Claude-Code](https://github.com/HermeticOrmus/LibreArch-Claude-Code) — Software architecture and system design
- [LibreCopy-Claude-Code](https://github.com/HermeticOrmus/LibreCopy-Claude-Code) — Technical writing and documentation engineering
- [LibreDevOps-Claude-Code](https://github.com/HermeticOrmus/LibreDevOps-Claude-Code) — DevOps engineering and infrastructure automation
- [LibreEmbed-Claude-Code](https://github.com/HermeticOrmus/LibreEmbed-Claude-Code) — Embedded systems, firmware, and IoT development
- [LibreFinTech-Claude-Code](https://github.com/HermeticOrmus/LibreFinTech-Claude-Code) — Financial technology development
- [LibreGEO-Claude-Code](https://github.com/HermeticOrmus/LibreGEO-Claude-Code) — AI-search optimization (ChatGPT, Perplexity, Gemini, Google AI Overviews)
- [LibreGameDev-Claude-Code](https://github.com/HermeticOrmus/LibreGameDev-Claude-Code) — Game development across Godot, Unity, Unreal
- [LibreMLOps-Claude-Code](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code) — ML engineering and AI operations
- [LibreMobileDev-Claude-Code](https://github.com/HermeticOrmus/LibreMobileDev-Claude-Code) — Mobile app development (Flutter, React Native, native iOS, native Android)
- [LibreSecOps-Claude-Code](https://github.com/HermeticOrmus/LibreSecOps-Claude-Code) — Security operations

### Skills mini-repos — single CLAUDE.md drop-ins

- [vibe-engineer-skills](https://github.com/HermeticOrmus/vibe-engineer-skills) — Direct AI codegen well (hypothesis → scope → validate → reject working-but-wrong)
- [markdown-discipline-skills](https://github.com/HermeticOrmus/markdown-discipline-skills) — Strip AI-slop from markdown (no em dashes, no marketing fluff)
- [shell-safety-skills](https://github.com/HermeticOrmus/shell-safety-skills) — `set -euo pipefail` discipline + 15 failure-mode examples
- [commit-standard-skills](https://github.com/HermeticOrmus/commit-standard-skills) — Ormus Commit Standard v1.0 + commit-msg hook + commitlint
- [unwoke-skills](https://github.com/HermeticOrmus/unwoke-skills) — Strip AI theater (ten sins to eliminate, symmetric engagement)
- [python-conventions-skills](https://github.com/HermeticOrmus/python-conventions-skills) — Modern Python 3.11+ (types, pathlib, async, ruff, mypy, uv)
- [typescript-conventions-skills](https://github.com/HermeticOrmus/typescript-conventions-skills) — TypeScript strict mode, discriminated unions, Result types
- [hermetic-laws-skills](https://github.com/HermeticOrmus/hermetic-laws-skills) — Seven Hermetic Principles applied to engineering
- [riper-workflow-skills](https://github.com/HermeticOrmus/riper-workflow-skills) — Research / Innovate / Plan / Execute / Review systematic dev
- [six-day-cycle-skills](https://github.com/HermeticOrmus/six-day-cycle-skills) — Sustainable shipping cadence with mandatory rest
- [token-optimization-skills](https://github.com/HermeticOrmus/token-optimization-skills) — Claude Code token + context optimization
- [osint-skills](https://github.com/HermeticOrmus/osint-skills) — OSINT research methodology (multi-wave investigative spiral)
- [calcinate-skills](https://github.com/HermeticOrmus/calcinate-skills) — Stage 1 of the Magnum Opus (burn project bloat)
- [claude-md-overhaul-skills](https://github.com/HermeticOrmus/claude-md-overhaul-skills) — Audit CLAUDE.md and MEMORY.md against caps
- [session-handoff-skills](https://github.com/HermeticOrmus/session-handoff-skills) — Session handoff + pickup discipline
- [naming-skills](https://github.com/HermeticOrmus/naming-skills) — Product naming methodology (mine the brand's vocabulary)
- [magnum-opus-skills](https://github.com/HermeticOrmus/magnum-opus-skills) — Seven-stage alchemy applied to project transformation

### Template source

- [andrej-karpathy-skills](https://github.com/HermeticOrmus/andrej-karpathy-skills) — the canonical single-file CLAUDE.md pattern (fork of jiayuan_jy's original)

Star the family, not just one — that's how the suite stays coherent.
