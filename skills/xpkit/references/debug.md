# XPKit Debug & Fix Workflow

## /xpk:fix [bug description]

### Step 1: Reproduce
- Confirm the bug exists with minimal test case
- Note exact error message, stack trace, inputs

### Step 2: Root Cause (5 Whys)
```
Error: [X]
Why 1: Because [A]
Why 2: Because [B]
Why 3: Because [C] ← actual root cause
```

### Step 3: Fix
- Make the minimal change that fixes root cause
- Avoid "shotgun" fixes (changing many things at once)
- Prefer surgical edits over rewrites

### Step 4: Verify
- Re-run the failing case
- Check for regressions in related paths
- If tests exist: run them

### Step 5: Commit
```
fix: [short description]

Root cause: [1 sentence]
Fix: [1 sentence]
```

### Common Patterns

**Import/module errors** → Check paths, exports, circular deps  
**Async/await bugs** → Check Promise chains, missing awaits  
**Type errors** → Check data shape at boundary (API, DB)  
**Race conditions** → Check shared state, add locks/queues
