---
name: bilinc-shared-memory
description: Use Bilinc, the memory Cursor shares with your other AI agents (Claude, ChatGPT, Codex and more). Recall earlier decisions before working, save decisions the user confirms, correct a stored fact without losing the old version, and compare or undo memory changes with checkpoints.
---

# Bilinc shared memory

Bilinc is one memory that several AI agents share. Cursor, Claude, ChatGPT, Codex and other MCP clients read and write the same facts. Every change is dated, shows which agent or app wrote it, and can be undone.

This skill tells the agent when to use that memory and how to use it safely. It does not contain the memory itself: the user's own Bilinc workspace does.

## Connection

This plugin adds the `bilinc` MCP server (`https://mcp.bilinc.space/mcp`). It signs in with OAuth in the browser, so no API key goes into any file. The user chooses read-only or read and write when they approve the connection. A free plan is available at https://bilinc.space (2,000 operations a month, no card).

If the `bilinc` tools are not available, tell the user that Bilinc is not signed in yet: open Cursor's MCP settings and select Login next to `bilinc`. Do not try to reach Bilinc any other way.

## Tools

- `recall`: search memories with a plain-language query. Each result has a version, its source and when it last changed.
- `remember`: store a new memory under a key that does not exist yet. It never overwrites.
- `revise`: change an existing memory. Pass `expected_version` from `recall` so a newer value is never overwritten.
- `forget`: take a memory out of active recall, with a reason. Its history stays.
- `create_snapshot` / `list_snapshots`: save and list checkpoints of the whole workspace.
- `diff`: show what changed since a checkpoint, with before and after values.
- `preview_rollback` / `rollback`: see exactly what restoring a checkpoint would change, then restore it with the preview's confirmation token.
- `status`: show the workspace, the plan and whether this connection can write.

## When to use it

1. **Before starting work** on a project, task or person the user has mentioned before: `recall` the relevant decisions and facts first, and say briefly what you found.
2. **When the user confirms a decision or fact** ("we launch on the 12th", "use Postgres, not SQLite", "never touch the migrations folder"): `recall` to check for an existing entry, then `remember` (new) or `revise` (changed).
3. **When two sources disagree** (a forwarded draft says one date, the memory says another): do not silently overwrite. Show the user both values and their sources, and `revise` only after they choose.
4. **Before a risky batch of changes** to memory: `create_snapshot` first.
5. **When the user asks what changed or wants to undo**: `diff` against a checkpoint, `preview_rollback`, show the preview, and `rollback` only after the user agrees.
6. **At the end of a work session**, if the user wants it: save a short outcome (what was decided, what was verified, open questions) under a stable key.

## Rules

- Recall before you write, so a new memory does not duplicate or contradict an existing one.
- Use stable, descriptive keys, for example `project.<name>.decision.<topic>`.
- Store decisions and facts the user confirmed, not guesses or your own assumptions.
- Revise, forget or roll back only when the user asks or agrees.
- Never store secrets, passwords, API keys, tokens, cookies or private personal data in memory.
- When you use a recalled fact, say it came from Bilinc and how old it is if that matters.

## Network and data

This skill makes network calls only through the user's own `bilinc` MCP connection to `https://mcp.bilinc.space/mcp` (operated by ReARC Labs Inc.). Memories are stored in the user's Bilinc workspace. The skill itself sends no data anywhere else, contains no scripts and asks for no credentials. Privacy policy: https://bilinc.space/privacy

## Example

User: "We decided on the call that Harbor 2.0 launches on November 12. Remember that."

1. `recall` "Harbor 2.0 launch date": an entry says November 19, source: a forwarded draft.
2. Tell the user: "Bilinc has November 19 from a forwarded draft. You're saying the call set November 12. Should I replace it?"
3. After the user agrees: `revise` the entry to November 12 with `expected_version` from step 1. The old value stays in history and can be restored.
