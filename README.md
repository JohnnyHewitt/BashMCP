# Safe-Bash MCP

An MCP server that replaces Claude Code's bash tool with atomic command validation, fixing the prefix-matching security vulnerability in bash permissions.

## Problem

Claude Code's bash permission system uses prefix matching on the entire command string. This means `git add . && rm -rf /` gets approved when users think they're approving `git add .`.

## Solution

An MCP server that intercepts all bash commands, parses chaining operators, validates each segment atomically against configurable allowlists/blocklists, and blocks the entire command if any segment fails validation.

## Status

**Research phase** — investigating existing solutions and Claude Code internals before implementation.

## Development

This project uses the [HelpIRL Claude Template](https://github.com/HelpIRL/ClaudeTemplateV1)
for AI-assisted development. See `ClaudeTemplate.md` for details.

### Commands

- `/brain` — Scaffold a new feature with intent breakdown
- `/commit` — Smart commit with conventional format
- `/review` — Code review
- `/status` — Project status overview

## License

MIT
