# req-mod-sync

Claude Code plugin that keeps your project's requirements and mental model up to date in `CLAUDE.md`.

Requirements and mental models are critical to track — they're the context Claude works from on every task. When they drift, Claude makes decisions based on stale assumptions. This plugin catches that drift automatically.

## Install

```sh
claude plugin marketplace add https://github.com/elimydlarz/req-mod-sync
claude plugin install req-mod-sync@req-mod-sync --scope project
```

This adds `"req-mod-sync@req-mod-sync": true` to the target project's `.claude/settings.json`. Commit that file to share with your team.

Restart Claude Code after installing.

## What It Does

After every response, you are blocked from stopping and asked to check:

1. Does `## Requirements` in `CLAUDE.md` need updating or creating? (functional and cross-functional requirements)
2. Does `## Mental Model` in `CLAUDE.md` need updating or creating? (key concepts, architecture, relationships, lifecycle)
3. Does the rest of `CLAUDE.md` need updating? (procedures, rules, conventions)

Update whichever need it. If none, reply only with `0`.

## Uninstall

```sh
claude plugin uninstall req-mod-sync@req-mod-sync
```
