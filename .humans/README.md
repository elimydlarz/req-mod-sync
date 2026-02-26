# req-mod-sync

A [Claude Code](https://claude.ai/code) plugin that keeps your project documentation honest.

## The Problem

Claude Code works from `CLAUDE.md` as its primary project context — requirements and mental models are the most critical sections because they shape every decision Claude makes. But nothing ensures they stay accurate as the codebase evolves. Requirements drift, mental models go stale, and conventions change without the docs catching up. The result: Claude works from assumptions that no longer match reality.

## The Solution

This plugin adds a stop hook — every time Claude finishes a response, it's asked to review three sections of `CLAUDE.md` before it can stop:

- **Requirements** — what the system does and how it behaves (functional and cross-functional)
- **Mental Model** — how the system works (key concepts, domain objects, architecture, relationships, lifecycle)
- **Everything else** — how to work in the system (procedures, rules, conventions)

If nothing needs updating, Claude says `0` and moves on. If something's out of date, it updates `CLAUDE.md` before finishing.

## Install

Requires [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) and `jq`.

```sh
claude plugin marketplace add https://github.com/elimydlarz/req-mod-sync
claude plugin install req-mod-sync@req-mod-sync --scope project
```

Restart Claude Code after installing. The `--scope project` flag writes to `.claude/settings.json` in your project, which you can commit to share with your team.

## How It Works

The plugin is a single [Stop hook](https://docs.anthropic.com/en/docs/claude-code/hooks) — a shell command that runs when Claude tries to finish responding. It:

1. Reads the hook input JSON from stdin
2. Checks `stop_hook_active` — if `true`, exits cleanly (prevents infinite loops)
3. Otherwise, prints the review prompt to stderr and exits with code 2, which tells Claude Code to block the stop and feed the message back as instructions

That's the entire plugin. You can read the full hook in [`hooks/hooks.json`](hooks/hooks.json).

## Uninstall

```sh
claude plugin uninstall req-mod-sync@req-mod-sync
```
