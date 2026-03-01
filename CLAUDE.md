# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A Claude Code plugin that keeps a project's requirements and mental model up to date in `CLAUDE.md`.

Requirements, mental models, and repo maps are critical to track — they're the context Claude works from on every task. If they drift from reality, Claude makes decisions based on stale assumptions. This plugin prevents that by running a Stop hook after every response, blocking the stop to ask Claude whether `## Requirements`, `## Mental Model`, `## Repo Map`, or general working-in-this-system sections of `CLAUDE.md` need updating. If nothing needs updating, Claude replies with `0` and stops.

## Architecture

This repo serves as both a plugin and its own marketplace (so it can be installed via `claude plugin install`).

- `.claude-plugin/plugin.json` — plugin manifest, points to `./hooks/hooks.json`
- `.claude-plugin/marketplace.json` — marketplace manifest, lists this plugin with `source: "./"` (self-referencing)
- `hooks/hooks.json` — the Stop hook definition, inline shell command
- `.claude/settings.json` — enables the plugin for this project (dogfooding)

The hook logic: read JSON from stdin, check `stop_hook_active` to prevent infinite loops (exit 0 if true), otherwise emit the review prompt to stderr and exit 2 (which blocks the stop and feeds the message back to Claude).

## Dogfooding

This project uses itself. After changing hook logic, reinstall so the running session picks up the new hook:

```sh
claude plugin uninstall req-mod-sync@req-mod-sync
claude plugin install req-mod-sync@req-mod-sync --scope project
```

Then restart Claude Code.

## Repo Map

- `CLAUDE.md` — project instructions and context for Claude Code
- `.claude-plugin/plugin.json` — plugin manifest (name, description, version, hooks path)
- `.claude-plugin/marketplace.json` — marketplace manifest for `claude plugin install` (self-referencing)
- `hooks/hooks.json` — Stop hook definition with inline shell command
- `.claude/settings.json` — project settings enabling the plugin (dogfooding)

## Dependencies

The hook requires `jq` on the host system.
