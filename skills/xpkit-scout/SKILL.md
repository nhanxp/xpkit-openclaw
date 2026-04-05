---
name: xpkit-scout
description: XPKit Scout — rapid codebase mapping with minimal token usage. Use when: exploring an unfamiliar codebase, finding where a feature lives, mapping dependencies before editing, understanding project structure. Triggers on /xpk:scout, "explore codebase", "map the project", "where is [X] implemented", "what does this project do".
---

# XPKit Scout — Codebase Mapper

Goal: maximum insight, minimum tokens. Read structure first, files second.

## Scout Protocol

### Phase 1: Structure (always do this first)
```bash
# Project type detection
ls -la | head -20
cat package.json 2>/dev/null | python3 -m json.tool | head -30
cat pyproject.toml 2>/dev/null | head -20
cat composer.json 2>/dev/null | head -20

# File tree (shallow)
find . -maxdepth 2 -not -path '*/node_modules/*' -not -path '*/.git/*' \
  -not -path '*/dist/*' -not -path '*/__pycache__/*' | sort
```

### Phase 2: Entry Points
Read in order (stop when you understand the flow):
1. `README.md` — 1-2 min scan, grab key sections only
2. Main entry file (index.js / main.py / app.php / etc.)
3. Router/config if it exists

### Phase 3: Targeted Search
For specific keyword:
```bash
grep -rn "[keyword]" --include="*.ts" --include="*.js" \
  --exclude-dir=node_modules --exclude-dir=dist -l | head -10
# Then read only the most relevant files
```

## Scout Report Format
```markdown
## Scout Report: [project name]

**Type:** [Node.js API / Next.js / Laravel / Python CLI / etc.]
**Stack:** [key technologies]

**Structure:**
- `src/` — [purpose]
- `api/` — [purpose]
- [etc., max 6 entries]

**Key files:**
- `src/index.ts` — entry point
- `src/routes/` — API routes
- [etc., max 5 files]

**Dependencies:** [3-5 most important]

**Notes:** [anything surprising or important to know]
```

## Token Budget
- Phase 1: ~200 tokens
- Phase 2: ~300 tokens  
- Phase 3: ~200 tokens (targeted only)
- Report: ~150 tokens
- **Total target: < 900 tokens**
