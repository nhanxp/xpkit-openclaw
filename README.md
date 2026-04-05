# XPKit OpenClaw

> **Sử dụng OpenClaw nhanh hơn, tiết kiệm hơn** — dành cho Claude Extra Usage

Bộ skills tối ưu cho OpenClaw, lấy cảm hứng từ [XPClaudeKit](https://github.com/nhanxp/xpclaudekit). Thiết kế xoay quanh một nguyên tắc: **cache hit cao = chi phí thấp**.

---

## Tại sao XPKit?

```
Không dùng XPKit:          Dùng XPKit:
Cache hit: ~60%            Cache hit: >90%
Token/turn: ~3000          Token/turn: ~300
Chi phí: $$$               Chi phí: $
```

Anthropic tính cache token rẻ hơn ~10x so với input token thường.  
XPKit được thiết kế để **tối đa hóa cache hit** trong mỗi session.

---

## Skills

| Skill | Mô tả |
|-------|-------|
| `xpkit` | Orchestrator chính — command router, full pipelines, cache-aware workflows |
| `xpkit-scout` | Codebase mapper — khám phá project với < 900 tokens |
| `xpkit-context` | Token manager — monitor context, compact đúng lúc, checkpoint |

---

## Commands

| Command | Mô tả |
|---------|-------|
| `/xpk:cook [feature]` | Pipeline đầy đủ: plan → code → test → review → commit |
| `/xpk:plan [feature]` | Research + implementation plan |
| `/xpk:scout [keyword]` | Khám phá codebase, map dependencies |
| `/xpk:fix [bug]` | Debug → root cause → fix → verify |
| `/xpk:review` | Code quality + security audit |
| `/xpk:think [problem]` | Step-by-step reasoning |
| `/xpk:brainstorm [problem]` | 3+ options với trade-offs |
| `/xpk:research [topic]` | Multi-source research |
| `/xpk:context` | Check/compact token usage |

---

## Cài đặt

```bash
git clone https://github.com/nhanxp/xpkit-openclaw.git

cp -r xpkit-openclaw/skills/xpkit ~/.openclaw/skills/
cp -r xpkit-openclaw/skills/xpkit-scout ~/.openclaw/skills/
cp -r xpkit-openclaw/skills/xpkit-context ~/.openclaw/skills/
```

Restart OpenClaw — skills được tự động nhận diện.

---

## Tối ưu bộ nhớ đệm (Cache Strategy)

XPKit thiết kế để tận dụng **Anthropic Prompt Caching**:

### Nguyên tắc hoạt động
```
Mỗi request:
[System prompt + Skills]  ← CACHED (trả 1/10 giá)
[Conversation history]    ← CACHED (tăng dần)
──────────────────────────────────────────
[Tin nhắn mới]            ← Tính full price
[Response mới]            ← Tính full price (output)
```

### 3 thói quen tiết kiệm token

**1. Grep trước, read sau**
```bash
# Thay vì đọc cả file → grep tìm vị trí → đọc đúng đoạn
grep -n "authenticate" src/auth.js  # → line 45
# Rồi đọc offset 40-80 thay vì cả file
```

**2. Không đọc lại file đã có trong context**
```
Lần 1: read config.js → vào cache ✅
Lần 2: cùng session → SKIP, đã có trong context ✅
```

**3. Compact đúng lúc với checkpoint**
```
/xpk:context compact → lưu state vào context-checkpoint.md
→ Session mới đọc file → tiếp tục ngay, không mất gì
```

Xem chi tiết: [`skills/xpkit/references/cache-optimization.md`](skills/xpkit/references/cache-optimization.md)

---

## Workflow chuẩn

```
# Feature mới
/xpk:scout              ← map project (< 900 tokens)
/xpk:plan "feature"     ← lên kế hoạch
→ approve
/xpk:cook "feature"     ← thực thi end-to-end

# Fix bug
/xpk:fix "mô tả lỗi"   ← tự động: debug → fix → verify

# Trước task lớn
/xpk:context            ← check token còn bao nhiêu
```

---

## Related

- [XPClaudeKit](https://github.com/nhanxp/xpclaudekit) — phiên bản gốc cho Claude Code (32 skills, 15 agents)
- [OpenClaw](https://openclaw.ai) — AI assistant platform

---

MIT License · by [NhanXP](https://nhanxp.com)
