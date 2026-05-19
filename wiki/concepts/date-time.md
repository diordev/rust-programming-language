---
title: "DateTime<Tz>"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, chrono, date-time, timezone]
source_count: 1
---

# DateTime<Tz>

## Short Definition

`chrono::DateTime<Tz>` timezone-aware date/time value.

## Why It Matters

Backend API, logs, tokens, va scheduled events exact vaqtni timezone/offset bilan ifodalashi kerak bo'lishi mumkin.

## Mental Model

`DateTime<Tz>` date + time + timezone context. `Tz` `Utc`, `Local`, yoki `FixedOffset` kabi type bo'lishi mumkin.

## Syntax and Examples

```rust
use chrono::{TimeZone, Utc};

let dt = Utc.with_ymd_and_hms(2025, 12, 15, 13, 51, 10).unwrap();
```

## Common Mistakes

- `Local`ni server environmentdan mustaqil deb o'ylash.
- `DateTime<FixedOffset>`ni full timezone database deb qabul qilish.

## Related Concepts

- [[time-zone|time zone]]
- [[utc|UTC]]
- [[local-time|local time]]
- [[fixed-offset|FixedOffset]]
- [[rfc3339|RFC3339]]

## Sources

- [[wiki/sources/rust-for-backend-developers-date-and-time]]

