# Cache Optimization — Tiết Kiệm Token Tối Đa

## Anthropic Prompt Caching hoạt động thế nào?

```
┌─────────────────────────────────────────────┐
│  EVERY REQUEST                              │
│                                             │
│  [System Prompt]   ← CACHED (tính 1 lần)   │
│  [Skills loaded]   ← CACHED (tính 1 lần)   │
│  [History cache]   ← CACHED (tính 1 lần)   │
│  ─────────────────────────────────────────  │
│  [New message]     ← TỐN TOKEN (mỗi turn)  │
│  [New response]    ← TỐN TOKEN (output)    │
└─────────────────────────────────────────────┘

Cache token = ~10% giá input token thường
→ 90% tiết kiệm trên phần được cache!
```

## 3 Nguyên tắc vàng

### 1. Giữ System Prompt ổn định
Cache chỉ có tác dụng khi nội dung **không thay đổi** giữa các request.

```
✅ Tốt: Skills, SOUL.md, AGENTS.md (ổn định, ít thay đổi)
❌ Xấu: Timestamp động, random seeds trong system prompt
```

**Rule:** Đừng inject thứ gì thay đổi liên tục vào system prompt.

### 2. Đặt nội dung lớn LÊN ĐẦU
Anthropic cache theo prefix — càng nhiều nội dung giống nhau từ đầu → cache hit càng cao.

```
Thứ tự tối ưu:
1. System prompt (skills, persona) — lớn, ổn định
2. Conversation history — trung bình
3. User message mới — nhỏ, thay đổi mỗi turn
```

### 3. TTL 5 phút — Tránh idle quá lâu
Cache tự expire sau ~5 phút không dùng. Nếu bạn để 10 phút rồi hỏi tiếp → cache miss → tốn full token.

```
💡 Nếu cần nghỉ lâu: dùng /xpk:context compact để lưu state
   Rồi open session mới — cache sẽ rebuild ngay turn đầu
```

---

## Kỹ thuật Cache-Aware khi làm việc với file

### Đừng đọc lại file đã có trong context

```
# Lần 1: Đọc file → vào context
read src/auth.js  ✅

# Lần 2: Cùng session, cùng file → SKIP
"Nhớ auth.js ta vừa đọc không?" ✅ (đã trong cache)
read src/auth.js  ❌ (lãng phí — đọc lại thứ đã có)
```

### Batch reads thay vì từng file một

```
# Xấu: 3 round-trips riêng lẻ
read config.js    → response 1
read routes.js    → response 2  
read models.js    → response 3

# Tốt: 1 lượt grep để map, rồi đọc chọn lọc
grep -rn "auth" src/ -l    → biết file nào liên quan
read src/auth/index.js     → chỉ đọc đúng file cần
```

### Dùng offset/limit khi file lớn

```bash
# Thay vì đọc cả file 500 dòng:
read large_file.js                    # ❌ tốn ~2000 tokens

# Grep trước để biết dòng nào:
grep -n "authenticate" large_file.js  # → line 145
# Rồi đọc đúng đoạn:
read large_file.js lines 140-180     # ✅ ~200 tokens
```

---

## Cache Budget Planning

| Phần | Token ước tính | Cache? |
|------|---------------|--------|
| System prompt + skills | ~15-30k | ✅ Luôn cache |
| Conversation history | ~5-20k | ✅ Cache tăng dần |
| File reads trong session | ~2-10k | ✅ Cache trong session |
| User message mới | ~50-200 | ❌ Luôn tính tiền |
| Response mới | ~200-2000 | ❌ Luôn tính tiền |

**Mục tiêu:** Cache hit > 80% → chi phí thực tế giảm 8-10x

---

## Compact Strategy — Khi context đầy

### Khi nào compact?
- Context > 70% → cân nhắc
- Context > 90% → bắt buộc
- Trước task lớn mới (scout + plan + implement)

### Compact đúng cách

```markdown
# Xấu: Chỉ nói "hãy nhớ những gì ta đã làm"
→ Không có file → session mới = mất hết

# Tốt: Ghi ra file trước khi compact
/xpk:context compact
→ Tạo context-checkpoint.md
→ Session mới đọc file đó
→ Tiếp tục ngay, không mất context
```

### Checkpoint format chuẩn

```markdown
# Checkpoint — [date]

## Done ✅
- Implement JWT auth (src/auth/)
- Fix login 500 bug

## In Progress 🔄  
- Unit tests cho AuthService
- Đang ở: src/auth/auth.service.test.ts

## Decisions 📝
- Dùng RS256 thay HS256 (lý do: stateless verification)
- Token TTL: access=15m, refresh=7d

## Next ▶️
- Viết test cho refreshToken()
- Review PR trước khi merge
```

---

## Quick Reference

```
Tiết kiệm nhiều nhất:
✅ Giữ skills ổn định (không sửa liên tục)
✅ Batch file reads
✅ Grep trước, read sau
✅ Không đọc lại file đã có trong context
✅ Compact đúng lúc, dùng checkpoint

Tránh:
❌ Đọc cả file lớn để tìm 1 function
❌ Inject dynamic content vào system prompt
❌ Idle > 5 phút rồi tiếp tục (cache miss)
❌ Re-read files trong cùng session
```
