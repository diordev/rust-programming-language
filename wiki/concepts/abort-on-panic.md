---
title: "Abort on Panic"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, panic, cargo]
source_count: 1
---

# Abort on Panic

## Short Definition

Abort on panic - panic bo'lganda [[stack-unwinding|stack unwinding]] qilmasdan programni darhol tugatish strategy.

## Why It Matters

Abort binary hajmini kichraytirishi va panic paytidagi cleanup workni olib tashlashi mumkin. Buning evaziga Rust stack bo'ylab cleanup qilib chiqmaydi; ishlatilgan memoryni operating system qaytarib oladi.

## Mental Model

Unwinding "ortga yurib yig'ishtirish", aborting esa "darhol to'xtash". Ikkalasi ham [[panic|panic!]]dan keyin normal recovery emas; recoverable errors uchun [[result|Result<T, E>]] kerak.

## Syntax and Examples

Release profile uchun:

```toml
[profile.release]
panic = 'abort'
```

Bu setting [[cargo-toml|Cargo.toml]] ichida yoziladi.

## Common Mistakes

- `panic = 'abort'` error handlingni yaxshilaydi deb o'ylash; u faqat panic response strategy'ni o'zgartiradi.
- Abortni recoverable error handling bilan adashtirish.
- Cleanup kerak bo'lgan programlarda abort oqibatlarini hisobga olmaslik.

## Related Concepts

- [[panic|panic!]]
- [[stack-unwinding|stack unwinding]]
- [[cargo-toml|Cargo.toml]]
- [[release-build|release build]]
- [[error-handling]]

## Sources

- [[9-1-unrecoverable-errors-with-panic]]
