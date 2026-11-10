---
name: connect-three-ws
description: Connect an MCP client (Claude Code, Cursor, VS Code, Codex and others) to the hosted three.ws MCP servers with the @three-ws/cli package, then diagnose a broken setup. Use when the user wants three.ws tools (3D generation, agents, wallets, x402) available in their client.
---

# Connect an MCP client to three.ws

`@three-ws/cli` (binary `three-ws`) signs in, writes the hosted three.ws MCP servers into each client it finds, and verifies each with a live `tools/list`. It needs Node.js 20.12 or newer. It never writes a token into a client config: credentials live in `~/.config/three-ws/credentials.json` (mode 600) and the config gets a `three-ws proxy` launcher.

## Install in one command

```bash
npx @three-ws/cli setup                       # interactive, finds every client
npx @three-ws/cli --claude                    # whole setup for Claude Code, no prompts
npx @three-ws/cli setup --clients cursor,vscode --yes
```

Per-client flags: `--claude --cursor --codex --vscode --windsurf --gemini --bob --hermes`. `--clients` also accepts `claude-desktop`, `grok-bot` and `print`. Add `--project` for project-scoped config (`.mcp.json`, `.cursor/`, `.vscode/`) in the current directory. Running setup again changes nothing.

## Sign-in options

- Browser OAuth with PKCE (default, `--oauth`).
- `--device` approves a code in any browser, for SSH sessions.
- `--key sk_live_...` or env `THREE_WS_API_KEY` uses an API key from the three.ws dashboard.
- `--financial` additionally requests the scopes that let tools spend from an agent wallet. Financial tools stay off unless enabled.
- `three-ws link BCDF-GHJK` links a terminal with a code from the account page.

## Check and fix

```bash
npx @three-ws/cli doctor      # bad config, expired sign-in, unreachable server, with the fix for each
npx @three-ws/cli status      # account, plan, wallet balance, token expiry, configured clients
npx @three-ws/cli mcp list --available
npx @three-ws/cli mcp add <server>
npx @three-ws/cli mcp remove <server>
```

Choose which tool groups a server exposes (tiers: `read`, `write`, `financial`):

```bash
npx @three-ws/cli tools --server three-ws-main --enable financial
npx @three-ws/cli tools --server three-ws-main --reset      # read and write on, financial off
```

Add `--json` for machine-readable output and `--origin <url>` (or `THREE_WS_ORIGIN`) to target another deployment.

## Grok Bot

Grok Bot keeps its MCP config in xAI's cloud, so there is no file to write:

```bash
npx @three-ws/cli setup --client grok-bot                  # prints the connector fields, URL https://three.ws/api/mcp-grok
npx @three-ws/cli setup --client grok-bot --connector-key  # also mints a spend-free key for the Bot secret
```

## Worked example

```bash
npx @three-ws/cli --claude && npx @three-ws/cli doctor
claude "generate a 3D model of a brass telescope"
```

## Money-moving commands

`fund` (move USDC from an agent wallet into inference credits) and `launch --intent` (a funded coin launch) spend real funds. Show the user the recipient, amount and asset, and wait for an explicit yes before running either. `fund` takes `--amount <usdc>` and `--agent <id>`; without `--yes` it prints the transfer and prompts.
