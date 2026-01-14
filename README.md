# AI Config Search Guide

A curated collection of search queries to discover AI coding assistant configurations in public repositories.

## Why This Matters

GitHub's code search reveals real-world AI tool configurations that traditional searches miss. While official documentation shows you the format, queries like `path:.claude/skills` uncover how experienced developers actually configure their tools—patterns and techniques refined in production codebases.

## Contents

- [Basics](#basics)
  - [Adding Keywords for Domain-Specific Search](#adding-keywords-for-domain-specific-search)
  - [Searching by Filename Pattern](#searching-by-filename-pattern)
- [AI Coding Tools](#ai-coding-tools)
  - [Claude Code](#claude-code)
  - [Cursor](#cursor)
  - [Windsurf](#windsurf)
  - [GitHub Copilot](#github-copilot)
  - [Codex CLI](#codex-cli)
  - [Cline](#cline)
  - [Roo Code](#roo-code)
  - [OpenCode](#opencode)
  - [Aider](#aider)
  - [Continue.dev](#continuedev)
  - [Cody (Sourcegraph)](#cody-sourcegraph)
  - [Amazon Q Developer](#amazon-q-developer)
- [Advanced Tricks](#advanced-tricks)
- [Building Queries Step-by-Step](#building-queries-step-by-step)
- [Known Limitations](#known-limitations)
- [Contributing](#contributing)
- [Related Resources](#related-resources)

## Basics

GitHub's code search supports powerful operators. Here are the essentials:

| Operator | Example | Description |
|----------|---------|-------------|
| `path:` | `path:.claude/skills` | Search in specific path |
| `path:**/` | `path:**/CLAUDE.md` | Search by filename (any directory) |
| `repo:` | `repo:owner/name` | Search in specific repository |
| `org:` | `org:anthropic` | Search in organization's repos |
| `user:` | `user:username` | Search in user's repos |
| `language:` | `language:python` | Filter by programming language |
| `symbol:` | `symbol:functionName` | Search function/class definitions |
| `content:` | `content:keyword` | Search only in file contents (not paths) |
| `OR` | `path:.cursor OR path:.claude` | Match either condition |
| `NOT` | `config NOT path:test` | Exclude matches |

For the complete syntax, see [GitHub Code Search documentation](https://docs.github.com/en/search-github/github-code-search/understanding-github-code-search-syntax).

### Adding Keywords for Domain-Specific Search

The real power comes from combining path queries with keywords. Add any keyword before the path query to find configurations for specific domains, frameworks, or use cases:

| Use Case | Search Query | What You'll Find |
|----------|-------------|------------------|
| Reddit skills | [`reddit path:.claude/skills`](https://github.com/search?q=reddit+path%3A.claude%2Fskills&type=code) | Claude skills for Reddit automation |
| SwiftUI rules | [`swiftui path:**/.cursorrules`](https://github.com/search?q=swiftui+path%3A**%2F.cursorrules&type=code) | Cursor rules for SwiftUI projects |
| React configs | [`react path:**/.windsurfrules`](https://github.com/search?q=react+path%3A**%2F.windsurfrules&type=code) | Windsurf rules for React |
| Python conventions | [`python path:**/CONVENTIONS.md`](https://github.com/search?q=python+path%3A**%2FCONVENTIONS.md&type=code) | Aider conventions for Python |
| TypeScript agents | [`typescript path:**/AGENTS.md`](https://github.com/search?q=typescript+path%3A**%2FAGENTS.md&type=code) | Agent instructions for TypeScript |
| Next.js prompts | [`nextjs path:.github/prompts`](https://github.com/search?q=nextjs+path%3A.github%2Fprompts&type=code) | Copilot prompts for Next.js |

You can use any keyword: framework names (Next.js, Laravel, Rails), languages (Rust, Go, Python), platforms (AWS, Vercel), or domains (e-commerce, auth, API).

### Searching by Filename Pattern

The keyword search above looks inside file contents. To search only by **filename**, use wildcards in the path.

**Understanding the pattern:**

```
path:**/.claude/**/*reddit*
      │    │     │    │
      │    │     │    └── *reddit* = filename contains "reddit"
      │    │     └─────── ** = any subdirectory (recursive)
      │    └───────────── .claude = target directory
      └────────────────── ** = in any repository location
```

**Wildcards:**
- `**` matches any number of directories (recursive search)
- `*` matches any characters within a filename (but not `/`)
- `?` matches exactly one character (e.g., `path:*.a?c` matches `file.abc`, `file.aac`)

**Path anchoring:**
- `path:src/*.js` matches `src/app.js` and `app/src/main.js` (unanchored)
- `path:/src/*.js` matches only `src/app.js` at repository root (anchored with leading `/`)

**Literal matching:**
- `path:"file?"` searches for literal `file?` (glob patterns disabled in quotes)

**Nested directory patterns:**

The trailing pattern matters for nested searches:

| Pattern | Behavior |
|---------|----------|
| `path:.claude` | Substring match - any path containing ".claude" |
| `path:.claude/` | Paths within .claude directory |
| `path:.claude/skills` | All files in directory AND subdirectories |
| `path:.claude/skills/**/*` | Only files in subdirectories (direct children excluded) |

**Key insight:** `path:dir` is more comprehensive than `path:dir/**/*` because the latter only matches files in subdirectories, missing direct children of the directory.

**Recommended approach:** Use the simpler pattern for comprehensive results:

```
path:.claude/skills
```

Use `**/*` only when you specifically want subdirectory files:

```
path:.claude/skills/**/*
```

**Build your own query:**

```
path:**/[directory]/**/*[keyword]*
```

Replace `[directory]` with the config folder (`.claude`, `.cursor`, `.roo`, etc.) and `[keyword]` with what you're looking for.

**Examples:**

| Use Case | Search Query | What You'll Find |
|----------|-------------|------------------|
| Reddit-related files in .claude | [`path:**/.claude/**/*reddit*`](https://github.com/search?q=path%3A**%2F.claude%2F**%2F*reddit*&type=code) | Files with "reddit" in filename |
| React rules in .cursor | [`path:**/.cursor/**/*react*`](https://github.com/search?q=path%3A**%2F.cursor%2F**%2F*react*&type=code) | Files with "react" in filename |
| API-related skills | [`path:**/.claude/skills/**/*api*`](https://github.com/search?q=path%3A**%2F.claude%2Fskills%2F**%2F*api*&type=code) | Skill files with "api" in filename |

**Key difference from keyword search:**
- `reddit path:.claude/skills` - finds "reddit" in file **contents**
- `path:**/.claude/skills/**/*reddit*` - finds "reddit" in **filename** only

---

## AI Coding Tools

### Claude Code

#### Skills

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.claude/skills/` | `path:.claude/skills` | Skills directory | [Try](https://github.com/search?q=path%3A.claude%2Fskills&type=code) |
| `SKILL.md` | `path:**/SKILL.md` | Skill definition files | [Try](https://github.com/search?q=path%3A**%2FSKILL.md&type=code) |

#### Settings

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.claude/settings.json` | `path:.claude/settings.json` | Project settings | [Try](https://github.com/search?q=path%3A.claude%2Fsettings.json&type=code) |
| `.claude/settings.local.json` | `path:.claude/settings.local.json` | Local overrides | [Try](https://github.com/search?q=path%3A.claude%2Fsettings.local.json&type=code) |

#### Context and Memory

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `CLAUDE.md` | `path:**/CLAUDE.md` | Project context file | [Try](https://github.com/search?q=path%3A**%2FCLAUDE.md&type=code) |
| `CLAUDE.local.md` | `path:**/CLAUDE.local.md` | Personal context | [Try](https://github.com/search?q=path%3A**%2FCLAUDE.local.md&type=code) |
| `.claude/rules/` | `path:.claude/rules` | Modular rules | [Try](https://github.com/search?q=path%3A.claude%2Frules&type=code) |

#### Hooks and Automation

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.claude/hooks/` | `path:.claude/hooks` | Lifecycle hooks | [Try](https://github.com/search?q=path%3A.claude%2Fhooks&type=code) |

#### MCP (Model Context Protocol)

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.mcp.json` | `path:**/.mcp.json` | MCP server config | [Try](https://github.com/search?q=path%3A**%2F.mcp.json&type=code) |

#### Agents

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.claude/agents/` | `path:.claude/agents` | Custom subagents | [Try](https://github.com/search?q=path%3A.claude%2Fagents&type=code) |

#### Legacy

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.claude/commands/` | `path:.claude/commands` | Legacy commands | [Try](https://github.com/search?q=path%3A.claude%2Fcommands&type=code) |
| `.claude.json` | `path:**/.claude.json` | Legacy user config | [Try](https://github.com/search?q=path%3A**%2F.claude.json&type=code) |

---

### Cursor

#### Rules

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.cursorrules` | `path:**/.cursorrules` | Legacy rules file | [Try](https://github.com/search?q=path%3A**%2F.cursorrules&type=code) |
| `.cursor/rules/` | `path:.cursor/rules` | Modern rules directory | [Try](https://github.com/search?q=path%3A.cursor%2Frules&type=code) |

#### Hooks and MCP

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.cursor/hooks.json` | `path:.cursor/hooks.json` | Hook automation | [Try](https://github.com/search?q=path%3A.cursor%2Fhooks.json&type=code) |
| `.cursor/mcp.json` | `path:.cursor/mcp.json` | MCP configuration | [Try](https://github.com/search?q=path%3A.cursor%2Fmcp.json&type=code) |

#### Ignore Files

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.cursorignore` | `path:**/.cursorignore` | Block AI access | [Try](https://github.com/search?q=path%3A**%2F.cursorignore&type=code) |
| `.cursorindexingignore` | `path:**/.cursorindexingignore` | Exclude from indexing | [Try](https://github.com/search?q=path%3A**%2F.cursorindexingignore&type=code) |

#### Agent Instructions

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `AGENTS.md` | `path:**/AGENTS.md` | Agent workflow instructions | [Try](https://github.com/search?q=path%3A**%2FAGENTS.md&type=code) |

---

### Windsurf

#### Rules

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.windsurfrules` | `path:**/.windsurfrules` | Project rules | [Try](https://github.com/search?q=path%3A**%2F.windsurfrules&type=code) |
| `global_rules.md` | `path:**/global_rules.md` | Global Cascade rules | [Try](https://github.com/search?q=path%3A**%2Fglobal_rules.md&type=code) |
| `.windsurf/rules/` | `path:.windsurf/rules` | Rules directory | [Try](https://github.com/search?q=path%3A.windsurf%2Frules&type=code) |

#### Workflows and Memories

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.windsurf/workflows/` | `path:.windsurf/workflows` | Automation workflows | [Try](https://github.com/search?q=path%3A.windsurf%2Fworkflows&type=code) |
| `.windsurf/memories/` | `path:.windsurf/memories` | Persistent context | [Try](https://github.com/search?q=path%3A.windsurf%2Fmemories&type=code) |

#### MCP

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `mcp_config.json` | `path:**/mcp_config.json` | MCP configuration | [Try](https://github.com/search?q=path%3A**%2Fmcp_config.json&type=code) |

#### Ignore

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.codeiumignore` | `path:**/.codeiumignore` | File exclusions | [Try](https://github.com/search?q=path%3A**%2F.codeiumignore&type=code) |

---

### GitHub Copilot

#### Repository Instructions

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `copilot-instructions.md` | `path:**/copilot-instructions.md` | Repository instructions | [Try](https://github.com/search?q=path%3A**%2Fcopilot-instructions.md&type=code) |
| `.github/instructions/` | `path:.github/instructions` | File-type instructions | [Try](https://github.com/search?q=path%3A.github%2Finstructions&type=code) |

#### Prompts and Agents

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.github/prompts/` | `path:.github/prompts` | Prompt files | [Try](https://github.com/search?q=path%3A.github%2Fprompts&type=code) |
| `AGENTS.md` | `path:**/AGENTS.md` | Agent configuration | [Try](https://github.com/search?q=path%3A**%2FAGENTS.md&type=code) |

---

### Codex CLI

#### Configuration

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `config.toml` (codex) | `path:.codex/config.toml` | Main configuration | [Try](https://github.com/search?q=path%3A.codex%2Fconfig.toml&type=code) |
| `.codexrc` | `path:**/.codexrc` | Quick overrides | [Try](https://github.com/search?q=path%3A**%2F.codexrc&type=code) |

#### Agent Instructions

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `AGENTS.md` | `path:**/AGENTS.md` | Agent guidance | [Try](https://github.com/search?q=path%3A**%2FAGENTS.md&type=code) |
| `AGENTS.override.md` | `path:**/AGENTS.override.md` | Override instructions | [Try](https://github.com/search?q=path%3A**%2FAGENTS.override.md&type=code) |

#### Skills

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.codex/skills/` | `path:.codex/skills` | Skills directory | [Try](https://github.com/search?q=path%3A.codex%2Fskills&type=code) |

---

### Cline

#### Rules

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.clinerules` | `path:**/.clinerules` | Project rules file | [Try](https://github.com/search?q=path%3A**%2F.clinerules&type=code) |
| `.clinerules/` | `path:**/.clinerules/*.md` | Rules directory | [Try](https://github.com/search?q=path%3A**%2F.clinerules%2F*.md&type=code) |

#### Memory Bank

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `memory-bank/` | `path:**/memory-bank/*.md` | Memory bank files | [Try](https://github.com/search?q=path%3A**%2Fmemory-bank%2F*.md&type=code) |
| `cline_docs/` | `path:**/cline_docs/*.md` | Legacy memory folder | [Try](https://github.com/search?q=path%3A**%2Fcline_docs%2F*.md&type=code) |

#### Ignore and MCP

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.clineignore` | `path:**/.clineignore` | File exclusions | [Try](https://github.com/search?q=path%3A**%2F.clineignore&type=code) |
| `cline_mcp_settings.json` | `path:**/cline_mcp_settings.json` | MCP configuration | [Try](https://github.com/search?q=path%3A**%2Fcline_mcp_settings.json&type=code) |

---

### Roo Code

#### Modes and Rules

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.roomodes` | `path:**/.roomodes` | Custom mode definitions | [Try](https://github.com/search?q=path%3A**%2F.roomodes&type=code) |
| `.roo/rules/` | `path:**/.roo/rules` | Rules directory | [Try](https://github.com/search?q=path%3A**%2F.roo%2Frules&type=code) |
| `.roorules` | `path:**/.roorules` | Legacy rules file | [Try](https://github.com/search?q=path%3A**%2F.roorules&type=code) |

#### Memory and MCP

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `memory-bank/` | `path:**/memory-bank/productContext.md` | Memory bank context | [Try](https://github.com/search?q=path%3A**%2Fmemory-bank%2FproductContext.md&type=code) |
| `.roo/mcp.json` | `path:**/.roo/mcp.json` | MCP configuration | [Try](https://github.com/search?q=path%3A**%2F.roo%2Fmcp.json&type=code) |

#### Ignore

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.rooignore` | `path:**/.rooignore` | File exclusions | [Try](https://github.com/search?q=path%3A**%2F.rooignore&type=code) |

---

### OpenCode

#### Configuration

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `opencode.json` | `path:**/opencode.json` | Main configuration | [Try](https://github.com/search?q=path%3A**%2Fopencode.json&type=code) |
| `opencode.jsonc` | `path:**/opencode.jsonc` | JSON with comments config | [Try](https://github.com/search?q=path%3A**%2Fopencode.jsonc&type=code) |
| `.opencode.json` | `path:**/.opencode.json` | Alternative config | [Try](https://github.com/search?q=path%3A**%2F.opencode.json&type=code) |

#### Agents and Modes

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.opencode/agent/` | `path:**/.opencode/agent` | Agent definitions | [Try](https://github.com/search?q=path%3A**%2F.opencode%2Fagent&type=code) |
| `.opencode/mode/` | `path:**/.opencode/mode` | Mode configurations | [Try](https://github.com/search?q=path%3A**%2F.opencode%2Fmode&type=code) |
| `.opencode/command/` | `path:**/.opencode/command` | Custom commands | [Try](https://github.com/search?q=path%3A**%2F.opencode%2Fcommand&type=code) |

#### Plugins

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.opencode/plugin/` | `path:**/.opencode/plugin` | Plugin files | [Try](https://github.com/search?q=path%3A**%2F.opencode%2Fplugin&type=code) |

---

### Aider

#### Configuration

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.aider.conf.yml` | `path:**/.aider.conf.yml` | Main configuration | [Try](https://github.com/search?q=path%3A**%2F.aider.conf.yml&type=code) |
| `.aiderignore` | `path:**/.aiderignore` | Exclusion patterns | [Try](https://github.com/search?q=path%3A**%2F.aiderignore&type=code) |

#### Conventions

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `CONVENTIONS.md` | `path:**/CONVENTIONS.md` | Coding standards | [Try](https://github.com/search?q=path%3A**%2FCONVENTIONS.md&type=code) |

#### History

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.aider.chat.history.md` | `path:**/.aider.chat.history.md` | Chat history | [Try](https://github.com/search?q=path%3A**%2F.aider.chat.history.md&type=code) |
| `.aider.input.history` | `path:**/.aider.input.history` | Input history | [Try](https://github.com/search?q=path%3A**%2F.aider.input.history&type=code) |

#### Advanced

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.aider.model.settings.yml` | `path:**/.aider.model.settings.yml` | Model settings | [Try](https://github.com/search?q=path%3A**%2F.aider.model.settings.yml&type=code) |

---

### Continue.dev

#### Configuration

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `config.yaml` (continue) | `path:.continue/config.yaml` | Primary config | [Try](https://github.com/search?q=path%3A.continue%2Fconfig.yaml&type=code) |
| `config.json` (continue) | `path:.continue/config.json` | Legacy config | [Try](https://github.com/search?q=path%3A.continue%2Fconfig.json&type=code) |
| `.continuerc.json` | `path:**/.continuerc.json` | Workspace override | [Try](https://github.com/search?q=path%3A**%2F.continuerc.json&type=code) |

#### Rules

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.continuerules` | `path:**/.continuerules` | Rules file | [Try](https://github.com/search?q=path%3A**%2F.continuerules&type=code) |
| `.continue/rules/` | `path:.continue/rules` | Rules directory | [Try](https://github.com/search?q=path%3A.continue%2Frules&type=code) |

#### Agents and MCP

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.continue/agents/` | `path:.continue/agents` | Agent configs | [Try](https://github.com/search?q=path%3A.continue%2Fagents&type=code) |
| `.continue/mcpServers/` | `path:.continue/mcpServers` | MCP configs | [Try](https://github.com/search?q=path%3A.continue%2FmcpServers&type=code) |

---

### Cody (Sourcegraph)

#### Configuration

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `cody.json` (vscode) | `path:.vscode/cody.json` | Custom commands | [Try](https://github.com/search?q=path%3A.vscode%2Fcody.json&type=code) |
| `.codyignore` | `path:**/.codyignore` | File exclusions | [Try](https://github.com/search?q=path%3A**%2F.codyignore&type=code) |
| `.cody/ignore` | `path:.cody/ignore` | Alt ignore location | [Try](https://github.com/search?q=path%3A.cody%2Fignore&type=code) |

---

### Amazon Q Developer

#### Project Rules

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.amazonq/rules/` | `path:.amazonq/rules` | Project rules | [Try](https://github.com/search?q=path%3A.amazonq%2Frules&type=code) |

#### Agents and MCP

| Config | Query | Description |   |
|--------|-------|-------------|---|
| `.amazonq/agents/` | `path:.amazonq/agents` | Agent configs | [Try](https://github.com/search?q=path%3A.amazonq%2Fagents&type=code) |
| `.amazonq/mcp.json` | `path:.amazonq/mcp.json` | MCP config | [Try](https://github.com/search?q=path%3A.amazonq%2Fmcp.json&type=code) |

---

## Advanced Tricks

### Multi-Tool Search

Find any AI tool configuration in one query:

```
path:.claude OR path:.cursor OR path:.windsurf OR path:.continue
```

### Excluding Noise

Find real configurations, not test fixtures:

```
path:**/.cursorrules NOT path:test NOT path:fixture NOT path:example
```

### Regex Patterns

Search for version patterns in config files:

```
/version.*[0-9]+\.[0-9]+/ path:**/*.json
```

**Claude Code regex examples:**

| Pattern | Query | Finds |
|---------|-------|-------|
| Markdown headings | [`/^## [A-Z].*/ path:**/CLAUDE.md`](https://github.com/search?q=%2F%5E%23%23+%5BA-Z%5D.*%2F+path%3A**%2FCLAUDE.md&type=code) | Section headers in CLAUDE.md |
| Directives | [`/IMPORTANT:\|NOTE:\|NEVER:/ path:**/CLAUDE.md`](https://github.com/search?q=%2FIMPORTANT%3A%7CNOTE%3A%7CNEVER%3A%2F+path%3A**%2FCLAUDE.md&type=code) | Important instructions |
| YAML frontmatter | [`/^---\n.*name:/ path:.claude/skills`](https://github.com/search?q=%2F%5E---%5Cn.*name%3A%2F+path%3A.claude%2Fskills&type=code) | Skill definitions with metadata |
| Tool permissions | [`/"allowedTools"\s*:\s*\[/ path:.claude/settings`](https://github.com/search?q=%2F%22allowedTools%22%5Cs*%3A%5Cs*%5C%5B%2F+path%3A.claude%2Fsettings&type=code) | Settings with tool restrictions |
| MCP servers | [`/"mcpServers"\s*:\s*\{/ path:**/.mcp.json`](https://github.com/search?q=%2F%22mcpServers%22%5Cs*%3A%5Cs*%5C%7B%2F+path%3A**%2F.mcp.json&type=code) | MCP server configurations |
| Hook events | [`/"hooks"\s*:\s*\{\s*"[a-zA-Z]+"/ path:.claude`](https://github.com/search?q=%2F%22hooks%22%5Cs*%3A%5Cs*%5C%7B%5Cs*%22%5Ba-zA-Z%5D%2B%22%2F+path%3A.claude&type=code) | Hook event definitions |
| Agent definitions | [`/^---\n.*description:/ path:.claude/agents`](https://github.com/search?q=%2F%5E---%5Cn.*description%3A%2F+path%3A.claude%2Fagents&type=code) | Custom agents with YAML frontmatter |

### Combining Keywords with Boolean

Find TypeScript OR JavaScript related rules:

```
(typescript OR javascript) path:**/.cursorrules
```

### Organization-Wide Search

Find all Claude configs in a specific organization:

```
path:.claude org:your-org-name
```

### Filtering by Repository Properties

Exclude forks and archived repositories for cleaner results:

```
path:**/.cursorrules NOT is:fork NOT is:archived
```

Available `is:` filters: `archived`, `fork`, `vendored`, `generated`

### Case-Sensitive Search

Use regex with case-sensitivity flag disabled:

```
/(?-i)MyComponent/ path:.cursor/rules
```

### Exact Filename Match

Match only the exact filename (not partial path matches):

```
path:/(^|\/)CLAUDE\.md$/
```

### Symbol Search

Find function or class definitions:

```
symbol:handleRequest language:typescript path:.claude
```

### Language-Filtered Config Search

Find Python-specific configurations:

```
language:markdown python path:.cursor/rules
```

---

## Building Queries Step-by-Step

Start broad, then narrow down. Here's how to explore Claude Code configurations:

```
Step 1: Start with the tool's directory
        path:.claude
        > See all Claude Code configs (settings, rules, skills, hooks...)

Step 2: Narrow to a specific config type
        path:.claude/skills
        > Only skill files

Step 3: Add a keyword to find specific skills
        api path:.claude/skills
        > Skills related to API development

Step 4: Use OR for related terms
        (api OR rest OR graphql) path:.claude/skills
        > Catches different API styles
```

**Another example:** Exploring Cursor configurations

```
Step 1: path:.cursor                                   > All Cursor configs
Step 2: path:.cursor/rules                             > Only rules
Step 3: typescript path:.cursor/rules                  > TypeScript-related rules
Step 4: (typescript OR javascript) path:.cursor/rules  > Both TS and JS
```

**Filter cheatsheet:**

| Filter | Purpose | Example |
|--------|---------|---------|
| `path:` | Search in path | `path:.claude/skills` |
| `path:**/` | Any directory | `path:**/CLAUDE.md` |
| `language:` | Filter by language | `language:python` |
| `symbol:` | Function/class definitions | `symbol:MyClass` |
| `content:` | Only file contents | `content:api` |
| `is:` | Repository properties | `NOT is:fork` |
| `OR` | Match alternatives | `(react OR vue)` |
| `NOT` | Exclude | `NOT path:node_modules` |

**Note:** Filters like `stars:`, `pushed:`, and `fork:` only work in repository search, not code search.

---

## Known Limitations

### Search Scope
- Only the default branch is indexed (not other branches or tags)
- Private repositories are not visible unless you have access
- Deleted or old configs in commit history cannot be searched
- You must be logged in to search code in public repositories

### Query Limits
- Query length: max 1000 characters
- Boolean operators: max 5 `AND`, `OR`, or `NOT` per query
- Results: 20 per page, max 5 pages (100 results viewable even if thousands match)
- Code search doesn't support `stars:`, `pushed:`, `fork:` filters (these only work in repository search)

**Tip:** If your search returns too many results, narrow it down with additional keywords, path filters, or regex patterns to find what you need within the 100-result limit.

### File Indexing Limits
- Files over 350 KB are excluded
- Empty files are not indexed
- Binary files (PDF, images, etc.) are excluded
- Only UTF-8 encoded files are indexed
- Lines over 1024 characters are truncated

### Other Notes
- The `filename:` qualifier is deprecated; use `path:**/filename` instead
- Identical files across repos may be collapsed (click "Show identical files" to expand)
- Very large repositories may not be fully indexed
- Regex look-around assertions (`(?=...)`, `(?!...)`) are not supported

---

## Contributing

Contributions are welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## Related Resources

- [GitHub Code Search Syntax](https://docs.github.com/en/search-github/github-code-search/understanding-github-code-search-syntax) - Official documentation
- [Claude Code Configuration](https://docs.anthropic.com/en/docs/claude-code/settings) - Official `.claude` folder documentation
- [AGENTS.md Standard](https://agents.md/) - Open standard for AI agent instructions
- [Sourcegraph](https://sourcegraph.com/search) - Advanced code search with fewer limits
- [grep.app](https://grep.app) - Fast regex search across GitHub

---

## License

[MIT](LICENSE)
