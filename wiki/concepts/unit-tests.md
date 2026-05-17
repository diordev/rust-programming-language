---
title: "Unit Tests"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, testing, unit-tests]
source_count: 2
---

# Unit Tests

## Short Definition

Unit tests — bitta modulni izolyatsiyada, kodning o'zi bilan bir faylda test qiluvchi kichik testlar. `#[cfg(test)]` `mod tests` pattern orqali yoziladi; private funksiyalarni ham test qilish mumkin.

## Why It Matters

Unit tests modulning har bir qismi alohida to'g'ri ishlashini erta va tez tekshiradi. Bug topilganda qaysi modul yoki funksiya muammoli ekanini aniq ko'rsatadi. Integration testlardan tezroq ishlaydi va private implementatsiya detallarga kirish mumkin.

## Mental Model

Unit test — kodning yonida yashovchi nazoratchi. `#[cfg(test)]` tufayli u production binary'ga kirmaydi, lekin `cargo test`da kod bilan birga compile bo'lib, hamma narsani ko'radi — private funksiyalar ham.

`mod tests` child module bo'lgani uchun `use super::*` parent modulning **private** itemlarini ham scopega olib kiradi. Bu Rust privacy modeliga tamomila mos.

## Syntax and Examples

### Standart pattern

```rust
// src/lib.rs
pub fn add_two(a: u64) -> u64 {
    internal_adder(a, 2)
}

fn internal_adder(left: u64, right: u64) -> u64 {
    left + right
}

#[cfg(test)]
mod tests {
    use super::*;  // private itemlar ham ko'rinadi

    #[test]
    fn test_public() {
        assert_eq!(add_two(3), 5);
    }

    #[test]
    fn test_private() {
        assert_eq!(internal_adder(2, 3), 5);  // private funksiya
    }
}
```

### Joylashuv

Har bir `src/` faylidagi module o'zining `#[cfg(test)] mod tests` blokiga ega bo'lishi mumkin:

```
src/
├── lib.rs       → #[cfg(test)] mod tests { ... }
├── models.rs    → #[cfg(test)] mod tests { ... }
└── utils.rs     → #[cfg(test)] mod tests { ... }
```

## Common Mistakes

- `#[cfg(test)]` yozmaslik — test kodi release binary'ga kiradi.
- `use super::*` o'rniga to'liq path yozishga urinish — ortiqcha.
- Private funksiyani test qilish shart deb o'ylash — Rust imkon beradi, lekin majburlamaydi; bu dizayn qaror.
- Assertion o'rniga faqat `println!` bilan signal berish yetadi deb o'ylash — failure mexanizmi `assert!` family va `panic!`.

## Related Concepts

- [[integration-tests]]
- [[testing]]
- [[cfg-test]]
- [[test-macros]]
- [[privacy]]

## Sources

- [[11-3-test-organization]]
- [[wiki/sources/rust-for-backend-developers-testing]]
