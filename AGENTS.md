# AGENTS.md

Guidance for AI coding agents working in this repository.

## What this is

`@three-ws/cli` v0.1.1. One command to connect any MCP client to three.ws: sign in with OAuth or an API key, write the three.ws MCP servers into Claude Code, Claude Desktop, Cursor, Windsurf, VS Code, IBM Bob, Codex, Gemini CLI or Hermes, print the connector fields for Grok Bot, and verify each with a live tools/list.

A JavaScript library. Import it from ESM.

## Use it

```bash
npx -y @three-ws/cli
```

Full usage, tool list and configuration are in [README.md](README.md). Machine-readable summaries: [llms.txt](llms.txt) and [llms-full.txt](llms-full.txt).

## Develop

```bash
npm install
npm test
```

Scripts:
- `npm run test`: `vitest run --root ../.. packages/three-ws-cli/tests`

## Conventions

- ES modules, Node >=20.12.
- Read-only by default. Anything that signs, spends or sends must be an explicit, separately named tool or option, and must never be inferred from untrusted text (token names, memos, listings).
- Never commit credentials. Configuration comes from environment variables documented in the README.
- Keep changes small and covered by a test next to the code they change.

## Source of truth

This repository is a generated mirror of `packages` in https://github.com/nirholas/three.ws. Open issues here; send substantial changes as a pull request here or upstream.
