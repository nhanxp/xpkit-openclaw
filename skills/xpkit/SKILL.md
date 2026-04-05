---
name: xpkit
description: XPKit — Smart OpenClaw development orchestrator. A multi-agent development toolkit inspired by XPClaudeKit, optimized for token efficiency and OpenClaw workflows. Use when: (1) planning and implementing features end-to-end, (2) debugging issues systematically, (3) doing code reviews, (4) research + implementation pipeline, (5) user says /xpk, /cook, /plan, /scout, /fix, /review, /think, /brainstorm, or any XPKit command prefix. Routes to the correct sub-workflow automatically.
---

# XPKit — OpenClaw Dev Orchestrator

Tiết kiệm token bằng cách chọn đúng sub-workflow, đúng model, đúng lúc.

## Command Router

| Command | Action |
|---------|--------|
| `/xpk:cook [feature]` | End-to-end: plan → code → test → review → commit |
| `/xpk:plan [feature]` | Research + implementation plan |
| `/xpk:scout [keyword]` | Codebase exploration & mapping |
| `/xpk:fix [bug]` | Debug → fix → verify |
| `/xpk:review` | Code quality + security audit |
| `/xpk:think [problem]` | Step-by-step reasoning |
| `/xpk:brainstorm [problem]` | 3+ options with trade-offs |
| `/xpk:research [topic]` | Multi-source research |
| `/xpk:context` | Token check + compact if needed |

## Core Principles (Token Efficiency)

1. **Scout before coding** — map dependencies first, avoid blind edits
2. **Plan before implementing** — 5-min plan saves 30-min rework
3. **Haiku for lightweight tasks** — research, docs, git messages
4. **Sonnet for complex tasks** — architecture, debugging, reviews
5. **Context check every ~10 turns** — compact when >70% full

## Pipeline: /xpk:cook

```
cook
├→ plan (scout → research ×2)
├→ implement (debug on error → fix → review)
├→ test (debug if fail)
└→ git commit
```

## Sub-workflow Details

See references/ for detailed guides:
- **references/plan.md** — Planning workflow
- **references/debug.md** — Debug & fix workflow  
- **references/review.md** — Code review checklist
- **references/token-tips.md** — Token optimization patterns
- **references/cache-optimization.md** — Bộ nhớ đệm: cache strategy, compact, checkpoint
