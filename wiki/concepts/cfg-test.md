---
title: "cfg(test) va mod tests"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, testing, modules, attributes]
source_count: 1
---

# cfg(test) va mod tests

## Short Definition

`#[cfg(test)]` — bu attribute faqat `cargo test` ishlayotganda compile qilinadigan kod blokini belgilaydi. `mod tests` — test funksiyalarini to'plovchi ichki module. Ikkalasi birga Rustda standart test module pattern'ini hosil qiladi.

## Why It Matters

Test kodi release binary'ga kirib qolmasligi kerak — u faqat test vaqtida kerak. `#[cfg(test)]` buни kafolatlaydi: `cargo build` yoki `cargo build --release`da test module compile qilinmaydi, faqat `cargo test`da qilinadi.

## Mental Model

`cfg(test)` conditional compilation'dir: `test` feature yoqilganda (ya'ni `cargo test`da) ko'rsatilgan kod compile qilinadi, aks holda — yo'q. Bu unit test kodini production binary'dan ajratib turadi.

`mod tests` — oddiy Rust module. Ichki module bo'lgani uchun tashqi modulning private itemlarini ham ko'ra oladi (Rust privacy qoidalariga ko'ra, child module parent'ning private itemlarini ko'radi). `use super::*` esa tashqi modul public va private itemlarini glob bilan ichki scopega olib kiradi.

## Syntax and Examples

### Standart pattern

```rust
pub fn add(left: u64, right: u64) -> u64 {
    left + right
}

#[cfg(test)]          // faqat `cargo test`da compile qilinadi
mod tests {
    use super::*;     // tashqi moduldan hamma narsani import qilish

    #[test]
    fn it_works() {
        assert_eq!(add(2, 2), 4);
    }

    #[test]
    fn another() {
        assert_eq!(add(5, 3), 8);
    }
}
```

### `use super::*` nima uchun kerak

`tests` ichki module. `add` funksiyasi tashqi modulda. Ichki moduldan tashqi modulning itemlarini ko'rish uchun `use super::*` (yoki `use super::add`) kerak.

Alternativ — to'liq path: `super::add(2, 2)`, lekin `use super::*` qisqaroq.

### Bir nechta test module

Bitta fayl ichida bir nechta `#[cfg(test)]` blok bo'lishi mumkin:

```rust
#[cfg(test)]
mod unit_tests {
    use super::*;
    #[test]
    fn test_one() { ... }
}

#[cfg(test)]
mod integration_style {
    use super::*;
    #[test]
    fn test_two() { ... }
}
```

## Common Mistakes

- `#[cfg(test)]` yozmaslik — test kodi release binary'ga ham kiradi, binary katta bo'ladi.
- `use super::*;` yozmaslik — test module ichidan tashqi funksiyalar ko'rinmaydi.
- `mod tests` ichida helper (non-test) funksiya yozib `#[test]` qo'ymaslik — bu to'g'ri, helper funksiyalar `#[test]`siz yoziladi.

## Related Concepts

- [[testing]]
- [[test-macros]]
- [[module]]
- [[privacy]]
- [[use-declarations]]

## Sources

- [[11-1-how-to-write-tests-the-rust-programming-language]]
