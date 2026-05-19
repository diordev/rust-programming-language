---
title: "NaiveDateTime"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, chrono, date-time]
source_count: 1
---

# NaiveDateTime

## Short Definition

`chrono::NaiveDateTime` date va time kombinatsiyasi, lekin timezone'siz.

## Why It Matters

Ba'zi database schemas timezone'siz datetime saqlaydi, lekin backendda bu exact instant emasligini bilish muhim.

## Mental Model

`NaiveDateTime` "2025-12-15 13:51:10" deydi, lekin bu UTCmi, localmi, yoki boshqa zone ekanini aytmaydi.

## Syntax and Examples

```rust
let dt = chrono::NaiveDate::from_ymd_opt(2025, 12, 15)
    .unwrap()
    .and_hms_opt(13, 51, 10)
    .unwrap();
```

## Common Mistakes

- API timestamp uchun timezone'siz datetime yuborish.
- `naive_utc()` va `naive_local()` conversionlarida context yo'qolishini e'tiborsiz qoldirish.

## Related Concepts

- [[date-time|DateTime<Tz>]]
- [[time-zone|time zone]]
- [[utc|UTC]]
- [[wiki/crates/chrono]]

## Sources

- [[wiki/sources/rust-for-backend-developers-date-and-time]]

