# Fabric DP-700 Study Assistant

This repo is an AI-powered study assistant for the [DP-700 certification](https://learn.microsoft.com/en-us/credentials/certifications/fabric-data-engineer-associate/) (Fabric Data Engineer Associate).

## The idea in short

Instead of re-explaining the same context every time (to another person or to an AI), it gets written down **once, in one clearly named place**. On top of that, answers about the constantly-changing Fabric (and DP-700) landscape are kept current by using the Microsoft Learn Fabric MCP server.

- **`AGENTS.md`** — the canonical instructions file. Guardrails (what not to do / what to check first), a pointer to the `references/` folder, and a description of the available tools.
- **`CLAUDE.md`** — carries no content of its own, just points to `AGENTS.md`. This way the instructions live in one place, not in two different wordings.
- **`.mcp.json`** — MCP server connections (e.g. for Microsoft Learn Fabric documentation).
- **`references/`** — durable knowledge split into small, single-topic files (architecture, naming conventions, known gotchas, glossary). One file per topic is easier to keep current and easier to read than one giant multi-hundred-line document.

Three of the reference files (`exam-domains.md`, `topic-by-product-area.md`, `known-gotchas.md`) carry `markmap:` YAML front matter.
That's optional — the files read fine as plain markdown — but if you open them with a markmap-capable viewer (e.g. the [Markmap VS Code extension](https://marketplace.visualstudio.com/items?itemName=gera2ld.markmap-vscode)), the same heading/bullet outline also renders as a mind map.

## What's in this folder

```
DP700_Assistant/
├── README.md                     ← this file
├── AGENTS.md                      ← canonical instructions: guardrails + reference index + tools
├── CLAUDE.md                      ← one-line pointer to AGENTS.md
├── .mcp.json                      ← Microsoft Learn MCP connection
├── .gitignore                     ← excludes local/non-shared files (see below)
├── .claude/
│   └── settings.json               ← shared Claude Code settings (settings.local.json is not shared)
└── references/
    ├── INDEX.md                   ← plain list: which files exist and what each one covers
    ├── exam-domains.md            ← official DP-700 exam skill areas (source of truth)
    ├── topic-by-product-area.md   ← DP-700 topics regrouped by Fabric product area
    ├── study-progress.md          ← study progress by skill area
    ├── known-gotchas.md           ← known gotchas and the reasoning behind them
    └── glossary.md                ← core domain terms
```

The listing above only covers files added to version control (`git ls-files`). Personal/work-in-progress notes (e.g. `temp.md`) and typical junk (`__pycache__/`, `.venv/`, `node_modules/`, `.env*`, etc.) are excluded via `.gitignore`.

## Sources
Some useful links for AI Assistant
  - "*My AI Setup for Microsoft Fabric: Never Explain Yourself Twice*" [(video)](https://www.youtube.com/watch?v=5-sXBbBJbAk)
  - A post about Microsoft’s Skills for Fabric [link](https://www.linkedin.com/pulse/claude-code-microsoft-fabric-practical-data-harsha-guggilla-mth8e/ "Claude Code and Microsoft Fabric - Practical Data Engineer Workflow")

## AI-tool agnosticism and possible next steps

While an effort was made to keep this repo's structure reasonably AI-tool-agnostic, it is currently built/optimized for Claude Code. If the project is also meant to be used with other AI coding agents, configuration files will need to be created and/or the existing ones adapted, since each tool reads its own format:

- **MCP connection** (`.mcp.json`): the MCP protocol itself is tool-independent, but every editor/agent registers servers in its own file, using the same `mcpServers`/`servers` URL (`https://learn.microsoft.com/api/mcp`). In Cursor this is `.cursor/mcp.json`; in VS Code / GitHub Copilot it's `.vscode/mcp.json` (note: the key is `servers`, not `mcpServers`); Codex CLI has its own configuration.
- **Permission rules/hooks** (`.claude/settings.json`): entirely Claude-Code-specific, do not carry over as-is to any other tool.
- **Instructions file** (`AGENTS.md`): the name follows the tool-independent "agents.md" convention that, among others, Codex reads natively. The project already uses a pattern where `CLAUDE.md` is just a thin pointer to `AGENTS.md`; the same pattern repeats, in a similar style, for other tools (e.g. Cursor and Copilot) with slightly different syntax. Check how this works in your own tool — what matters most is having a one-line pointer there ("read `AGENTS.md`, it's the canonical instructions file"), so the actual content doesn't need to be duplicated, only pointed to.


