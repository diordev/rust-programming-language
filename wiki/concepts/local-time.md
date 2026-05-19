---
title: "Local Time"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, chrono, date-time]
source_count: 1
---

# Local Time

## Short Definition

Process ishlayotgan environmentning local timezone'iga bog'langan date/time.

## Why It Matters

CLI yoki local display uchun foydali bo'lishi mumkin, lekin serverlarda environmentga bog'liqlik API va storage buglariga olib keladi.

## Mental Model

`Local::now()` host machine configurationiga qaraydi.

## Syntax and Examples

```rust
let local: chrono::DateTime<chrono::Local> = chrono::Local::now();
```

## Common Mistakes

- Server local timezone'ini business timezone deb qabul qilish.
- Local time'ni offset/contextsiz API'dan chiqarish.

## Related Concepts

- [[time-zone|time zone]]
- [[utc|UTC]]
- [[date-time|DateTime<Tz>]]

## Sources

- [[wiki/sources/rust-for-backend-developers-date-and-time]]

