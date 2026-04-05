# XPKit Code Review Checklist

## /xpk:review [files?]

Quick review (token-efficient): scan changed files, flag critical issues only.

### Critical (must fix)
- [ ] Security: SQL injection, XSS, exposed secrets, unvalidated input
- [ ] Logic bugs: off-by-one, null deref, wrong conditions
- [ ] Data loss: destructive ops without confirmation
- [ ] Breaking changes: API contract, DB schema

### Important (should fix)
- [ ] Error handling: uncaught exceptions, silent failures
- [ ] Performance: N+1 queries, missing indexes, blocking I/O
- [ ] Resource leaks: unclosed connections, memory leaks

### Nice to have (optional)
- [ ] Naming clarity
- [ ] Dead code removal
- [ ] Missing docs for public APIs

### Output Format
```markdown
## Code Review

### 🔴 Critical
- [file:line] [issue] → [fix suggestion]

### 🟡 Important  
- [file:line] [issue] → [fix suggestion]

### 🟢 LGTM
Overall: [1-sentence summary]
```

### Token Tip
- Read diff/changed files only, not entire codebase
- Focus on logic paths, not style (unless it's a style review)
