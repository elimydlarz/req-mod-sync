# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A Claude Code plugin containing a single Stop hook. Every time Claude finishes a response, the hook blocks the stop and asks Claude to check whether `CLAUDE.md` in the target project needs updating — specifically the `## Requirements`, `## Mental Model`, and general working-in-this-system sections. If nothing needs updating, Claude replies with `0` and stops.

## Architecture

This repo serves as both a plugin and its own marketplace (so it can be installed via `claude plugin install`).

- `.claude-plugin/plugin.json` — plugin manifest, points to `./hooks/hooks.json`
- `.claude-plugin/marketplace.json` — marketplace manifest, lists this plugin with `source: "./"` (self-referencing)
- `hooks/hooks.json` — the Stop hook definition, inline shell command
- `.claude/settings.json` — enables the plugin for this project (dogfooding)

The hook logic: read JSON from stdin, check `stop_hook_active` to prevent infinite loops (exit 0 if true), otherwise emit the review prompt to stderr and exit 2 (which blocks the stop and feeds the message back to Claude).

## Dependencies

The hook requires `jq` on the host system.
