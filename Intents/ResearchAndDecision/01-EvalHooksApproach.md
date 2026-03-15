# 1. EvalHooksApproach

**Goal**: Deep dive on PreToolUse hooks — how they work mechanically, whether they can be bypassed or prompted around, and evaluate the top community solutions (bash-guardian, nah, claude-code-guardian).
**Est.**: ≤2 hours
**Dependencies**: None

## Steps
- [x] Research how PreToolUse hooks work in Claude Code (lifecycle, blocking behavior, what data they receive)
- [x] Determine if hooks can be bypassed (prompt injection, model choosing not to use Bash, alternate tool paths)
- [x] Evaluate `claude-code-bash-guardian` — approach, parsing quality, coverage, limitations
- [x] Evaluate `claude-code-guardian` — approach, rule system, coverage, limitations
- [x] Evaluate `nah` — structural classifier approach, LLM routing, coverage, limitations
- [x] Evaluate Anthropic's own `bash_command_validator_example.py`
- [x] Summarize: do any of these fully solve the prefix-matching bypass?

## Definition of Done
- [x] Clear understanding of hooks lifecycle and bypass potential
- [x] Each candidate evaluated with strengths/weaknesses
- [x] Verdict: can hooks alone solve this?
- [x] Findings documented in Outcome section

## Outcome
- **Actual Time**: ~30 min (agent-assisted)
- **Result**: Hooks cannot fully solve the problem
- **Follow-ups**: Hooks valuable as defense-in-depth layer alongside primary solution

### Hooks Lifecycle
- PreToolUse fires before tool execution, receives tool name + full input (command string) as JSON on stdin
- Exit 0 = allow, Exit 2 = block (with JSON reason on stderr)
- Hooks can only allow/block — **cannot modify commands**
- The hook matcher itself uses the same prefix-matching (ironic)

### Bypass Vectors
1. **Alternate tool paths**: Write a script via Write tool, then run with `bash script.sh`
2. **Command obfuscation**: `eval`, base64 encoding, variable expansion, brace expansion, process substitution
3. **Prompt injection**: Malicious file content can instruct model to use obfuscated forms
4. **Model reformulation**: If blocked, model can try different formulations to evade detection

### Candidate Evaluations

| Tool | Approach | Strengths | Weaknesses |
|------|----------|-----------|------------|
| **bash-guardian** | bashlex AST parsing | Parses real bash grammar, handles `&&`/`;`/`\|` | Can't handle eval, variables, encoded payloads; bashlex itself is niche/undermaintained |
| **claude-code-guardian** | Rule-based validation (PyPI) | Most accessible, easy config | Coverage depends on rule completeness; novel attacks pass through |
| **nah** | Structural classifier + LLM | Catches semantic intent, best at obfuscation | Latency (1-5s per call), cost, reliability; LLM can be wrong |
| **Anthropic example** | Basic pattern matching | Good reference for hooks API | Example only, not a solution |

### Verdict
**Hooks alone cannot solve this.** Bash is Turing-complete — blocklist approaches face an infinite problem space. An allowlist approach (only permit known-safe commands) is fundamentally stronger. Hooks are valuable as a **secondary defense layer** but not as the primary solution.
