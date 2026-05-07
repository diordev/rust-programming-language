---
title: "API Design"
type: pattern
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, api-design, borrowing, modules]
source_count: 4
---

# API Design

## Pattern

Function signature value'dan faqat o'qish uchun foydalanadigan bo'lsa, eng tor va flexible borrowed type'ni qabul qilsin. String input uchun `&String` o'rniga ko'pincha `&str` idiomaticroq.

Module system kontekstida public API'ni ongli tanlang: helper code private qoladi, crate users ishlatishi kerak bo'lgan items esa [[pub-keyword|pub]] bilan public qilinadi.

Internal module structure user mental modeliga mos kelmasa, [[pub-use|pub use]] orqali qulayroq public path re-export qiling.

Validation muhim bo'lgan API'da primitive type o'rniga [[custom-validation-types|custom validation types]] ishlating. Constructor invariantni tekshiradi, private fields esa caller validationni chetlab o'tishiga yo'l qo'ymaydi.

## Why It Matters

`&str` parameter `String`, `&String`, string slice, va string literal bilan ishlaydi. `&String` esa caller'dan aynan heap-backed `String` talab qiladi va API'ni keraksiz toraytiradi.

[[public-api|Public API]] crate users bilan contract. Public function, module, struct field, yoki enum variant keyin o'zgarsa downstream code ta'sirlanadi. Shu sababli [[privacy]] Rustda default bo'lib, public surface explicit tanlanadi.

## Example

```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[..i];
        }
    }

    &s[..]
}
```

Binary + library package patternida binary crate `src/lib.rs`dagi public API'ni external client kabi ishlatadi:

```rust
// src/main.rs
fn main() {
    my_package::run();
}
```

Re-export orqali public pathni soddalashtirish:

```rust
pub use crate::front_of_house::hosting;
```

Validation type:

```rust
pub struct Guess {
    value: i32,
}

impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 || value > 100 {
            panic!("Guess value must be between 1 and 100, got {value}.");
        }

        Guess { value }
    }
}
```

## Related Pages

- [[4-3-the-slice-type-the-rust-programming-language]]
- [[slices]]
- [[string-slice|String slice]]
- [[deref-coercions|deref coercions]]
- [[borrowing]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree-the-rust-programming-language]]
- [[public-api|public API]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[pub-use|pub use]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword-the-rust-programming-language]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[custom-validation-types|custom validation types]]
- [[function-contracts|function contracts]]
- [[invariants]]
- [[9-3-to-panic-or-not-to-panic-the-rust-programming-language]]
