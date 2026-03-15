<!-- Built with ClaudeTemplate - see ClaudeTemplate.md -->
<!-- WARNING: Do not remove. Enables template features (/brain, /commit, etc.) -->

# Project Context

## Project Structure

- **Source code**: `src/`
- **Tests**: none yet
- **Config files**: `.claude/safe-bash.json` (planned)
- **Generated artifacts**: none yet

## Language & Tooling

- **Language**: TBD (pending research — likely TypeScript or Python)
- **Framework**: MCP SDK
- **Build**: TBD
- **Test**: TBD
- **Package manager**: TBD
- **Target platform**: Linux

## Build & Test Entry Points

These are the approved commands. Do not invent alternatives.

- Build: TBD
- Test: TBD
- Lint: TBD

## Intent Management

Intents are stored under `Intents/{FeatureName}/` with numbered intent files.
See `/brain` command for the BRAIN workflow.

## Constraints

- Must work on Linux first
- Language/toolchain decision deferred until research is complete
- MCP approach preferred over hooks for airtight control
- Must be able to replace Claude Code's built-in bash tool, not just supplement it
