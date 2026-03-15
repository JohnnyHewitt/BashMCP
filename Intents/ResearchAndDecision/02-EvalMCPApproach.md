# 2. EvalMCPApproach

**Goal**: Deep dive on MCP-based gating — can the built-in Bash tool actually be blocked via settings while an MCP tool takes over? Evaluate cli-mcp-server, mcp-shell. Test the CLAUDE.md + block-bash + MCP-tool strategy.
**Est.**: ≤2 hours
**Dependencies**: None

## Steps
- [ ] Research Claude Code's `allowedTools`/`blockedTools` settings — can the built-in Bash tool be fully disabled?
- [ ] If Bash can be blocked, does Claude Code still function (Read, Write, Edit, etc. are separate tools)?
- [ ] Evaluate `cli-mcp-server` (MladenSU) — whitelisting approach, shell operator handling, gaps
- [ ] Evaluate `mcp-shell` (sonirico) — secure mode, no-shell-parsing approach, trade-offs
- [ ] Test the combo: block built-in Bash via settings + provide MCP bash tool + CLAUDE.md instructions
- [ ] Determine if the model reliably uses the MCP tool when instructed, or falls back to built-in
- [ ] Summarize: is MCP replacement viable in practice?

## Definition of Done
- [ ] Clear answer on whether built-in Bash can be blocked
- [ ] Each MCP candidate evaluated with strengths/weaknesses
- [ ] The block+replace strategy tested or determined feasible/infeasible
- [ ] Verdict: can MCP alone solve this?
- [ ] Findings documented in Outcome section

## Outcome (fill after Iterate)
- **Actual Time**:
- **Result**:
- **Follow-ups**:
