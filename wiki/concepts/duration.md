---
title: "Duration"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, time, std]
source_count: 1
---

# Duration

## Short Definition

`std::time::Duration` vaqt oralig'ini seconds va nanoseconds aniqligida ifodalaydi.

## Why It Matters

Sleep, timeout, elapsed time, retry delay, va rate-limit kabi backend operatsiyalar vaqt oralig'i bilan ishlaydi.

## Mental Model

`Duration` calendar vaqt emas; u "5 sekund" yoki "25 millisecond" kabi interval.

## Syntax and Examples

```rust
let delay = std::time::Duration::from_secs(60);
std::thread::sleep(delay);
```

## Common Mistakes

- `Duration`ni date/time value deb o'ylash.
- Source'dagi minute constructor kabi mavjud bo'lmagan std API'ni ko'chirish.

## Related Concepts

- [[instant|Instant]]
- [[system-time|SystemTime]]
- [[monotonic-clock|monotonic clock]]

## Sources

- [[wiki/sources/rust-for-backend-developers-date-and-time]]
