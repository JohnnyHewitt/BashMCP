# 1. EvalHooksApproach

**Goal**: Deep dive on PreToolUse hooks — how they work mechanically, whether they can be bypassed or prompted around, and evaluate the top community solutions (bash-guardian, nah, claude-code-guardian).
**Est.**: ≤2 hours
**Dependencies**: None

## Steps
- [ ] Research how PreToolUse hooks work in Claude Code (lifecycle, blocking behavior, what data they receive)
- [ ] Determine if hooks can be bypassed (prompt injection, model choosing not to use Bash, alternate tool paths)
- [ ] Evaluate `claude-code-bash-guardian` — approach, parsing quality, coverage, limitations
- [ ] Evaluate `claude-code-guardian` — approach, rule system, coverage, limitations
- [ ] Evaluate `nah` — structural classifier approach, LLM routing, coverage, limitations
- [ ] Evaluate Anthropic's own `bash_command_validator_example.py`
- [ ] Summarize: do any of these fully solve the prefix-matching bypass?

## Definition of Done
- [ ] Clear understanding of hooks lifecycle and bypass potential
- [ ] Each candidate evaluated with strengths/weaknesses
- [ ] Verdict: can hooks alone solve this?
- [ ] Findings documented in Outcome section

## Outcome (fill after Iterate)
- **Actual Time**:
- **Result**:
- **Follow-ups**:
