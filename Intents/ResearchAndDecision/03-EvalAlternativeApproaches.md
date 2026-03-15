# 3. EvalAlternativeApproaches

**Goal**: Explore approaches beyond hooks and MCP — execve interception, Anthropic's sandbox-runtime, safer permission configuration, prompt-level awareness, or contributing a fix upstream.
**Est.**: ≤2 hours
**Dependencies**: None

## Steps
- [x] Evaluate OpenAI's `shell-tool-mcp` execve interception — how it works, portability, could it be adapted for Claude Code?
- [x] Evaluate Anthropic's `sandbox-runtime` — does it actually prevent the prefix-matching bypass or just limit blast radius?
- [x] Research if Claude Code's permission system can be configured more safely (e.g., exact match mode, disabling wildcard expansion)
- [x] Explore prompt-level solutions — can CLAUDE.md instructions make the model self-police compound commands?
- [x] Research whether contributing a fix upstream to anthropics/claude-code is viable (open PRs, contributor guidelines, Anthropic's responsiveness)
- [x] Explore hybrid approaches — e.g., hooks + sandbox, MCP + prompt awareness
- [x] Summarize: are there approaches that solve this more fundamentally than hooks or MCP alone?

## Definition of Done
- [x] Each alternative approach evaluated for viability
- [x] Hybrid combinations considered
- [x] Upstream contribution path assessed
- [x] Verdict: is there a better angle than hooks or MCP?
- [x] Findings documented in Outcome section

## Outcome
- **Actual Time**: ~30 min (direct research)
- **Result**: Multiple alternative angles exist; hybrid approach is strongest
- **Follow-ups**: Test sandbox-runtime + hooks combo; verify permission docs claims

### OpenAI shell-tool-mcp (execve interception)
- Uses **patched Bash/zsh binaries** that intercept `execve(2)` before process spawning
- Sends each exec request back to MCP server for policy evaluation
- Three actions: allow (run outside sandbox), prompt (human approval), forbidden (block)
- **Always knows the full path** to the program being executed — prevents PATH manipulation, aliases, shell function bypass
- **Most robust approach architecturally** — operates at syscall level, not string parsing
- **Portability concern**: requires custom-built shell binaries, tied to specific bash/zsh versions
- **Could be adapted for Claude Code** but significant engineering effort

### Anthropic sandbox-runtime
- Uses **bubblewrap** (Linux) / **sandbox-exec** (macOS) for OS-level isolation
- Filesystem: deny-by-default writes, always blocks sensitive files (.bashrc, .git/hooks, .gitconfig)
- Network: all access denied by default, routed through proxy with domain allowlists
- Linux uses **seccomp BPF** to block Unix socket creation at syscall level
- **Does NOT prevent the bypass** — it limits blast radius. `rm -rf /` inside sandbox can only delete what sandbox allows
- **Complementary, not a fix** — defense-in-depth layer

### Permission System Configuration
- **Critical finding from docs**: Anthropic claims *"Claude Code is aware of shell operators (like &&) so a prefix match rule like `Bash(safe-cmd *)` won't give it permission to run `safe-cmd && other-cmd`"*
- **But issue #28784 proves** `Bash(cd:*)` still allows `cd /path && python3 script.py` — closed "completed" March 2026 but root issue (#16561, 79 upvotes) is still OPEN
- The `:*` (deprecated) vs ` *` (new) syntax may behave differently — needs testing
- **No exact-match mode exists** beyond specifying the full command without wildcards
- Issue #31523: users accumulate 150+ narrow rules that still don't cover compound commands

### Prompt-Level Solutions
- CLAUDE.md can instruct the model to split compound commands into separate Bash calls
- **Not reliable as primary defense**: prompt injection can override, model compliance varies
- Useful as additional layer but not trustworthy for security

### Upstream Contribution
- Issue #16561 (compound command parsing) is open with 79 upvotes, **no Anthropic response**
- Issue #13371 author claims working implementation with 100% bypass prevention — status unknown
- PR #28294 addresses piped command permissions — still OPEN (Feb 2026)
- **Permission matching code is likely in the closed-source binary**, not the open repo
- Upstream fix is the ideal outcome but Anthropic's strategic direction is sandboxing, not parser fixes
- Contributing is possible but depends on Anthropic's willingness to merge

### Hybrid Approaches
Best strategy is layered defense:
1. **Primary**: MCP server with parse-validate-execute (allowlist-based)
2. **Secondary**: PreToolUse hooks as safety net (catch anything that slips through)
3. **Tertiary**: sandbox-runtime to limit blast radius of any bypass
4. **Supporting**: CLAUDE.md instructions for model self-policing + `disabledTools: ["Bash"]`

### Verdict
No single alternative solves this completely. The **hybrid approach** is strongest: MCP for primary control, hooks for defense-in-depth, sandbox for blast radius limitation. Upstream contribution is worth pursuing in parallel but shouldn't be the plan. The execve interception approach (OpenAI) is the most robust architecturally but requires significant engineering and custom binaries.
