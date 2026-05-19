---
title: "Rust for Backend Developers: Date and Time"
type: chapter
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, backend, date-time, chrono, chapter, advance]
source_count: 1
---

# Rust for Backend Developers: Date and Time

## Learning Goal

Backend code'da elapsed time, system clock, calendar date/time, timezone, va wire-format timestamplarni aralashtirmasdan ishlatish.

## Main Ideas

- `std::time::Duration` vaqt oralig'i, `SystemTime` wall-clock, `Instant` monotonic elapsed timer.
- Code execution duration o'lchash uchun `Instant`, Unix timestamp olish uchun `SystemTime` ishlatiladi.
- `SystemTime` backward/forward siljishi mumkin; benchmark yoki timeout measurement uchun default emas.
- Calendar date/time uchun source [[wiki/crates/chrono|chrono]]ni de-facto crate sifatida beradi.
- `NaiveDate`, `NaiveTime`, `NaiveDateTime` timezone'siz model; `DateTime<Tz>` timezone-aware model.
- `Utc`, `Local`, va `FixedOffset` turli timezone strategiyalarini bildiradi; public API'da explicit offset yoki UTC aniqroq.
- `to_rfc3339` API timestamp uchun ko'p hollarda qulay wire format.

## Concepts

- [[duration|Duration]]
- [[system-time|SystemTime]]
- [[instant|Instant]]
- [[monotonic-clock|monotonic clock]]
- [[unix-time|Unix time]]
- [[naive-date|NaiveDate]]
- [[naive-time|NaiveTime]]
- [[naive-date-time|NaiveDateTime]]
- [[date-time|DateTime<Tz>]]
- [[time-zone|time zone]]
- [[utc|UTC]]
- [[local-time|local time]]
- [[fixed-offset|FixedOffset]]
- [[rfc3339|RFC3339]]
- [[rfc2822|RFC2822]]

## Examples

```rust
let started = std::time::Instant::now();
let elapsed = started.elapsed();
```

```rust
let now = std::time::SystemTime::now()
    .duration_since(std::time::SystemTime::UNIX_EPOCH)?
    .as_secs();
```

```rust
let utc = chrono::Utc
    .with_ymd_and_hms(2025, 12, 15, 13, 51, 10)
    .unwrap();
```

## Exercises

- `Instant` bilan function elapsed time'ini o'lchang.
- `SystemTime`dan Unix timestamp oling va error case haqida yozing.
- `NaiveDate` va `DateTime<Utc>` farqini bitta misolda ko'rsating.
- RFC3339 timestamp parse/format round-trip qiling.

## Review Questions

- `SystemTime` nega elapsed timing uchun noto'g'ri default?
- `NaiveDateTime` qaysi ma'lumotni saqlamaydi?
- `Utc`, `Local`, va `FixedOffset` backend API uchun qanday tradeoff beradi?
- `chrono` typelarini serde bilan ishlatish uchun qaysi feature kerak?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-date-and-time]]
- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/crates/chrono]]
- [[duration]]
- [[instant]]
- [[system-time]]

