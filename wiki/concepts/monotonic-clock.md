---
title: "Monotonic Clock"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, time]
source_count: 1
---

# Monotonic Clock

## Short Definition

Keyingi o'qish oldingi o'qishdan kichik bo'lmasligini kafolatlaydigan timer modeli.

## Why It Matters

Elapsed time measurement system clock backward jump qilganda buzilmasligi kerak.

## Mental Model

Monotonic clock calendar emas; u process yoki OS timing uchun "orqaga yurmaydigan" o'lchov chizig'i.

## Syntax and Examples

```rust
let start = std::time::Instant::now();
let elapsed = start.elapsed();
```

## Common Mistakes

- Monotonic time'ni real timestamp sifatida saqlash.
- Distributed systems vaqt ordering uchun faqat local monotonic clock yetadi deb o'ylash.

## Related Concepts

- [[instant|Instant]]
- [[duration|Duration]]
- [[system-time|SystemTime]]

## Sources

- [[wiki/sources/rust-for-backend-developers-date-and-time]]

