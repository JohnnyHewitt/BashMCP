# Research Synthesis & Architecture Decision

> Owner: John Hewitt  ·  Created: 2026-03-14

## Begin (raw)

Claude Code's permission settings are misleading. When a user sees a permission
prompt for `git add .` and clicks "always allow", Claude Code stores
`Bash(git add .*)` — which then auto-approves `git add . && rm -rf /` because
it matches the prefix pattern. Users think they're approving a specific command
but they're approving anything that starts with those characters.

The goal is to find or build a tool that truly fixes this risk, or find a way to
make Claude Code itself more aware of the danger.

### Research findings so far

**The problem is well-documented:**
- 30+ open issues on anthropics/claude-code
- CVE-2025-66032 assigned (Flatt Security research)
- Anthropic's partial fix in v1.0.93 (default allowlist) doesn't fix user-defined rules
- Anthropic's strategic direction is OS-level sandboxing, not fixing the parser
- Exec tool feature request was rejected ("not planned")

**Community hooks-based solutions:**
- `claude-code-bash-guardian` — bashlex AST parsing
- `claude-code-guardian` — rule-based validation, PyPI installable
- `nah` — structural classifier + optional LLM routing
- `claude-code-safety-net` — destructive git/filesystem commands
- Anthropic's own `bash_command_validator_example.py`

**Community MCP-based solutions:**
- `cli-mcp-server` (MladenSU) — command/flag whitelisting, shell operators disabled
- `mcp-shell` (sonirico) — secure mode, no shell parsing
- `shell-tool-mcp` (OpenAI Codex) — intercepts execve(2) via patched Bash binary

**Key constraint discovered:**
MCP cannot directly replace the built-in Bash tool — only supplement it. Built-in
tools are hardcoded in the system prompt and handled by the runtime. An MCP approach
would require CLAUDE.md instructions + blocking built-in Bash via settings.

## Refine (scope)

- **Goal**: Find a tool that truly fixes the prefix-matching risk, or find another
  way to solve it. If existing solutions work, use them. If not, build something.
- **In scope**:
  - Deep evaluation of hooks vs MCP vs other approaches
  - Testing existing community solutions for effectiveness
  - Determining if Claude Code can be made more aware (prompt-level, settings-level)
  - Agreeing on a Phase II course of action
- **Out of scope**:
  - Building a solution (that's Phase II, if needed)
  - Publishing anything yet
- **Definition of Done**: We have a clear, justified decision on what to build/use
  for Phase II, backed by tested evidence.
- **Constraints**:
  - Must work on Linux
  - Must actually prevent the bypass, not just make it harder
  - Prefer solutions that can't be prompted around
