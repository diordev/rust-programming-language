---
title: "Unix Time"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, time]
source_count: 1
---

# Unix Time

## Short Definition

1970-01-01 00:00:00 UTC Unix epochdan beri o'tgan vaqt, ko'pincha sekundlarda ifodalanadi.

## Why It Matters

Logs, tokens, databases, va APIs ko'p hollarda timestampni Unix time sifatida saqlaydi yoki uzatadi.

## Mental Model

Unix time wall-clock timestamp; elapsed measurement emas.

## Syntax and Examples

```rust
let secs = std::time::SystemTime::now()
    .duration_since(std::time::SystemTime::UNIX_EPOCH)?
    .as_secs();
```

## Common Mistakes

- Local timezone bilan Unix timestampni aralashtirish.
- `SystemTime` old epochdan oldin bo'lishi mumkinligini unutish.

## Related Concepts

- [[system-time|SystemTime]]
- [[utc|UTC]]
- [[rfc3339|RFC3339]]

## Sources

- [[wiki/sources/rust-for-backend-developers-date-and-time]]

