---
title: "Bad State"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, error-handling, panic]
source_count: 1
---

# Bad State

## Short Definition

Bad state - code tayanayotgan assumption, guarantee, [[function-contracts|contract]], yoki [[invariants|invariant]] buzilgan holat.

## Why It Matters

Bad state'da code davom etsa, noto'g'ri result, security vulnerability, yoki boshqa harmful behavior yuzaga kelishi mumkin. Bunday holatda [[panic|panic!]] qilish recoverable error qaytarishdan ko'ra to'g'riroq bo'lishi mumkin.

## Mental Model

Bad state "bu joyga kelmasligimiz kerak edi" degani. Agar keyingi code bad state yo'qligiga tayanishi kerak bo'lsa, har qadamda tekshirib yurish o'rniga invariantni buzilgan joyda to'xtatish yaxshiroq bo'lishi mumkin.

## Syntax and Examples

```rust
pub fn new(value: i32) -> Guess {
    if value < 1 || value > 100 {
        panic!("Guess value must be between 1 and 100, got {value}.");
    }

    Guess { value }
}
```

Bu yerda invalid `Guess` yaratish bad state hisoblanadi.

## Common Mistakes

- Har bir expected failure'ni bad state deb belgilash.
- Caller recover qila oladigan error uchun panic qilish.
- Bad state shartlarini API documentationda yozmaslik.

## Related Concepts

- [[panic|panic!]]
- [[panic-vs-result|panic! vs Result]]
- [[invariants]]
- [[function-contracts|function contracts]]
- [[custom-validation-types|custom validation types]]

## Sources

- [[9-3-to-panic-or-not-to-panic-the-rust-programming-language]]
