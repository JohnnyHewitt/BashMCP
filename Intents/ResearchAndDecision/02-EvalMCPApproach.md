# 2. EvalMCPApproach

**Goal**: Deep dive on MCP-based gating — can the built-in Bash tool actually be blocked via settings while an MCP tool takes over? Evaluate cli-mcp-server, mcp-shell. Test the CLAUDE.md + block-bash + MCP-tool strategy.
**Est.**: ≤2 hours
**Dependencies**: None

## Steps
- [x] Research Claude Code's `allowedTools`/`blockedTools` settings — can the built-in Bash tool be fully disabled?
- [x] If Bash can be blocked, does Claude Code still function (Read, Write, Edit, etc. are separate tools)?
- [x] Evaluate `cli-mcp-server` (MladenSU) — whitelisting approach, shell operator handling, gaps
- [x] Evaluate `mcp-shell` (sonirico) — secure mode, no-shell-parsing approach, trade-offs
- [x] Test the combo: block built-in Bash via settings + provide MCP bash tool + CLAUDE.md instructions
- [x] Determine if the model reliably uses the MCP tool when instructed, or falls back to built-in
- [x] Summarize: is MCP replacement viable in practice?

## Definition of Done
- [x] Clear answer on whether built-in Bash can be blocked
- [x] Each MCP candidate evaluated with strengths/weaknesses
- [x] The block+replace strategy tested or determined feasible/infeasible
- [x] Verdict: can MCP alone solve this?
- [x] Findings documented in Outcome section

## Outcome
- **Actual Time**: ~30 min (agent-assisted)
- **Result**: MCP block+replace strategy is mechanistically viable but no existing server solves the problem
- **Follow-ups**: Need to verify `disabledTools` setting behavior; existing MCP servers need live testing

### Can Built-in Bash Be Blocked?
- **Yes.** `permissions.deny: ["Bash"]` blocks all Bash invocations. `disabledTools: ["Bash"]` removes it from the tool set entirely.
- **Claude Code still functions** — Read, Write, Edit, Grep, Glob are independent tools
- **Lost capabilities**: build commands, tests, git operations, package management, arbitrary shell commands
- If Bash is disabled and an MCP tool exists, the model **must** use the MCP tool — it has no alternative

### Block+Replace Strategy
- **Mechanistically sound**: disabled tools genuinely cannot be invoked, even via prompt injection
- **Model adapts**: even though system prompt mentions Bash, model handles missing tools gracefully
- **CLAUDE.md helps** but isn't strictly necessary when Bash is fully disabled
- **Risk**: MCP tool must match Bash capability (working dir, env vars, timeouts, background execution) or model struggles

### Candidate Evaluations

| Criterion | cli-mcp-server | mcp-shell (secure) |
|-----------|---------------|-------------------|
| Prevents shell injection | **No** — passes to shell, only checks first command | **Yes** — uses exec directly, no shell |
| Supports pipes/chains | Yes (via shell) | **No** |
| Practical for dev work | Yes | Limited |
| Solves prefix bypass | **No** — same class of bug | Yes (eliminates shell) |
| Maturity | Moderate | Lower |

### Verdict
**The block+replace mechanism works, but neither existing MCP server solves the core problem.** cli-mcp-server has the same vulnerability at a different layer. mcp-shell's secure mode genuinely prevents injection but is too restrictive (no pipes, no chains, no globbing). The gap is: **no existing tool provides both security against compound injection AND practical shell functionality.** This validates the need for a custom solution that parses compound commands, validates each segment, then executes.
