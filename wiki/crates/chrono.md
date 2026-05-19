---
title: "chrono"
type: crate
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, crate, date-time]
source_count: 1
---

# chrono

## Short Definition

`chrono` Rustda calendar date/time, timezone-aware `DateTime`, parsing va formatting uchun keng ishlatiladigan crate.

## Cargo Dependency

```toml
[dependencies]
chrono = { version = "0.4", features = ["serde"] }
```

## Basic Usage

```rust
use chrono::{TimeZone, Utc};

let dt = Utc.with_ymd_and_hms(2025, 12, 15, 13, 51, 10).unwrap();
println!("{}", dt.to_rfc3339());
```

## Notes

- Source snapshot `chrono = "0.4"`ni ko'rsatadi; latest version deb da'vo qilinmaydi.
- `serde` feature `chrono` typelarini serialize/deserialize qilish uchun alohida yoqiladi.
- `Naive*` typelar timezone saqlamaydi; `DateTime<Tz>` timezone-aware.

## Related Pages

- [[naive-date|NaiveDate]]
- [[naive-time|NaiveTime]]
- [[naive-date-time|NaiveDateTime]]
- [[date-time|DateTime<Tz>]]
- [[time-zone|time zone]]
- [[wiki/sources/rust-for-backend-developers-date-and-time]]

