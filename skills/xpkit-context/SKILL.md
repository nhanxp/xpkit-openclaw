---
name: xpkit-context
description: XPKit Context Manager — monitor and optimize token usage during long sessions. Use when: checking how much context is left, compacting a long conversation, starting a new major task in an ongoing session, session feels slow or expensive, user says /xpk:context, "check context", "compact", "how much tokens left", "start fresh".
---

# XPKit Context Manager

## /xpk:context check
Report current context status:
- Estimate % of context window used (based on conversation length)
- Flag if > 70%: recommend compact
- Flag if > 90%: strongly recommend compact or new session

## /xpk:context compact
Summarize and reset:

1. **Capture essentials** — what was accomplished, what's in-progress, key decisions
2. **Write summary** to `context-checkpoint.md` in workspace:
```markdown
# Context Checkpoint — [timestamp]

## Completed
- [task 1]
- [task 2]

## In Progress
- [current task + where we left off]

## Key Decisions
- [decision + rationale]

## Next Steps
- [immediate next action]
```
3. **Inform user**: "Đã lưu checkpoint. Bắt đầu session mới và đọc `context-checkpoint.md` để tiếp tục."

## /xpk:context new
Start fresh with context:
- Read `context-checkpoint.md` if exists
- Summarize current state in 3-5 bullets
- Ask: "Tiếp tục [task]?"

## Token Budget Thresholds
| Usage | Action |
|-------|--------|
| < 50% | Normal operation |
| 50-70% | Proactive: avoid re-reading large files |
| 70-90% | Recommend compact before new major task |
| > 90% | Compact immediately or start new session |
