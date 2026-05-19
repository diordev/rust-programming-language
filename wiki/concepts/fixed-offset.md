---
title: "FixedOffset"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, chrono, date-time, timezone]
source_count: 1
---

# FixedOffset

## Short Definition

`chrono::FixedOffset` UTCga nisbatan aniq seconds offset bilan timezone context beradi.

## Why It Matters

API'da `+05:00` kabi explicit offset bor timestampni saqlash yoki formatlash kerak bo'lishi mumkin.

## Mental Model

Fixed offset timezone database emas; u faqat UTCdan qancha oldin/keyin ekanini saqlaydi.

## Syntax and Examples

```rust
let offset = chrono::FixedOffset::east_opt(5 * 3600).unwrap();
let dt = chrono::Utc::now().with_timezone(&offset);
```

## Common Mistakes

- Fixed offsetni daylight saving rules bilan to'liq timezone deb o'ylash.
- Offset seconds valid range'dan chiqishi mumkinligini unutish.

## Related Concepts

- [[date-time|DateTime<Tz>]]
- [[time-zone|time zone]]
- [[utc|UTC]]
- [[rfc3339|RFC3339]]

## Sources

- [[wiki/sources/rust-for-backend-developers-date-and-time]]

