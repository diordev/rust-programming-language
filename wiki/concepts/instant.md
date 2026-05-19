---
title: "Instant"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, time, std]
source_count: 1
---

# Instant

## Short Definition

`std::time::Instant` monotonic timer API'si bo'lib, elapsed time o'lchash uchun ishlatiladi.

## Why It Matters

Timeout, latency measurement, benchmark, va retry timing system clock siljishidan mustaqil bo'lishi kerak.

## Mental Model

`Instant` "hozirgi wall-clock vaqt"ni emas, monotonic timeline'dagi nuqtani bildiradi.

## Syntax and Examples

```rust
let start = std::time::Instant::now();
let took = start.elapsed();
```

## Common Mistakes

- `Instant`ni serialize qilish yoki processlar orasida yuborish mumkin deb o'ylash.
- Calendar timestamp kerak bo'lgan joyda `Instant` ishlatish.

## Related Concepts

- [[monotonic-clock|monotonic clock]]
- [[duration|Duration]]
- [[system-time|SystemTime]]

## Sources

- [[wiki/sources/rust-for-backend-developers-date-and-time]]

