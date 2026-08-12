# AGENTS.md

## Project Overview

Vox Deorum (VD, do not use Vox alone) is LLM-enhanced AI for Civilization V, built on the Community Patch framework.

```
Civ 5 ↔ DLL ↔ Bridge Service ↔ MCP Server ↔ Vox Agents → LLM
     (Named Pipe) (REST/SSE)    (MCP/HTTP)     (LLMs)
```

The system is made up of five components:

| Component | Directory | What it does |
| --- | --- | --- |
| Community Patch DLL | `civ5-dll/` | C++ DLL with named pipe IPC. Build and deploy with `powershell -Command "& .\build-and-copy.bat"` from `civ5-dll/`. |
| Bridge Service | `bridge-service/` | REST/SSE bridge between Civ V and the AI. |
| MCP Server | `mcp-server/` | MCP tools plus SQLite game data access (Kysely). |
| Vox Agents | `vox-agents/` | LLM-powered strategic AI framework. |
| Civ 5 Mod | `civ5-mod/` | Lua hooks and UI for game integration. |

Each component has its own AGENTS.md with detailed patterns. Read it before working in that directory.

## Writing Style

Keep prose plain and easy to follow. Do not document revision history in docs (e.g., instead of X we chose to do Y), unless explicitly instructed to do so.

## Code Rules

- Prioritize simplification and streamlining more than complicating things or adding unnecessary guardrails.
- ESM everywhere: all TS modules use `"type": "module"` with `.js` import extensions.
- npm workspaces: always run `npm install <pkg>` from the repo root, never from a workspace, and keep sub-package `package.json` files minimal. Use `npm install`, `npm run build:all`, and `npm run test:all` from root.
- Vitest for all TypeScript testing.
- Winston logger only: never use `console.log/error/warn` in production code (it is fine in tests).
- camelCase for exported constants (for example, `export const apiKeyFields`).
- Comment everywhere: every function, at least, needs a comment.
- Use the `// Vox Deorum:` prefix for Vox Populi/Community Patch modifications outside CvConnectionService.

## Documentation Rules

Documentation is centralized in `/docs/` and serves two audiences: players (how to play) and developers (what the repo does and how its pieces fit).

- Update docs in the same change that alters behavior, configuration, or setup, and never create docs proactively.
- Keep the detail light. Avoid raw code in docs; describe the behavior and name the source file instead.
- No line-number anchors. They drift, so refer to files, functions, or concepts by name.
- Component `docs/` folders are only for component-specific reference material (for example, `mcp-server/docs/events/`). Don't add new root-level markdown inside components.