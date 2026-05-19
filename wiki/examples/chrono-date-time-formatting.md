---
title: "chrono DateTime formatting"
type: example
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, chrono, date-time, formatting]
source_count: 1
---

# chrono DateTime formatting

`DateTime<Utc>` yaratish va backend wire format uchun RFC3339 string olish.

```rust
use chrono::{TimeZone, Utc};

fn main() {
    let utc = Utc.with_ymd_and_hms(2025, 12, 15, 13, 51, 10).unwrap();

    println!("RFC3339: {}", utc.to_rfc3339());
    println!("RFC2822: {}", utc.to_rfc2822());
    println!("{}", utc.format("%Y-%m-%d %H:%M:%S.%3f %z"));
}
```

## Related

- [[wiki/crates/chrono]]
- [[date-time|DateTime<Tz>]]
- [[utc|UTC]]
- [[rfc3339|RFC3339]]
- [[rfc2822|RFC2822]]

