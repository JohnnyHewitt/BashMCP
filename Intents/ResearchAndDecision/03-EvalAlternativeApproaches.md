# 3. EvalAlternativeApproaches

**Goal**: Explore approaches beyond hooks and MCP — execve interception, Anthropic's sandbox-runtime, safer permission configuration, prompt-level awareness, or contributing a fix upstream.
**Est.**: ≤2 hours
**Dependencies**: None

## Steps
- [ ] Evaluate OpenAI's `shell-tool-mcp` execve interception — how it works, portability, could it be adapted for Claude Code?
- [ ] Evaluate Anthropic's `sandbox-runtime` — does it actually prevent the prefix-matching bypass or just limit blast radius?
- [ ] Research if Claude Code's permission system can be configured more safely (e.g., exact match mode, disabling wildcard expansion)
- [ ] Explore prompt-level solutions — can CLAUDE.md instructions make the model self-police compound commands?
- [ ] Research whether contributing a fix upstream to anthropics/claude-code is viable (open PRs, contributor guidelines, Anthropic's responsiveness)
- [ ] Explore hybrid approaches — e.g., hooks + sandbox, MCP + prompt awareness
- [ ] Summarize: are there approaches that solve this more fundamentally than hooks or MCP alone?

## Definition of Done
- [ ] Each alternative approach evaluated for viability
- [ ] Hybrid combinations considered
- [ ] Upstream contribution path assessed
- [ ] Verdict: is there a better angle than hooks or MCP?
- [ ] Findings documented in Outcome section

## Outcome (fill after Iterate)
- **Actual Time**:
- **Result**:
- **Follow-ups**:
