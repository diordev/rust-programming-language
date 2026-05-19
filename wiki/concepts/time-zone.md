---
title: "Time Zone"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, chrono, date-time, timezone]
source_count: 1
---

# Time Zone

## Short Definition

Date/time value'ni UTC timeline bilan bog'laydigan zona yoki offset context.

## Why It Matters

Bir xil local clock value turli timezone'larda boshqa instant bo'lishi mumkin.

## Mental Model

Timezone - "13:51 qayerning vaqti?" degan savolga javob.

## Syntax and Examples

```rust
let utc = chrono::Utc::now();
let local = chrono::Local::now();
```

## Common Mistakes

- `NaiveDateTime`da timezone bor deb o'ylash.
- Offset (`+05:00`) bilan full timezone rulesni bir xil deb olish.

## Related Concepts

- [[date-time|DateTime<Tz>]]
- [[utc|UTC]]
- [[local-time|local time]]
- [[fixed-offset|FixedOffset]]

## Sources

- [[wiki/sources/rust-for-backend-developers-date-and-time]]

