---
title: "UTC"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, chrono, date-time]
source_count: 1
---

# UTC

## Short Definition

UTC global time baseline bo'lib, chrono'da `chrono::Utc` timezone type'i bilan ishlatiladi.

## Why It Matters

Backend storage, logs, va distributed systems uchun UTC local timezone ambiguityni kamaytiradi.

## Mental Model

UTC - local timezone emas; umumiy reference timeline.

## Syntax and Examples

```rust
let now: chrono::DateTime<chrono::Utc> = chrono::Utc::now();
```

## Common Mistakes

- UTC timestampni user-facing local time sifatida ko'rsatish.
- `naive_utc()` natijasida timezone context yo'qolishini unutish.

## Related Concepts

- [[date-time|DateTime<Tz>]]
- [[unix-time|Unix time]]
- [[rfc3339|RFC3339]]
- [[time-zone|time zone]]

## Sources

- [[wiki/sources/rust-for-backend-developers-date-and-time]]

