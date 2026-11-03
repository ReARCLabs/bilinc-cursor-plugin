# Bilinc for Cursor

<img src="assets/logo.svg" alt="Bilinc" width="72" align="right">

A company brain your agents share. Bilinc keeps what your agents learn current, shows who wrote what, and lets you undo it.

Your agent in Cursor forgets yesterday's decisions, and your other agents (Claude, ChatGPT, Codex) never knew them. With this plugin, Cursor reads and writes the same memory they do. Every change is dated, shows which agent or app wrote it, and can be undone.

## What's in the plugin

- **MCP server:** the hosted Bilinc server at `https://mcp.bilinc.space/mcp`. It signs in with OAuth in your browser, so no API key goes into any config file.
- **Skill:** `bilinc-shared-memory` tells the agent when to recall, when to save, and how to correct or undo memory safely.

## Get started

1. Install **Bilinc** from the Cursor Marketplace.
2. In Cursor's MCP settings, select **Login** next to `bilinc` and approve the connection in your browser. You choose read-only or read and write.
3. Ask the agent something like "What did we decide about the billing migration?" or "Remember that we use pnpm in this repo."

No account yet? The free plan has 2,000 operations a month and needs no card: https://bilinc.space

## What the agent can do

| Tool | What it does |
| --- | --- |
| `recall` | Search memories in plain language. Each result shows its version, source and last change. |
| `remember` | Save a new memory under a new key. It never overwrites. |
| `revise` | Change a memory without losing a newer value (`expected_version`). The old value stays in history. |
| `forget` | Take a memory out of recall, with a reason. Its history stays. |
| `create_snapshot`, `list_snapshots` | Save and list checkpoints of the whole workspace. |
| `diff` | Show what changed since a checkpoint, before and after. |
| `preview_rollback`, `rollback` | Preview exactly what a restore would change, then restore it. |
| `status` | Show the workspace, the plan and whether this connection can write. |

The skill asks the agent to recall before it writes, to save only what you confirmed, to show you conflicting values instead of overwriting them, and to never store secrets.

## Works with your other agents

The same workspace connects to Claude (Claude Code, Claude Desktop, claude.ai), ChatGPT, Codex and any MCP client with the same URL. Setup guides: https://bilinc.space/mcp

## Data and privacy

Memories live in your Bilinc workspace, operated by ReARC Labs Inc. This plugin contains no scripts and sends nothing anywhere except through your own `bilinc` MCP connection.

- Privacy policy: https://bilinc.space/privacy
- Terms: https://bilinc.space/terms
- Subprocessors: https://bilinc.space/legal/subprocessors
- Remote MCP docs: https://bilinc.space/docs/remote-mcp

## Local testing

Copy this repository to `~/.cursor/plugins/local/bilinc` and restart Cursor.

## License

MIT © ReARC Labs Inc.
