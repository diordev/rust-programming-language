---
title: "Дата и время - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, backend, source, date-time, chrono]
source_count: 1
---

# Дата и время - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/3. advance/56. Дата и время.md`
- URL: https://rust-for-backend-developers.github.io/dive-deeper/date-time.html

## Detailed Summary

Bu source Rust standard library'da date/calendar API yo'qligini, lekin `std::time` ichida uchta asosiy time primitive borligini ko'rsatadi: [[duration|Duration]], [[system-time|SystemTime]], va [[instant|Instant]]. Backend code'da bu uchalasi bir xil narsa emas: duration vaqt oralig'i, system time wall-clock vaqt, instant esa elapsed time o'lchash uchun monotonic timer.

`Duration` seconds va nanoseconds asosida vaqt oralig'ini ifodalaydi. Source `Duration::from_secs`, subtraction, `as_secs`, va `thread::sleep(Duration)` kabi ishlatishni ko'rsatadi. Source misolidagi minute constructor std API uchun to'g'ri emas; wiki'da buni `Duration::from_secs(60)` deb tuzatish kerak.

`SystemTime` system clock bilan ishlaydi va Unix epochdan o'tgan vaqtni olish uchun `SystemTime::now().duration_since(SystemTime::UNIX_EPOCH)` patterni ishlatiladi. Muhim caveat: system clock backward yoki forward sozlanishi mumkin; shu sababli code execution time o'lchash uchun `SystemTime` noto'g'ri default.

`Instant` [[monotonic-clock|monotonic clock]] ustidagi API. `Instant::now()` va `elapsed()` code segment qancha vaqt olganini o'lchash uchun ishlatiladi. Source buni system clockdan alohida saqlaydi: `Instant` astronomical vaqt bermaydi, lekin elapsed measurement uchun to'g'riroq.

Date/calendar uchun source de-facto crate sifatida [[wiki/crates/chrono|chrono]]ni beradi. `chrono = { version = "0.4", features = ["serde"] }` dependency `chrono` typelarini [[wiki/crates/serde|serde]] bilan serialize qilishga imkon beradi; `serde` feature alohida yoqiladi.

`NaiveDate`, `NaiveTime`, va `NaiveDateTime` timezone'siz ISO 8601 date/time modellari. `NaiveDate::from_ymd_opt`, `parse_from_str`, `format`, `checked_add_days`, `checked_add_months`, `iter_days` kabi API'lar date parsing, formatting, arithmetic, va iteration uchun ishlatiladi. `NaiveTime` vaqt komponentini, `NaiveDateTime` esa date + time kombinatsiyasini beradi.

`DateTime<Tz>` timezone-aware model. `Tz` generic argument `TimeZone` traitini implement qiladi. Source `Utc`, `Local`, va `FixedOffset`ni ajratadi: `Utc` global baseline, `Local` host timezone, `FixedOffset` esa aniq offset. Backendda storage va API boundary uchun odatda UTC yoki explicit offset afzal; local timezone process environmentga bog'liq.

Formatting bo'limi `format`, `to_rfc3339`, va `to_rfc2822`ni ko'rsatadi. RFC3339 API wire format uchun ko'p hollarda aniqroq default, chunki offsetni ham o'z ichiga oladi. `FixedOffset` bo'limi `+02:30` kabi offset yaratish va `with_timezone` orqali UTC vaqtni fixed offsetga ko'chirishni ko'rsatadi.

## Key Concepts

- [[duration|Duration]]
- [[system-time|SystemTime]]
- [[instant|Instant]]
- [[monotonic-clock|monotonic clock]]
- [[unix-time|Unix time]]
- [[wiki/crates/chrono|chrono]]
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

## Code Examples

```rust
use std::time::{Duration, Instant};

let start = Instant::now();
let took: Duration = start.elapsed();
```

```rust
use std::time::{Duration, SystemTime};

let unix_time_now = SystemTime::now()
    .duration_since(SystemTime::UNIX_EPOCH)
    .unwrap()
    .as_secs();
```

```toml
[dependencies]
chrono = { version = "0.4", features = ["serde"] }
```

```rust
use chrono::{TimeZone, Utc};

let utc = Utc.with_ymd_and_hms(2025, 12, 15, 13, 51, 10).unwrap();
println!("{}", utc.to_rfc3339());
```

## Exercises or Practice Ideas

- `SystemTime` bilan Unix timestamp oling, `Instant` bilan esa code block elapsed time'ini o'lchang.
- `NaiveDate` parse/format va date arithmetic uchun kichik misol yozing.
- `DateTime<Utc>`ni RFC3339 stringga aylantirib, qayta parse qiling.
- `FixedOffset` bilan UTC vaqtni `+05:00` offsetga ko'chiring.

## Questions Raised

- Backend storage uchun `NaiveDateTime` ishlatish qachon xato bo'ladi?
- Public API'da local time yuborish offset yo'qolishiga olib keladimi?
- `SystemTime` bilan `Instant`ni aralashtirish qanday bug beradi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/chapters/rust-for-backend-developers-date-and-time]]
- [[wiki/crates/chrono]]
- [[duration]]
- [[instant]]
- [[system-time]]
