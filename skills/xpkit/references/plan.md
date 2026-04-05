# XPKit Planning Workflow

## /xpk:plan [feature]

### Step 1: Scout (2 min)
- Read key files: README, main entry, relevant modules
- Map what exists, what's missing
- Note dependencies and constraints

### Step 2: Research (parallel when possible)
- Search for best practices / patterns
- Check existing similar implementations in codebase
- Identify risks and edge cases

### Step 3: Plan Output Format
```markdown
## Plan: [Feature Name]

### Context
- Current state: ...
- Goal: ...

### Approach
1. [Step 1] — [file/module affected]
2. [Step 2] — [file/module affected]
...

### Risks
- [risk] → [mitigation]

### Estimated complexity: Low / Medium / High
```

### Token Tips
- Keep plan under 300 words
- Skip obvious steps (no need to document "open file")
- Focus on non-obvious decisions and trade-offs
