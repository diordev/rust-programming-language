---
title: "Panic Hook"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, panic, debugging]
source_count: 1
---

# Panic Hook

## Short Definition

Panic sodir bo'lganda unwind/capture'dan oldin ishlaydigan callback.

## Why It Matters

Custom logging, telemetry, yoki output formatlash uchun kerak bo'lishi mumkin.

## Mental Model

`std::panic::set_hook` orqali o'rnatiladi. `catch_unwind` panic'ni ushlasa ham hook oldin ishlaydi.

## Syntax and Examples

```rust
std::panic::set_hook(Box::new(|info| {
    eprintln!("{info}");
}));
```

## Common Mistakes

- `catch_unwind` hook output'ni avtomatik o'chiradi deb o'ylash.

## Related Concepts

- [[panic]]
- [[catch-unwind]]
- [[backtrace]]

## Sources

- [[wiki/sources/rust-for-backend-developers-panic]]

