# XPKit for OpenClaw

**Smart development skills for OpenClaw** — inspired by [XPClaudeKit](https://github.com/nhanxp/xpclaudekit), optimized for token efficiency and OpenClaw workflows.

## Skills

| Skill | Description |
|-------|-------------|
| `xpkit` | Main orchestrator — routes commands, full pipelines |
| `xpkit-scout` | Codebase mapper — rapid exploration with minimal tokens |
| `xpkit-context` | Token manager — monitor & compact context window |

## Commands

| Command | Description |
|---------|-------------|
| `/xpk:cook [feature]` | End-to-end pipeline: plan → code → test → review → commit |
| `/xpk:plan [feature]` | Research + implementation plan |
| `/xpk:scout [keyword]` | Explore codebase, map dependencies |
| `/xpk:fix [bug]` | Debug → root cause → fix → verify |
| `/xpk:review` | Code quality + security audit |
| `/xpk:think [problem]` | Step-by-step reasoning |
| `/xpk:brainstorm [problem]` | 3+ options with trade-offs |
| `/xpk:research [topic]` | Multi-source research |
| `/xpk:context` | Check/compact token usage |

## Install

```bash
# Copy skills into OpenClaw skills directory
cp -r skills/xpkit ~/.openclaw/skills/
cp -r skills/xpkit-scout ~/.openclaw/skills/
cp -r skills/xpkit-context ~/.openclaw/skills/
```

Then restart OpenClaw — skills are auto-detected.

## Token Efficiency

- Scout first, read second (avoid loading full files blindly)
- Cache-aware design (system prompt cached at 99% hit rate)
- Context manager prevents runaway sessions
- Reference files loaded on-demand, not upfront

## Related

- [XPClaudeKit](https://github.com/nhanxp/xpclaudekit) — original Claude Code version (32 skills, 15 agents)
- [OpenClaw](https://openclaw.ai) — AI assistant platform

---

MIT License · by [NhanXP](https://nhanxp.com)
