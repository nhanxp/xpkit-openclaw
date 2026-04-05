# Token Optimization Patterns for OpenClaw

## Core Principle
Every token costs money and latency. Be surgical, not verbose.

## Patterns

### 1. Read Selectively
```
# Bad: read entire file to find one function
read large_file.py

# Good: grep first, then read relevant section
grep -n "def authenticate" auth.py  # → line 45
read auth.py lines 40-80
```

### 2. Plan Output Length
- Simple bug fix → 100-200 tokens
- Feature plan → 200-400 tokens  
- Architecture review → 400-800 tokens
- Never pad with filler, summaries of summaries, or "In conclusion"

### 3. Scout Before Reading
Map file structure first:
```bash
find . -name "*.ts" | head -20
cat package.json | grep -E '"(main|scripts|dependencies)"' -A5
```

### 4. Compact Triggers
Check context when:
- Starting a new major task
- After a long debug session
- Every ~10 turns in a complex feature

### 5. Model Selection
| Task | Model |
|------|-------|
| Quick file read, grep, git msg | Haiku |
| Feature planning, debugging | Sonnet |
| Complex architecture, security review | Sonnet (extended thinking if available) |

### 6. Avoid Token Traps
- Don't re-read files you already have in context
- Don't ask for summaries of summaries
- Don't explain what you're about to do — just do it
- Don't list all possible options — recommend the best one

### 7. Sub-agent Token Isolation
When spawning sub-agents, pass only what they need:
- Relevant file snippets, not full files
- Specific task context, not full conversation history
- Expected output format upfront
